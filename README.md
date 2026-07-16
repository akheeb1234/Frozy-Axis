# Frozy Axis — Hostel Management

Single-page hostel management app (admin + student portal) backed by Firebase Auth and Firestore.

## Files

- `index.html` — the entire app (HTML/CSS/JS, no build step).
- `firestore.rules` — Firestore security rules matching the app's admin/student access model.
- `firestore.indexes.json` — placeholder Firestore indexes file (required by `firebase.json`).
- `firebase.json` — config for `firebase deploy` (Hosting + Firestore rules).
- `.github/workflows/deploy.yml` — GitHub Actions workflow that deploys `index.html` to GitHub Pages on every push to `main`.

## 1. Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

## 2. Enable GitHub Pages

1. In your GitHub repo, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **GitHub Actions**.
3. Push to `main` (or re-run the workflow from the **Actions** tab). The included workflow (`.github/workflows/deploy.yml`) will publish the site automatically.
4. Your app will be live at `https://<your-username>.github.io/<your-repo>/`.

No build step is needed since this is a static `index.html`.

## 3. Configure Firebase

The app is already wired to project **`frozy-c8ab4`** in `index.html`. Before it will work live, in the [Firebase Console](https://console.firebase.google.com/) for that project:

1. **Authentication → Sign-in method** → enable **Email/Password**.
2. **Firestore Database** → create a database (production mode).
3. **Authentication → Settings → Authorized domains** → add your GitHub Pages domain, e.g. `<your-username>.github.io`.
4. Deploy the security rules in `firestore.rules` (matches the app's admin/student logic):
   ```bash
   npm install -g firebase-tools
   firebase login
   firebase use frozy-c8ab4
   firebase deploy --only firestore:rules
   ```
5. Create your first admin: either add its email to `ADMIN_EMAILS` in `index.html`, or manually create a document at `admins/<uid>` in Firestore after the user signs up.

## Notes

- `ADMIN_EMAILS` in `index.html` (currently `businessfrozy@gmail.com`) is mirrored in `firestore.rules` — keep both in sync if you change it.
- Firebase Hosting is an alternative to GitHub Pages if you'd rather deploy with `firebase deploy --only hosting` using the included `firebase.json`.
