# UM Vantage — going live

This folder has everything needed to publish the site off of Claude, onto
your own GitHub Pages URL, backed by your Firebase project
(`consulting-project-c7b41`).

## 1. Finish the Firebase setup

In the Firebase console for `consulting-project-c7b41`:

1. **Build → Firestore Database → Create database.** Choose **Production
   mode**, pick a location (e.g. `nam5` or `us-central1`), create it.
2. **Build → Authentication → Sign-in method → Google → Enable.** This is
   what lets the admin dashboard gate itself to just your team.
3. **Project settings (gear icon) → Your apps → add a web app** (name it
   anything, skip "Also set up Firebase Hosting"). It'll show you a
   `firebaseConfig` object.
4. Open `index.html` in this folder, find the `firebaseConfig` block near
   the top (search for `REPLACE_WITH_YOUR`), and paste in the real values
   from step 3.
5. In **Firestore Database → Rules**, replace the contents with what's in
   `firestore.rules` in this folder, and click **Publish**.
6. In both `index.html` (the `ADMIN_EMAILS` array, right under
   `firebaseConfig`) and `firestore.rules` (the `isAdmin()` function), add
   the Google email addresses for Hunter, Preston, and Traci once you have
   them — they need to match in both places.

## 2. Publish to GitHub Pages

1. Create a new repo at `github.com/bstanton31/um-vantage` (public is
   fine — there's nothing secret in this code; the Firebase config is
   meant to be public, security is enforced by the rules file, not by
   hiding the config).
2. On the repo's page, **Add file → Upload files**, drag in `index.html`
   from this folder, commit.
3. **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`
   → Save.** GitHub will give you the live URL, which should be:
   `https://bstanton31.github.io/um-vantage/`
4. If you used a different repo name, the URL changes to match — tell me
   the real one and I'll regenerate the QR code on the homepage to point
   at it (it currently assumes `um-vantage`).

## 3. Test it end to end

- Open the live URL, click "Admin view," sign in with Google — you
  should land on the dashboard (empty, until someone applies).
- In another tab/device, go to `<your-url>/#apply`, fill out an
  application — it should show up live in the admin tab within a second
  or two.
- Try the "Export CSV" button on the admin page — it should download a
  normal file this time (no "declined" dialogs like on Claude).
- Scan the homepage QR code with your phone — it should go straight to
  the application, no sign-in wall, no app interception.

## What changed from the Claude version

- Swapped Claude's built-in `db`/`user`/`downloads` capabilities for real
  Firebase (Firestore + Google Auth) — same shape of data, so nothing
  about the UI or the 30 placeholder mentors changed.
- The admin dashboard now gates itself with a real "Sign in with Google"
  button instead of Claude's permission system; only emails in
  `ADMIN_EMAILS` (and `firestore.rules`) can read applications.
- CSV export is now a normal browser download.
- If Firebase can't load for any reason (offline, blocked script), the
  site still works for browsing and applying — just without saving or the
  live dashboard — instead of breaking.
