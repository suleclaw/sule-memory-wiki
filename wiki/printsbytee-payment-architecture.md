# PrintsbyTee — Payment Integration Architecture (MVP+1)

> Document version: 1.0
> Author: Software Architect subagent
> Date: 2026-04-08
> Status: Draft — for review before implementation

---

## 1. Provider Comparison

### The Contenders

| Provider | Transaction Fee (UK) | Payout Timing | Integration Complexity | Notes |
|---|---|---|---|---|
| **Stripe** | 1.5% + 20p (domestic cards) | 2 business days (standard) | Medium — excellent docs, rich APIs | Best for B2C fashion, diaspora buyers |
| **PayPal** | 2.9% + 20p | Instant (PayPal balance) / 2-3 days (bank) | Low-Medium — easy but older UX | Strong trust in some diaspora markets |
| **Square** | 1.5% + 20p (online) | 1-2 business days | Medium — simpler than Stripe | Better for physical goods + POS |

### Recommendation: **Stripe as primary, PayPal as secondary**

**Why Stripe for PrintsbyTee:**
- Best card payment UX for fashion (clean, customizable appearance)
- Supports Apple Pay / Google Pay natively via the Payment Element
- Strong dashboard for order tracking and refund management
- Webhook system is battle-tested
- Well-suited for UK + international (diaspora) transactions — supports many currencies
- Checkout SDK integrates cleanly with Next.js

**Why PayPal as secondary:**
- Some customers (especially first-time buyers from African markets) prefer PayPal
- Adds trust signal — seeing PayPal at checkout reassures some buyers
- Stripe can route to PayPal in the same checkout flow via PayPal SDK

**Why NOT Square for now:**
- Square is stronger for in-person/omnichannel
- Stripe has better developer experience for Next.js
- Square's online checkout is competitive but Stripe's ecosystem (Atlas, Billing, Connect) is richer for future growth

### Fee Comparison for a £150 Order

| Provider | Fee | Net to PrintsbyTee |
|---|---|---|
| Stripe | £2.25 + 20p = **£2.45** | £147.55 |
| PayPal | £4.35 + 20p = **£4.55** | £145.45 |
| Stripe + PayPal (both available) | Depends on buyer's choice | Maximizes conversion |

---

## 2. Integration Architecture

### Tech Stack Context

- **Frontend**: Next.js 15 (App Router), TypeScript, Tailwind, shadcn/ui, Framer Motion
- **Backend**: Next.js API Routes (`/app/api/`)
- **Database**: (assumed PostgreSQL via Supabase based on skill context)
- **State**: React Context or Zustand for cart state

### High-Level Flow

```
Buyer clicks "Checkout"
  → POST /api/payments/create-intent   (create PaymentIntent, return client_secret)
  → Redirect to Stripe Payment Element (hosted on page, iframe)
  → Buyer enters card / PayPal
  → Stripe processes → success/failure
  → Webhook fires to /api/webhooks/stripe
  → Order saved to DB, confirmation email sent
```

### Frontend Architecture

**Files involved:**
- `app/cart/page.tsx` — Cart page (already exists)
- `app/checkout/page.tsx` — New dedicated checkout page
- `components/payment/checkout-form.tsx` — Stripe Payment Element wrapper
- `components/payment/order-summary.tsx` — Order total breakdown
- `app/api/payments/create-intent/route.ts` — POST endpoint
- `lib/stripe.ts` — Stripe server-side client initialization

**Checkout Page (`app/checkout/page.tsx`)**

```typescript
// app/checkout/page.tsx
import { CheckoutForm } from '@/components/payment/checkout-form';
import { OrderSummary } from '@/components/payment/order-summary';

export default function CheckoutPage() {
  return (
    <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 max-w-7xl mx-auto px-4 py-12">
      <CheckoutForm />
      <OrderSummary />
    </div>
  );
}
```

**Stripe Payment Element (`components/payment/checkout-form.tsx`)**

