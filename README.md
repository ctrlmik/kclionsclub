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

## Making a self-hosted deploy (GitHub Pages, etc.) sync live for everyone
By default, a self-hosted copy of `index.html` only saves data in each visitor's own
browser — it does **not** sync between people or devices on its own. The live Claude
artifact link syncs automatically with no setup. To get the same live sync on your own
deployment, connect a free Firebase project:

1. Go to https://console.firebase.google.com and create a free project (no credit card
   needed for the free "Spark" plan).
2. In the project, open **Build → Firestore Database** and click **Create database**
   (start in test mode, or use the rule below).
3. In Firestore's **Rules** tab, paste and publish:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /club/{doc} { allow read, write: if true; }
     }
   }
   ```
   ⚠️ This makes the data readable/writable by anyone who has your Firebase project's
   config (not just people with your app's link) — the same trust model as the app's
   login already has. Don't put highly sensitive personal data in the club records.
4. Go to **Project settings → General**, scroll to "Your apps", add a **Web app**, and
   copy the `firebaseConfig` object it gives you.
5. Open `index.html` in a text editor, find the line starting with `const FIREBASE_CONFIG=`
   near the top of the `<script>` section, and paste your values into the empty quotes.
6. Save, redeploy, and reload the page — the sign-in screen briefly shows "Loading…"
   while it connects, then everyone who opens that URL shares the same live data.

If `FIREBASE_CONFIG` is left blank (as shipped), the app just uses each browser's own
local storage, same as before.

## Notes
This copy was exported from a Claude.ai artifact. Activity photo uploads still only work
on the live Claude artifact link, not on a self-hosted deploy (with or without Firebase).

The username/password login is a convenience layer, not a strong security boundary —
anyone with the file and some technical skill could bypass it or read the stored (hashed)
passwords via browser developer tools. Treat real access control as coming from who you
share the file/link with, and from your Firestore rules if you enable live sync.
