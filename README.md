# Rose Wrench Automotive — GitHub/Firebase site

## Pages
- `index.html` — customer booking page
- `ownership.html` — owner/admin dashboard (Firebase login required)
- `employee.html` — employee read-only schedule (Firebase login required and email must be on the owner employee list)
- `reset.html` — legacy reset/setup page; use the Reset tab inside the owner dashboard for the protected reset
- `firestore.rules` — Firestore rules

## Admin tabs
1. **Calendar** — physical 30-minute bay calendar plus appointment list.
2. **Job Times** — set the duration for every service.
3. **Bays & Hours** — add/disable bays and set each bay's open/close time for each day.
4. **Employees** — add employee login emails.
5. **Reset** — owner-only reset of scheduler bookings/jobs/default bays.

## Firebase setup
1. In Firebase Authentication, enable **Email/Password**.
2. Create the owner account there.
3. Deploy `firestore.rules` in Firestore Rules.
4. Open `ownership.html` through GitHub Pages and sign in with the owner account. The first owner account initializes `scheduler/settings.ownerUid`.
5. Employees need their own Firebase Email/Password accounts. The owner then adds each employee's email in the Employees tab.
6. Customers can book without an account.

### Important
The Firebase web config is client-side configuration and is not a password. Firestore Rules are what control access. For a production shop system, customer booking writes should eventually be moved behind a server/Cloud Function with transactional slot locking so two customers cannot race for the same bay/time.

## Firestore paths
- `scheduler/settings`
- `scheduler/settings/services/{serviceId}`
- `scheduler/settings/bookings/{bookingId}`
