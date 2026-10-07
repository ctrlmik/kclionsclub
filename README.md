# Koronadal Hinugyaw Lions Club — Club Manager

A single-page web app for the Hinugyaw Lions Club to record members, dues, billing,
payments/receipts, invoices, expenses, activities/service projects, minutes of meetings,
and role-based accounts (President/Admin, Treasurer, Secretary, Member).

## Files
- `index.html` — the whole app (open it directly in a browser, or host it as a static site).

## Sign in
A default admin account is created the first time the app ever runs with no saved data:
- Username: `admin`
- Password: `admin123`
Change this password immediately after first login (Accounts tab → Edit).
Forgot it / locked out? Use the "Forgot the password, or can't sign in?" link on the
sign-in screen.

## Live sync for self-hosted deploys (GitHub Pages, etc.)
This copy already has live sync built in, backed by a free Supabase project
(`hinugyaw-lions-club`, created for this app). Everyone who opens this `index.html`
from the same URL shares the same live data automatically — no setup needed.

Technical notes:
- Data is stored in a single Postgres table called `club` (one row per data partition:
  `core`, `fin`, `sec`), with Realtime enabled so every open tab updates live.
- The Supabase project is on the free tier and uses an open access policy — anyone who
  has the published `index.html` file (and therefore the embedded Supabase URL/key) can
  read and write the data directly, bypassing the app's own login if they chose to. This
  is the same trust model the app's username/password login already has: a convenience
  layer, not a strong security boundary. Don't put anything highly sensitive in the
  records, and keep the file itself only where your club's officers can get it.
- If you'd rather not use this shared Supabase project at all, edit `SUPABASE_CONFIG`
  near the top of `index.html`'s `<script>` section and point it at your own project
  (same `club` table structure), or clear it to fall back to each browser's own local
  storage with no sync.

## Notes
This copy was exported from a Claude.ai artifact. Activity photo uploads and meeting-form
signed-copy uploads still only work on the live Claude artifact link, not on a self-hosted
deploy — Supabase here only covers the data, not file storage.

The username/password login is a convenience layer, not a strong security boundary —
anyone with the file and some technical skill could bypass it or read the stored (hashed)
passwords via browser developer tools. Treat real access control as coming from who you
share the file/link with.
