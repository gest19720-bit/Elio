# Elio — Pre-Launch Landing Page & Waitlist

A pre-launch marketing site for **Elio**, an upcoming AI Back-Office Agent for small businesses. Built with plain HTML, CSS, and vanilla JavaScript — no frameworks, no build step.

## What's included

- `index.html` — the main landing page (hero, problem/solution, features, how it works, integrations, FAQ)
- `waitlist.html` — a 5-step waitlist form that collects market research
- `success.html` — post-signup page with a referral link and social sharing
- `privacy.html`, `terms.html`, `contact.html` — supporting pages
- `css/` — `style.css` (design system + shared components), `waitlist.css` (form + success page), `responsive.css` (breakpoints)
- `js/` — `main.js` (nav, scroll, FAQ), `waitlist.js` (form logic), `referral.js` (referral codes + sharing), `supabase.js` (backend config)

## Running it locally

No build step is required. Because the pages use `fetch`-based Supabase calls, open them through a local server rather than `file://`:

```bash
# from inside the elio/ folder
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Connecting Supabase

1. Create a free project at [supabase.com](https://supabase.com).
2. In **Project Settings → API**, copy your **Project URL** and **anon public key**.
3. Open `js/supabase.js` and replace `YOUR_SUPABASE_URL` and `YOUR_SUPABASE_ANON_KEY` with the Project URL and anon public key for the Elio project. Never use the service_role key in browser code.
4. In **Supabase Dashboard → SQL Editor**, run the complete file `supabase/migrations/001_waitlist.sql`. It creates a private `waitlist_users` table and an anonymous `join_waitlist` RPC. The RPC validates input, rejects duplicate emails, generates referral codes, and credits valid referrers atomically.

Until Supabase is configured, the waitlist form still works end-to-end for testing — submissions are logged to the browser console and the user is still taken to the success page with a working referral code.

## How the referral system works

- Every signup gets a randomly generated 6-character code (e.g. `AB3F9K`).
- Sharing `waitlist.html?ref=AB3F9K` credits that code's owner if the new signup completes the form.
- Referral counts are stored on each user's row in `waitlist_users.referral_count`.
- Rewards are purely presentational right now (1 invite → Priority Access, 3 → Early Access, 5 → VIP Early Access) — wire up real fulfillment later.

## What's intentionally not built

Per the project brief, this repository only covers the pre-launch marketing site. It does **not** include the Elio product dashboard, authentication, billing, or any AI agent functionality. Integrations listed on the site (Gmail, Stripe, Paystack, etc.) are labeled as planned and are not live.

## Customizing

- Brand colors and fonts live as CSS variables at the top of `css/style.css`.
- Replace every `<!-- Replace this placeholder with the official Elio mascot -->` comment and its neighboring `.mascot-placeholder` div with real mascot artwork once available.
- Copy for every section matches the approved brief; adjust in the relevant HTML file directly since there's no CMS layer.