```typescript
// components/payment/checkout-form.tsx
'use client';

import { useState } from 'react';
import { loadStripe } from '@stripe/stripe-js';
import { Elements, PaymentElement, useStripe, useElements } from '@stripe/react-stripe-js';

const stripePromise = loadStripe(process.env.NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY!);

function PaymentForm() {
  const stripe = useStripe();
  const elements = useElements();
  const [isProcessing, setIsProcessing] = useState(false);
  const [errorMessage, setErrorMessage] = useState<string | null>(null);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    if (!stripe || !elements) return;

    setIsProcessing(true);
    const { error } = await stripe.confirmPayment({
      elements,
      confirmParams: {
        return_url: `${window.location.origin}/checkout/success`,
      },
    });

    if (error) {
      setErrorMessage(error.message ?? 'An unexpected error occurred.');
      setIsProcessing(false);
    }
    // On success, Stripe redirects to return_url — no further action needed here
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-6">
      <PaymentElement
        options={{
          layout: 'tabs',
          wallets: { applePay: 'auto', googlePay: 'auto' },
        }}
      />
      {errorMessage && (
        <p className="text-sm text-red-500">{errorMessage}</p>
      )}
      <button
        type="submit"
        disabled={isProcessing || !stripe}
        className="w-full py-3 px-4 bg-black text-white rounded-md font-medium disabled:opacity-50"
      >
        {isProcessing ? 'Processing...' : 'Pay Now'}
      </button>
    </form>
  );
}

export function CheckoutForm() {
  const [clientSecret, setClientSecret] = useState<string | null>(null);

  // Fetch client secret on mount (or when cart updates)
  // In real implementation, call /api/payments/create-intent here

  if (!clientSecret) {
    return <div className="animate-pulse h-48 bg-gray-100 rounded-lg" />;
  }

  return (
    <Elements
      stripe={stripePromise}
      options={{
        clientSecret,
        appearance: {
          theme: 'stripe',
          variables: {
            colorPrimary: '#000000',
            fontFamily: 'inherit',
          },
        },
      }}
    >
      <div className="bg-white p-6 rounded-lg border">
        <h2 className="text-xl font-semibold mb-4">Payment Details</h2>
        <PaymentForm />
      </div>
    </Elements>
  );
}
```

### Backend Architecture

**Stripe client initialization (`lib/stripe.ts`)**

```typescript
// lib/stripe.ts
import Stripe from 'stripe';

if (!process.env.STRIPE_SECRET_KEY) {
  throw new Error('STRIPE_SECRET_KEY is not set');
}

export const stripe = new Stripe(process.env.STRIPE_SECRET_KEY, {
  apiVersion: '2024-11-20.acacia',
  typescript: true,
});
```

**Create Payment Intent (`app/api/payments/create-intent/route.ts`)**

```typescript
// app/api/payments/create-intent/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { stripe } from '@/lib/stripe';
import { getCartTotal, getCartItems } from '@/lib/cart'; // assumed utility

export async function POST(req: NextRequest) {
  try {
    const { currency = 'gbp' } = await req.json();

    const items = getCartItems(); // from your cart context/state
    const amount = getCartTotal(); // in pence (integer)

    if (amount < 50) {
      return NextResponse.json({ error: 'Amount too small' }, { status: 400 });
    }

    const paymentIntent = await stripe.paymentIntents.create({
      amount,
      currency,
      automatic_payment_methods: {
        enabled: true, // enables Apple Pay, Google Pay, card, etc.
      },
      metadata: {
        order_id: crypto.randomUUID(), // temporary until orders table is set up
      },
    });

    return NextResponse.json({
      clientSecret: paymentIntent.client_secret,
    });
  } catch (error) {
    console.error('Payment intent creation failed:', error);
    return NextResponse.json(
      { error: 'Failed to create payment intent' },
      { status: 500 }
    );
  }
}
```

**Stripe Webhook (`app/api/webhooks/stripe/route.ts`)**

