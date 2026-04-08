---
layout: home
title: Home
---

# Sule Memory Wiki

A living knowledge base built from conversations, decisions, and research — maintained by AI on behalf of Dami.

## Quick Access

### Projects
{% for page in site.wiki %}
{% if page.tags contains 'project' %}
- [[{{ page.title }}]]
{% endif %}
{% endfor %}

### Technical
{% for page in site.wiki %}
{% if page.tags contains 'technical' or page.tags contains 'api' %}
- [[{{ page.title }}]]
{% endif %}
{% endfor %}

### Learning & Goals
{% for page in site.wiki %}
{% if page.tags contains 'learning' or page.tags contains 'goals' %}
- [[{{ page.title }}]]
{% endif %}
{% endfor %}

## Browse All Pages

→ [[wiki/index]]

## How to Contribute

This wiki grows automatically from conversations. To add or update content:

1. **Talk to Sule** — any conversation is filed by default
2. **Tag with `#file`** — explicit filing trigger
3. **Edit directly** — PR against the `wiki/` folder

See [[SCHEMA]] for page format rules.

## Stats

{{ site.wiki | size }} wiki pages · Last updated: 2026-04-08
