# Rose Wrench Automotive — GitHub Pages + Firebase

Complete GitHub-ready static site for the Rose Wrench Automotive appointment scheduler.

## Files

- `index.html` — customer booking + owner scheduler
- `reset.html` — resets scheduler data and recreates defaults
- `.nojekyll` — keeps GitHub Pages from applying Jekyll processing

## Publish with GitHub Pages

1. Create a GitHub repository.
2. Upload all files in this folder to the repository root.
3. Commit the files.
4. Open **Settings → Pages**.
5. Under **Build and deployment**, select **Deploy from a branch**.
6. Select the branch containing these files and `/ (root)`.
7. Save.
8. GitHub will provide the public Pages address.

## Firebase

The site uses the Firebase project/config supplied in the original Firebase page.

Firestore collections used by the scheduler:

- `scheduler/settings`
- `scheduler/services`
- `scheduler/bookings`

The reset page does not touch `products_bracelets`.

## Important security note

GitHub Pages is static hosting. Firebase client configuration is normally included in browser code; security must come from Firebase Authentication and Firestore Security Rules.

Do not make production Firestore rules allow anyone to delete or overwrite scheduler data. The current reset utility is intended for setup/testing and should be protected or removed before public production use.

The scheduler also needs a transaction/locking strategy for guaranteed double-booking prevention when two customers book simultaneously.


## Owner / Firebase Admin Page

`ownership.html` provides:
- Firebase Authentication sign-up/sign-in
- Owner UID assignment in `scheduler/settings`
- Owner-only reset workflow
- Scheduler reset without deleting `products_bracelets`

For production, configure Firestore Security Rules so only the authenticated owner UID can write settings/services/bookings administration data. Do not rely on hiding the page URL for security.