```typescript
// app/api/webhooks/stripe/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { stripe } from '@/lib/stripe';
import { db } from '@/lib/db'; // your db client
import Stripe from 'stripe';

export async function POST(req: NextRequest) {
  const body = await req.text();
  const signature = req.headers.get('stripe-signature')!;
  const webhookSecret = process.env.STRIPE_WEBHOOK_SECRET!;

  let event: Stripe.Event;

  try {
    event = stripe.webhooks.constructEvent(body, signature, webhookSecret);
  } catch (err) {
    console.error('Webhook signature verification failed:', err);
    return NextResponse.json({ error: 'Invalid signature' }, { status: 400 });
  }

  switch (event.type) {
    case 'payment_intent.succeeded': {
      const pi = event.data.object as Stripe.PaymentIntent;
      // Save order to database
      // await saveOrder({ stripePaymentId: pi.id, amount: pi.amount, status: 'paid', ... })
      console.log(`Payment succeeded: ${pi.id}`);
      break;
    }
    case 'payment_intent.payment_failed': {
      const pi = event.data.object as Stripe.PaymentIntent;
      // Notify customer, log for review
      console.log(`Payment failed: ${pi.id}, reason: ${pi.last_payment_error?.message}`);
      break;
    }
    case 'charge.refunded': {
      const charge = event.data.object as Stripe.Charge;
      // Update order status, notify customer
      break;
    }
    default:
      console.log(`Unhandled event type: ${event.type}`);
  }

  return NextResponse.json({ received: true });
}
```

---

## 3. Security

### PCI DSS Compliance

**What PCI DSS requires for an e-commerce site:**
- Never store card numbers, CVV, or expiry dates — Stripe handles this entirely
- Use SSL/TLS for all payment pages (you already have HTTPS via deployment)
- Validate inputs, log access, restrict access to payment systems

**What you MUST do:**
- ✅ Never log `paymentIntent.client_secret` or card details
- ✅ Keep `STRIPE_SECRET_KEY` and `STRIPE_WEBHOOK_SECRET` in `.env.local` — never commit them
- ✅ Use environment variables for all secrets — never hardcode
- ✅ Verify webhook signatures (already in the webhook handler above)
- ✅ Use HTTPS in production

**Environment variables needed:**

```bash
# .env.local (never commit this file)
STRIPE_SECRET_KEY=sk_live_...
STRIPE_PUBLISHABLE_KEY=pk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_...
```

### Secrets Management

- Use `.env.local` for local development
- Use Vercel/Netlify environment variables for production
- Consider rotating `STRIPE_SECRET_KEY` if there's any suspicion of compromise
- Stripe dashboard has API key activity logs — review periodically

---

## 4. Implementation Phases

### Phase 1 — Minimal Viable Payments (MVP+1 Sprint)

**Goal:** Ship working Stripe card payments

**What's needed:**
- Stripe account setup (UK business account)
- Install `@stripe/stripe-js` and `stripe` npm packages
- Create `/app/checkout/page.tsx`
- Create `/app/api/payments/create-intent/route.ts`
- Create Stripe webhook handler
- Add new `orders` table to database
- Test in Stripe test mode before going live

**Effort:** Medium
**Timeline:** 3-5 days

### Phase 2 — Multi-Provider (Post-MVP+1)

**Goal:** Add PayPal, Apple Pay, Google Pay for maximum conversion

