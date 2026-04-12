# PrintsbyTee — Payment Integration Plan (MVP+1)

> **Project:** PrintsbyTee — Afro-Luxe Fashion E-Commerce
> **Stack:** Next.js + TypeScript + Tailwind CSS
> **Site:** printsbytee.co.uk
> **Stage:** MVP shipped → MVP+1 planning
> **Owner:** Dami
> **Document version:** 1.0 — 2026-04-08

---

## 1. Executive Summary

This document outlines the recommended payment integration strategy for PrintsbyTee's MVP+1 sprint. After evaluating the major UK payment providers — Stripe, PayPal, and Square — **Stripe is recommended as the primary payment provider**, with PayPal offered as a secondary option at checkout.

**Why Stripe for PrintsbyTee:**
- Most competitive UK card fees (1.5% + £0.20 for domestic)
- Excellent Next.js/TypeScript integration story
- Automatic SCA/3DS2 compliance
- Webhook-driven order confirmation
- Strong dashboard and dispute management
- No monthly fees — pay-per-transaction

**Why also offer PayPal:**
- Higher trust among certain customer segments (especially diaspora buyers unfamiliar with UK checkout flows)
- PayPal Checkout SDK integrates easily alongside Stripe
- No additional PCI burden — PayPal handles card data

---

## 2. Payment Provider Comparison

### 2.1 Stripe

| Attribute | Detail |
|---|---|
| **UK card fees** | 1.5% + £0.20 per transaction (Visa, Mastercard) |
| **EU/EEA card fees** | 2.5% + £0.20 |
| **International card fees** | 3.25% + £0.20 |
| **Amex (UK)** | 2.0% + £0.20 |
| **Currency conversion** | +2% if settling in non-base currency |
| **Payout timing** | Standard: 2 business days. Instant: 1% fee (min 50p) |
| **Chargeback fee** | £20 per dispute (win or lose) |
| **Refund fees** | Original processing fee is NOT refunded |
| **Setup requirements** | UK company details, bank account, identity verification |
| **PCI compliance** | Stripe handles most; you're SAQ-A or use Stripe Elements (lowest burden) |
| **Integration complexity** | Low. Stripe has official Next.js + TypeScript SDK and documentation. |
| **Dashboard quality** | Excellent — real-time events, disputes, revenue, refunds |
| ** SCA / 3DS2** | Handled automatically by Stripe; low-risk transactions exempt automatically |

**Verdict for PrintsbyTee:** ✅ Best primary provider. Most competitive UK fees, developer-friendly, well-suited for B2C fashion.

### 2.2 PayPal

| Attribute | Detail |
|---|---|
| **UK card fees (via PayPal)** | 2.9% + £0.30 per transaction |
| **EU card fees** | 2.9% + £0.30 |
| **International card fees** | 4.4% + fixed currency fee |
| **Extra: Pay in 3** | Available (interest-free instalments) — potential conversion boost |
| **Payout timing** | Immediate to PayPal balance; 1-3 days to bank |
| **Chargeback fee** | £14 per dispute (lower than Stripe) |
| **Setup requirements** | Business account, bank account, verification |
| **PCI compliance** | Minimal — PayPal holds card data |
| **Integration complexity** | Low-medium. PayPal JS SDK, but more overhead than Stripe Elements |
| **Customer trust** | High — especially valuable for first-time or diaspora customers |
| ** SCA / 3DS2** | Handled by PayPal |

**Verdict for PrintsbyTee:** ✅ Good secondary option to offer alongside Stripe. Customers who don't fully trust a new brand checkout can use PayPal. Pay in 3 could help average order value.

### 2.3 Square

| Attribute | Detail |
|---|---|
| **UK online card fees** | 1.4% + £0.25 per transaction (UK cards) |
| **Non-UK card fees** | 2.5% + £0.25 |
| **Payout timing** | 1-2 business days (standard); instant for extra fee |
| **Setup requirements** | Business verification, UK bank account |
| **PCI compliance** | SAQ-A via Square-hosted fields or use Square API |
| **Integration complexity** | Medium. Square has an online store product; API integration is less streamlined than Stripe |
| **Best for** | Businesses with physical POS needs |

**Verdict for PrintsbyTee:** ❌ Not recommended as primary. Square's strength is physical POS; Stripe is better for pure-play e-commerce with stronger developer tooling.

