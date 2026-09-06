# Elio waitlist: Supabase setup

## 1. Supabase Dashboard → SQL Editor

Open `supabase/migrations/001_waitlist.sql`, copy the entire file, and run it in the **Elio** Supabase project.

This creates:

- `public.waitlist_users`, with email uniqueness and private row-level security.
- `public.join_waitlist(...)`, the only anonymous operation exposed to the browser.
- Atomic referral-code creation and referral crediting.

Do not add a public `select`, `update`, or `delete` policy. Waitlist data should be read from the Supabase dashboard or a future protected admin tool.

## 2. Source file → `js/supabase.js`

From **Project Settings → API**, copy the Elio project's **Project URL** and **anon public key** into:

```js
const ELIO_SUPABASE_URL = "https://your-project.supabase.co";
const ELIO_SUPABASE_ANON_KEY = "your-anon-public-key";
```

The anon key is safe for browser use when RLS is configured. Never paste the `service_role` key into this project, HTML, JavaScript, or local storage.

## 3. PowerShell → local test server

Run this from the project folder:

```powershell
python -m http.server 8000
```

Open `http://localhost:8000/waitlist.html`, complete a test signup, and verify the new row in **Table Editor → waitlist_users**. Test the same email again to confirm the duplicate-email message.

## 4. Production checklist

- Configure the deployed site URL in **Authentication → URL Configuration** only if you later add Supabase Auth; the waitlist itself does not require accounts.
- Replace the placeholder values in `js/supabase.js` before deployment.
- Keep `supabase/migrations/001_waitlist.sql` as the source of truth for future changes.
- Export or review waitlist data through a protected admin process; never expose the table to anonymous reads.
