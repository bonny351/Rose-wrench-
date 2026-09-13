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
- `scheduler/settings/services`
- `scheduler/settings/bookings`

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


## Firestore structure fix

The earlier version used paths such as `scheduler/bookings`, which Firestore rejects because a collection path must have an odd number of path segments.

This package uses the valid hierarchy:

- `scheduler/settings` — document
- `scheduler/settings/services` — collection
- `scheduler/settings/bookings` — collection

The screenshot error (`Invalid collection reference ... scheduler/bookings has 2 segments`) is fixed in this package.

`firestore.rules` is included as a starting point. Review it before production; in particular, public booking reads expose booking records to the browser because the current static-site availability system needs to read existing appointments.


## Multi-bay scheduling

The scheduler supports multiple service bays.

The owner can:
- Add or disable bays.
- Rename bays.
- Turn each bay on/off for each day of the week.

Customers do not need to pick a bay. When they choose a service and time, the system automatically finds an open bay that is free for the entire service duration. This means, for example, that if three bays are open, three appointments can run simultaneously at compatible times.

Bookings store `bayId` and `bayName` so the owner calendar shows which bay was assigned.


## Admin flow
Click **Owner Sign In** on the customer page. Successful Firebase login redirects to `ownership.html`, where you can view scheduled appointments, choose the number of bays, enable/disable bays, and set each bay's open/close time separately for every day. Job durations are also controlled there.