### 2.4 Other Providers Worth Knowing

| Provider | Best For | Notes |
|---|---|---|
| **Adyen** | High-volume businesses (>£100k/month) | Custom pricing, complex integration, overkill for MVP+1 |
| **GoCardless** | Subscription/Direct Debit | Not ideal for one-off fashion purchases |
| **Klarna / ClearPay** | Buy Now Pay Later | Consider adding later as a conversion tool; fees ~3% per transaction |
| **Airwallex** | Businesses with heavy international volume | Better currency conversion rates than Stripe for non-UK cards |

---

## 3. Recommended Architecture

### 3.1 Provider Stack

```
Primary:   Stripe Checkout / Stripe Elements
Secondary: PayPal Buttons (rendered alongside Stripe)
Future:    Klarna / ClearPay (BNPL) — consider for MVP+2
```

### 3.2 What This Means for the Codebase

```
printsbytee/
├── app/
│   ├── (shop)/           ← existing shop routes
│   │   ├── cart/         ← cart page
│   │   └── checkout/     ← NEW: checkout page
│   └── api/
│       ├── webhooks/
│       │   └── stripe/   ← NEW: Stripe webhook handler
│       └── orders/       ← NEW: order creation/confirmation API
├── lib/
│   ├── stripe.ts         ← NEW: Stripe client + helpers
│   └── paypal.ts         ← NEW: PayPal client (optional)
├── hooks/
│   └── use-cart.ts       ← existing or updated
└── components/
    ├── checkout/         ← NEW: checkout components
    │   ├── StripePaymentForm.tsx
    │   ├── PayPalButton.tsx
    │   ├── OrderSummary.tsx
    │   └── AddressForm.tsx
    └── cart/             ← existing cart components
```

---

## 4. Implementation Steps

### Phase 1: Stripe Setup (Start Here)