**What's needed:**
- Stripe already enables Apple Pay / Google Pay via `automatic_payment_methods` — verify domain registration with Stripe
- PayPal: Add PayPal SDK button alongside Stripe Payment Element (or use Stripe's PayPal integration)
- Consider: "Pay with Google Pay" / "Pay with Apple Pay" as primary buttons above the card form for mobile

**Effort:** Low (Apple Pay/Google Pay are nearly free with Stripe), Medium for PayPal
**Timeline:** 1-2 days for Apple/Google Pay, 2-3 days for PayPal

### Phase 3 — "Pay in 3" (Fashion Buy Now, Pay Later)

**Goal:** Klarna/Sezane-style installment payments

**What's needed:**
- Stripe offers **"Buy Now, Pay Later" (BNPL)** via Stripe's installments product
- Alternatively: Klarna integration (via Klarna's SDK) for "Pay in 3"
- Requires: clear messaging at checkout, legal terms, refund flow for partial refunds
- For fashion: high average order value makes "pay in 3" attractive to customers
- **Cost**: BNPL providers typically charge 3-5% per transaction (higher than cards)

**Effort:** Medium
**Timeline:** 2-3 days + legal review
**Note**: Validate UK regulations for BNPL credit agreements — may need FCA registration depending on structure.

---

## 5. Data Model Changes

### New / Modified Tables

**`orders` table** (new)

```sql
CREATE TABLE orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  customer_email TEXT NOT NULL,
  customer_name TEXT NOT NULL,
  -- shipping address fields (add as needed)
  shipping_address_line1 TEXT,
  shipping_address_city TEXT,
  shipping_address_postal_code TEXT,
  shipping_address_country TEXT DEFAULT 'GB',

  -- totals (stored in smallest currency unit, e.g., pence)
  subtotal_cents INTEGER NOT NULL,
  shipping_cents INTEGER NOT NULL DEFAULT 0,
  discount_cents INTEGER NOT NULL DEFAULT 0,
  total_cents INTEGER NOT NULL,

  -- payment
  stripe_payment_intent_id TEXT UNIQUE,
  stripe_charge_id TEXT,
  payment_status TEXT NOT NULL DEFAULT 'pending'
    CHECK (payment_status IN ('pending', 'paid', 'failed', 'refunded', 'partially_refunded')),

  -- order tracking
  order_number TEXT UNIQUE NOT NULL, -- human-readable e.g. "PBT-00042"
  status TEXT NOT NULL DEFAULT 'confirmed'
    CHECK (order_status IN ('confirmed', 'processing', 'shipped', 'delivered', 'cancelled')),
  notes TEXT,

  -- timestamps
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
);

-- Indexes
CREATE INDEX idx_orders_customer_email ON orders(customer_email);
CREATE INDEX idx_orders_stripe_payment_intent_id ON orders(stripe_payment_intent_id);
CREATE INDEX idx_orders_order_number ON orders(order_number);
CREATE INDEX idx_orders_created_at ON orders(created_at DESC);
```

**`order_items` table** (new)

```sql
CREATE TABLE order_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  order_id UUID NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
  product_id UUID NOT NULL REFERENCES products(id), -- existing products table
  variant_id UUID REFERENCES product_variants(id),
  product_name_snapshot TEXT NOT NULL, -- snapshot at time of order (product name may change)
  sku TEXT,
  quantity INTEGER NOT NULL CHECK (quantity > 0),
  unit_price_cents INTEGER NOT NULL, -- price per unit at time of order
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_order_items_order_id ON order_items(order_id);
```

**`transactions` table** (new — for full payment audit trail)

```sql
CREATE TABLE transactions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  order_id UUID NOT NULL REFERENCES orders(id),

  -- Stripe/PayPal reference
  provider TEXT NOT NULL CHECK (provider IN ('stripe', 'paypal')),
  provider_transaction_id TEXT NOT NULL, -- stripe_payment_intent_id or paypal_txn_id
  provider_event_type TEXT NOT NULL, -- e.g. "payment_intent.succeeded"

  amount_cents INTEGER NOT NULL,
  currency TEXT NOT NULL DEFAULT 'GBP',
  status TEXT NOT NULL, -- success, failed, pending, refunded

  -- raw metadata snapshot for debugging
  metadata JSONB,

  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(provider_transaction_id, provider_event_type)
);

CREATE INDEX idx_transactions_order_id ON transactions(order_id);
CREATE INDEX idx_transactions_provider_tx_id ON transactions(provider_transaction_id);
```

**`cart` table changes** (if not already tracked):
- Ensure `cart_items` has product snapshots (price at time of adding to cart) vs current price

---

## 6. Error Handling

### Payment Failures

```typescript
// In checkout-form.tsx — handle Stripe errors gracefully
const { error } = await stripe.confirmPayment({ ... });

switch (error.type) {
  case 'card_error':
    // Show specific message from Stripe (e.g., "Your card was declined")
    setErrorMessage(error.message);
    break;
  case 'validation_error':
    // Client-side validation issue — shouldn't happen with Payment Element
    setErrorMessage('Please check your payment details.');
    break;
  default:
    // Unexpected error
    setErrorMessage('Something went wrong. Please try again.');
}
```

### Webhook Failures

**Retry logic:**
- Stripe webhooks retry automatically on 5xx responses — respond with non-2xx to trigger retry
- Stripe retries up to **72 hours** with exponential backoff
- Log all webhook failures to a `webhook_events` table or monitoring system

```typescript
// Best practice: respond 200 immediately, process async
export async function POST(req: NextRequest) {
  // Parse and verify first (fast), respond 200
  // Process in background — avoid timeout issues
  // Use a queue (e.g., Inngest, Trigger.dev, or even a simple DB flag) for reliability
}
```

**Failure handling:**
- Monitor `/api/webhooks/stripe` in Stripe dashboard (Webhook failures section)
- Set up alerts if webhook fails repeatedly
- For critical failures: email the team or log to a monitoring tool (Sentry, Datadog)

### Order State Transitions

```typescript
// orders.payment_status state machine:
// pending → paid (webhook: payment_intent.succeeded)
// pending → failed (webhook: payment_intent.payment_failed)
// paid → refunded (webhook: charge.refunded)
// paid → partially_refunded (webhook: charge.refunded with partial amount)
```

---

## 7. File Changes Summary

### New Files (Phase 1)

| File | Purpose |
|---|---|
| `lib/stripe.ts` | Stripe server client singleton |
| `lib/stripe-webhook.ts` | Webhook verification helper (optional extraction) |
| `app/api/payments/create-intent/route.ts` | Create Stripe PaymentIntent |
| `app/api/webhooks/stripe/route.ts` | Handle Stripe webhook events |
| `app/checkout/page.tsx` | Checkout page with Payment Element |
| `components/payment/checkout-form.tsx` | Stripe Elements form component |
| `components/payment/order-summary.tsx` | Order total sidebar |
| `components/payment/payment-success.tsx` | Post-payment success page |
| `app/checkout/success/page.tsx` | Post-payment redirect target |
| `lib/cart.ts` | Cart total/item helpers (if not already exists) |

### Modified Files (Phase 1)

| File | Change |
|---|---|
| `app/cart/page.tsx` | Add "Proceed to Checkout" CTA linking to `/checkout` |
| `app/layout.tsx` | (Check if Stripe provider needed at root level) |
| Environment config | Add `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` |
| `middleware.ts` | (Optional: protect checkout route, redirect empty carts) |

### Database Migrations

- Run SQL for `orders`, `order_items`, `transactions` tables
- (Assumes Supabase — use Supabase migrations or raw SQL)

---

## 8. Timeline & Effort

| Phase | Scope | Effort | Timeline | Notes |
|---|---|---|---|---|
| **Phase 1** | Stripe cards only (test mode) | **Medium** | 3-5 days | Includes DB schema, API routes, UI, webhook |
| **Phase 1b** | Stripe go live (switch from test to live keys) | **Low** | 1 day | Key rotation, final testing |
| **Phase 2** | Apple Pay + Google Pay (via Stripe) | **Low** | 1-2 days | Mostly Stripe dashboard config, verify domain |
| **Phase 2** | PayPal integration | **Medium** | 2-3 days | Requires PayPal app setup, separate SDK or Stripe PayPal |
| **Phase 3** | BNPL "Pay in 3" | **Medium** | 2-3 days | Legal review of UK BNPL regulations recommended |

**Total MVP+1 payment integration: ~7-12 days**

---

## 9. Pre-Implementation Checklist

Before starting Phase 1, confirm:

- [ ] Stripe account created at stripe.com (UK business account)
- [ ] Stripe dashboard accessible, API keys generated (test + live)
- [ ] Domain `printsbytee.co.uk` verified in Stripe (for Apple Pay)
- [ ] Existing database migration tooling confirmed (Supabase CLI? manual SQL?)
- [ ] Cart state management in place (Zustand/Context) — to pass items to create-intent
- [ ] HTTPS confirmed on production domain (required for Stripe)
- [ ] Refund policy page exists (required for BNPL in Phase 3)
- [ ] Customer email flow confirmed (how are order confirmations sent? Resend? SendGrid?)

---

## Appendix A: Install Dependencies

```bash
npm install @stripe/stripe-js stripe
```

## Appendix B: Key Stripe Test Card Numbers

| Card Number | Result |
|---|---|
| `4242 4242 4242 4242` | Successful payment |
| `4000 0000 0000 0002` | Always declined |
| `4000 0025 0000 3155` | Requires 3D Secure |
| `4000 0000 0000 9995` | Insufficient funds |

Use any future expiry date, any 3-digit CVC, any postal code.

---

Next step: Review this document with Dami, confirm provider choice, then begin Phase 1 implementation.*
