# Rose Wrench Automotive — GitHub + Firebase

## Pages
- `index.html` — customer booking
- `admin-signup.html` — first-admin bootstrap signup
- `admin.html` — admin dashboard: calendar, job times, bays/hours, employees, reset
- `employee.html` — employee schedule
- `reset.html` — admin-only scheduler reset

## Firebase setup
1. In Firebase Authentication, enable **Email/Password**.
2. In Firestore, create the database.
3. Publish `firestore.rules` in Firestore Rules.
4. Open `admin-signup.html` and create the first admin. The Firestore rule is designed to allow only the first admin bootstrap; later public admin signup is blocked.
5. After the first admin exists, create employee Authentication accounts in Firebase Console and create `scheduler_users/{uid}` documents with `{role:"employee", active:true, name, email, uid}`. For a production system, use a trusted backend/Cloud Function to automate employee creation and role changes.

## Important scheduling note
The customer page performs a final availability check before creating a booking. For a production shop with simultaneous customers, use a Firestore transaction or slot-lock Cloud Function to make booking allocation atomic and eliminate race-condition double bookings.

## Firestore collections
- `scheduler_jobs`
- `scheduler_bays`
- `scheduler_bookings`
- `scheduler_users`
- `scheduler_meta/adminBootstrap`

The Firebase web config is the same project config supplied in the original Rose Wrench/bracelet page. Firebase web API keys are not treated as passwords; Firestore Rules and Authentication provide access control.