#### Step 1.1: Create Stripe Account
1. Go to [stripe.com/uk](https://stripe.com/uk) → Create account
2. Complete business verification (company name, address, UK bank account)
3. Retrieve **Publishable Key** and **Secret Key** from the Stripe Dashboard → Developers → API keys
4. Enable test mode first; switch to live when ready

#### Step 1.2: Install Stripe Dependencies

```bash
npm install @stripe/stripe-js @stripe/react-stripe-js stripe
```

#### Step 1.3: Environment Variables

Add to `.env.local` (create if not exists; add `.env.local` to `.gitignore`):

```env
# Stripe
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# Optional: Stripe Connect (for marketplace — not needed for standard PrintsbyTee)
# STRIPE_ACCOUNT_ID=acct_...

# PayPal (if adding as secondary)
NEXT_PUBLIC_PAYPAL_CLIENT_ID=...
PAYPAL_SECRET_KEY=...
PAYPAL_WEBHOOK_ID=...
```

> ⚠️ **Never commit Stripe secret keys to version control.** Use `.env.local` or a secrets manager.

#### Step 1.4: Stripe Client Setup (`lib/stripe.ts`)

```typescript
import Stripe from 'stripe';
import { loadStripe, Stripe as StripeJS } from '@stripe/stripe-js';

export const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, {
  apiVersion: '2025-02-24.acacia', // Use latest stable version — check Stripe dashboard
  typescript: true,
});

let stripePromise: Promise<StripeJS | null>;
export const getStripe = () => {
  if (!stripePromise) {
    stripePromise = loadStripe(process.env.NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY!);
  }
  return stripePromise;
};
```

> **Note:** Always check the Stripe dashboard for the current latest API version label — it changes with each release.

#### Step 1.5: Checkout Page (`app/(shop)/checkout/page.tsx`)

Recommended approach: **Stripe Checkout** (hosted page) for fastest implementation.

```typescript
// app/(shop)/checkout/page.tsx
'use client';
import { redirect } from 'next/navigation';

export default async function CheckoutPage({
  searchParams,
}: {
  searchParams: { session_id?: string };
}) {
  // If returning from Stripe Checkout (session_id present), show confirmation
  if (searchParams.session_id) {
    return <OrderConfirmation sessionId={searchParams.session_id} />;
  }

  // Otherwise, redirect to Stripe Checkout
  const { getStripe } = await import('@/lib/stripe');
  const stripe = await getStripe();

  // Build line items from cart (client-side cart state or server-side session)
  const lineItems = buildLineItemsFromCart(/* pass cart data */);

  const { error } = await stripe.redirectToCheckout({ lineItems });

  if (error) {
    console.error('Stripe redirect error:', error);
    // Handle error — redirect to cart with error message
    redirect('/cart?error=checkout_failed');
  }
}
```

**Alternative: Stripe Elements (embedded, more control)**

If Dami prefers a fully embedded checkout (no redirect), use `StripeElements` with a custom form. This requires more UI work but provides a smoother experience. Stripe's official `@stripe/react-stripe-js` package makes this straightforward.

#### Step 1.6: Backend — Create Checkout Session (`app/api/checkout/route.ts`)

```typescript
// app/api/checkout/route.ts
import { NextResponse } from 'next/server';
import { stripe } from '@/lib/stripe';

export async function POST(request: Request) {
  const { items, customerEmail } = await request.json();

  const session = await stripe.checkout.sessions.create({
    payment_method_types: ['card'],
    line_items: items.map((item: any) => ({
      price_data: {
        currency: 'gbp',
        product_data: {
          name: item.name,
          images: item.images,
          metadata: { productId: item.id },
        },
        unit_amount: Math.round(item.price * 100), // Stripe uses pence
      },
      quantity: item.quantity,
    })),
    mode: 'payment',
    success_url: `${process.env.NEXT_PUBLIC_BASE_URL}/checkout?session_id={CHECKOUT_SESSION_ID}`,
    cancel_url: `${process.env.NEXT_PUBLIC_BASE_URL}/cart`,
    customer_email: customerEmail,
    shipping_address_collection: {
      allowed_countries: ['GB', 'DE', 'FR', 'NL', 'IE', 'ES', 'IT', 'PT', 'SE', 'DK', 'NO', 'US', 'CA', 'AU', 'NG', 'GH', 'KE', 'ZA'], // UK + EU + major diaspora
    },
    metadata: {
      source: 'printsbytee_mvp',
    },
  });

  return NextResponse.json({ url: session.url });
}
```

### Phase 2: Webhook Handling

#### Step 2.1: Stripe Webhook Handler (`app/api/webhooks/stripe/route.ts`)

```typescript
// app/api/webhooks/stripe/route.ts
import { headers } from 'next/headers';
import { stripe } from '@/lib/stripe';
import { NextResponse } from 'next/server';
import Stripe from 'stripe';

export async function POST(request: Request) {
  const body = await request.text();
  const signature = headers().get('stripe-signature');

  if (!signature) {
    return NextResponse.json({ error: 'Missing signature' }, { status: 400 });
  }

  let event: Stripe.Event;

  try {
    event = stripe.webhooks.constructEvent(
      body,
      signature,
      process.env.STRIPE_WEBHOOK_SECRET!
    );
  } catch (err) {
    console.error('Webhook signature verification failed:', err);
    return NextResponse.json({ error: 'Invalid signature' }, { status: 400 });
  }

  switch (event.type) {
    case 'checkout.session.completed': {
      const session = event.data.object as Stripe.Checkout.Session;
      // → Create order in database
      // → Send confirmation email
      // → Update inventory
      console.log('✅ Order received:', session.id, 'Customer:', session.customer_email);
      break;
    }

    case 'payment_intent.payment_failed': {
      const paymentIntent = event.data.object as Stripe.PaymentIntent;
      // → Log failed payment, notify customer
      console.log('❌ Payment failed:', paymentIntent.id);
      break;
    }

    case 'charge.refunded': {
      const charge = event.data.object as Stripe.Charge;
      // → Update order status to 'refunded'
      // → Initiate refund flow
      console.log('💰 Refund processed:', charge.id);
      break;
    }

    case 'dispute.created': {
      const dispute = event.data.object as Stripe.Dispute;
      // → Notify Dami, flag order
      console.log('⚠️ Dispute opened:', dispute.charge);
      break;
    }

    default:
      console.log(`Unhandled event type: ${event.type}`);
  }

  return NextResponse.json({ received: true });
}
```

#### Step 2.2: Expose Webhook Locally for Testing

```bash
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

This returns a **webhook signing secret** (`whsec_...`) — add it to `.env.local` as `STRIPE_WEBHOOK_SECRET`.

In production, configure the webhook URL in Stripe Dashboard → Webhooks:
```
https://printsbytee.co.uk/api/webhooks/stripe
```

Events to listen for:
- `checkout.session.completed`
- `payment_intent.payment_failed`
- `charge.refunded`
- `dispute.created`

### Phase 3: Refund Flow

#### Manual Refunds (via Stripe Dashboard)
For MVP+1, manual refunds via the Stripe Dashboard are sufficient:
1. Stripe Dashboard → Payments → find the charge → Refund
2. Refund is processed to the original card within 5-10 business days

#### Programmatic Refunds (future enhancement)
```typescript
// app/api/orders/[orderId]/refund/route.ts
import { stripe } from '@/lib/stripe';

export async function POST(request: Request, { params }: { params: { orderId: string } }) {
  const { reason } = await request.json();
  const chargeId = await getChargeIdForOrder(params.orderId);

  const refund = await stripe.refunds.create({
    charge: chargeId,
    reason: reason, // 'duplicate' | 'fraudulent' | 'requested_by_customer'
  });

  return NextResponse.json(refund);
}
```

### Phase 4: PayPal Integration (Secondary)

Only implement if Dami wants PayPal as a secondary option. If Stripe alone is sufficient for MVP+1, skip this phase.

#### Step 4.1: PayPal Setup
1. Create a [PayPal Business account](https://www.paypal.com/uk/business)
2. Create a **REST API app** in PayPal Developer Dashboard
3. Get `Client ID` and `Secret`

#### Step 4.2: Install SDK
```bash
npm install @paypal/react-paypal-js
```

#### Step 4.3: Render PayPal Button
```typescript
// components/checkout/PayPalButton.tsx
import { PayPalScriptProvider, PayPalButtons } from '@paypal/react-paypal-js';

export function PayPalButton({ amount, onSuccess }: { amount: string; onSuccess: (orderId: string) => void }) {
  return (
    <PayPalScriptProvider options={{ clientId: process.env.NEXT_PUBLIC_PAYPAL_CLIENT_ID! }}>
      <PayPalButtons
        createOrder={(data, actions) => {
          return actions.order.create({
            intent: 'CAPTURE',
            purchase_units: [{ amount: { value: amount } }],
          });
        }}
        onApprove={async (data, actions) => {
          const details = await actions.order?.capture();
          onSuccess(data.orderID);
        }}
      />
    </PayPalScriptProvider>
  );
}
```

---

## 5. Security Considerations

### 5.1 PCI DSS Compliance

Because PrintsbyTee will use **Stripe Elements** or **Stripe Checkout**, the PCI compliance burden is minimal:

| Approach | PCI Requirement |
|---|---|
| Stripe Checkout (hosted) | SAQ-A — the simplest form. Stripe handles all card data. |
| Stripe Elements (embedded) | SAQ-A — same, because card data goes directly to Stripe |
| Direct card storage (NEVER do this) | SAQ-D — very high burden, essentially requires annual audit |

**Rule: Never store card numbers, CVVs, or card expiry dates in the database.** Only store the Stripe `customer_id` or `payment_method_id` references.

### 5.2 What To Store

```typescript
// In your orders table, store:
{
  orderId: string;           // Internal order ID
  stripeSessionId: string;   // Stripe checkout session ID
  stripePaymentIntentId: string; // Stripe payment intent
  customerEmail: string;
  customerName: string;
  shippingAddress: Address;
  lineItems: LineItem[];
  totalAmount: number;       // In pence
  currency: 'GBP';
  status: 'pending' | 'paid' | 'refunded' | 'disputed';
  createdAt: Date;
}
```

### 5.3 Environment Variable Security

- `.env.local` — committed to local dev only, **never** to git
- In production (Vercel/ Railway/ etc.) — use the platform's environment variable config
- Rotate secret keys immediately if leaked

### 5.4 Webhook Security

- Always verify Stripe webhook signatures (handled by `stripe.webhooks.constructEvent`)
- Use HTTPS in production
- Log all webhook events for audit trail

---

## 6. Required Business Documents

To activate Stripe and receive payouts, you'll need:

| Document | Notes |
|---|---|
| **UK Company Registration** | Companies House number (if incorporated) or sole trader registration |
| **Personal identification** | Director/owner ID (passport or driver's licence) |
| **Proof of address** | Bank statement or utility bill (not older than 3 months) |
| **Bank account** | UK GBP account for payouts |
| **Privacy policy URL** | Required at checkout |
| **Terms of service URL** | Required at checkout |

For PayPal Business:
- Business verification (similar to Stripe)
- Bank account linked

---

## 7. Timeline Estimate

| Phase | Task | Effort | Notes |
|---|---|---|---|
| **1.1** | Stripe account setup + verification | Low | 1-2 days (account approval can take up to 1-2 days) |
| **1.2** | Install dependencies | Low | 10 minutes |
| **1.3** | Environment variables | Low | 15 minutes |
| **1.4** | Stripe client setup (`lib/stripe.ts`) | Low | 30 minutes |
| **1.5** | Checkout page + API route | Medium | 2-4 hours |
| **1.6** | Webhook handler | Medium | 2-3 hours (includes local testing) |
| **2** | Order confirmation page | Medium | 1-2 hours |
| **3** | Refund flow (manual via dashboard — MVP sufficient) | Low | Done |
| **4** | PayPal integration (optional, secondary) | Medium | 3-4 hours |
| **5** | Testing end-to-end (test card transactions) | Medium | 2-3 hours |
| **6** | Go live (switch from test to live keys) | Low | 15 minutes |

**Total estimated effort: ~1.5–2 days of development work**

### Effort Scale
- **Low** = < 2 hours
- **Medium** = 2–6 hours
- **High** = 6+ hours

---

## 8. Test Cards (Stripe Test Mode)

Before going live, use Stripe's test cards:

| Card Number | Scenario |
|---|---|
| `4242 4242 4242 4242` | Successful payment |
| `4000 0025 0000 3155` | 3D Secure authentication required |
| `4000 0000 0000 9999` | Insufficient funds |
| `4100 0000 0000 0019` | Failed payment |

Use any future expiry date, any 3-digit CVC, and any 5-digit UK postcode.

Test webhook events locally with:
```bash
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

Trigger test events:
```bash
stripe events trigger checkout.session.completed
```

---

## 9. Go-Live Checklist

- [ ] Stripe account fully verified and activated
- [ ] Test mode: complete end-to-end checkout with test card `4242 4242 4242 4242`
- [ ] Test webhook events received locally (`stripe listen`)
- [ ] Switch API keys from `pk_test_...` / `sk_test_...` to `pk_live_...` / `sk_live_...`
- [ ] Configure live webhook URL in Stripe Dashboard
- [ ] Privacy policy page live at printsbytee.co.uk/privacy
- [ ] Terms of service page live at printsbytee.co.uk/terms
- [ ] Confirm payout bank account verified in Stripe
- [ ] Monitor first few live transactions in Stripe Dashboard
- [ ] Set up Stripe email notifications for disputes

---

## 10. Future Enhancements (Post MVP+1)

| Enhancement | Why |
|---|---|
| **Klarna / ClearPay BNPL** | Buy Now Pay Later increases conversion; target audience (fashion + diaspora) often uses BNPL |
| **Stripe Connect (marketplace)** | If PrintsbyTee ever hosts third-party sellers |
| **Apple Pay / Google Pay** | One-tap mobile payments; Stripe supports this via Payment Request Button with minimal extra work |
| **Subscription model** | If PrintsbyTee adds a membership or loyalty programme |
| **Multi-currency pricing** | Display prices in EUR, USD, NGN etc. based on customer location |

---

## 11. Quick Reference

| Resource | URL |
|---|---|
| Stripe Dashboard | dashboard.stripe.com |
| Stripe API Docs | stripe.com/docs |
| Stripe Next.js Guide | stripe.com/docs/payments/checkout/nextjs |
| Stripe Webhooks Guide | stripe.com/docs/webhooks |
| PayPal Developer | developer.paypal.com |
| UK Card Fees (Stripe) | 1.5% + £0.20 UK; 2.5% + £0.20 EU; 3.25% + £0.20 international |
| PayPal Card Fees (UK) | 2.9% + £0.30 |

---

*Document prepared by researcher-agent for PrintsbyTee MVP+1 sprint planning. For questions or clarifications, escalate to main agent.*
