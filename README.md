## Overview
Implement the GO DISH User Panel as a single HTML-CSS-JS file, connected to the same Firebase project. It provides login/register/forgot flows, browsing movies, search and category filters, movie details with trailer play, profile, and logout. All data uses Firebase Authentication, Firestore, and Realtime Database only.

## Pages & Navigation
1. Auth Screens:
- Login (email/password)
- Register (name, email, password) → creates Firestore `users/{uid}`
- Forgot Password (email) → sends reset
2. Main App Screens:
- Home: grid of posters with search by title and filter by genre; sections for Top Rated and Popular
- Categories: list of genre chips/rows to filter the grid
- Movie Details: modal/page with poster, description, year, genre, IMDb rating, and "Watch Trailer"
- Profile: shows name/email, allows updating display name; logout button
3. Navigation:
- Bottom/side nav with icons: Home, Categories, Profile, Logout

## Data Model & Realtime
- Firestore `movies`: `{ title, desc, genre, year, rating, poster, trailer, views, createdAt, updatedAt }`
- Firestore `users/{uid}`: `{ name, email, role:'user', blocked:false, createdAt }`
- Realtime Database:
  - `blockedUsers/{uid}` (boolean)
  - `stats/{ totalMovies, totalUsers, totalGenres }` (read-only for the user panel)
- Realtime subscriptions:
  - Home listens to `movies` collection (ordered by rating, by views)
  - Profile listens to `users/{uid}` for name changes/block status

## Authentication & Block Handling
- On register: create Firestore profile, set `{ blocked:false, role:'user', createdAt: serverTimestamp() }`
- On login: check Firestore `users/{uid}.blocked` OR RTDB `blockedUsers/{uid}`; if blocked, show "Account blocked" and force sign-out
- Forgot password: uses Firebase Auth API

## UI/UX & Theme
- Match admin neon/dark cinematic theme: gradients, glowing cards, hover transitions, responsive layout
- Loading skeletons during fetch; toasts for success/error
- LocalStorage for minor UI preferences (e.g., last selected genre)

## Trailer & Popularity
- "Watch Trailer" opens modal/lightbox with embedded `<iframe>` for YouTube/Vimeo URLs
- Increment `views` in Firestore with atomic `increment(1)` when trailer starts or details modal opens
- "Popular" section derives from highest `views`

## File Output
- Produce a single file named `godish-user.html` in the same directory as the admin file
- Import Firebase v11 ESM modules directly (same config provided)

## Security Considerations (Rules to be applied on backend)
- Firestore: `movies` read allowed to all; writes to `movies` restricted to admins only
- Firestore: users can read/update their own profile name; cannot modify `blocked` or `role`
- RTDB: `blockedUsers` and `stats` writable by admins only; readable by authenticated users

## Implementation Steps
1. Scaffold single-file layout and theme consistent with admin
2. Implement Auth screens and logic (login/register/forgot, block check)
3. Build Home grid with search and genre filters; Top Rated and Popular tabs
4. Implement Movie Details modal with trailer and `views` increment
5. Implement Categories and Profile screens with name edit and logout
6. Wire realtime listeners for movies and user profile; add loading states and toasts
7. Validate URLs and inputs; handle fetch errors gracefully

## Acceptance Criteria
- Single HTML file runs in browser, connects to Firebase with provided config
- Auth works end-to-end; blocked users denied access
- Movies render in real-time with search and genre filters
- Details modal shows full info and plays trailer; popularity updates via `views`
- Profile shows user info and allows name edit; logout works
- UI matches cinematic theme and is responsive on mobile

Confirm to proceed and I will generate `godish-user.html` accordingly.
