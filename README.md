# Birthday Wish Website — Setup Guide

Your site now works like this:
- **You** open the site, customize the text/photos, click **"Get My Link"** → it saves your
  wish to a database and gives you a link like `yoursite.com/?wish=AbC123`.
- **Anyone you send that link to** opens it and sees the finished wish — no editing
  controls, no login, just the wish.

To make this actually work online, you need a free database (Firebase) and free hosting.
Takes about 10-15 minutes the first time.

## 1. Create a free Firebase project
1. Go to https://console.firebase.google.com and sign in with Google.
2. Click **Add project** → give it any name → finish the wizard (you can skip Google Analytics).

## 2. Turn on Firestore (the database)
1. In your new project, open **Build → Firestore Database** in the left menu.
2. Click **Create database** → choose **Start in production mode** → pick any region → Enable.
3. Go to the **Rules** tab and replace the rules with the contents of `firestore.rules`
   (included in this folder), then click **Publish**.
   These rules let anyone **create** a new wish and **read** an existing one by its exact
   link ID, but nobody can list, edit, or delete other people's wishes.

## 3. Get your web config
1. In the Firebase console, click the ⚙️ gear icon → **Project settings**.
2. Scroll to **Your apps** → click the **</>** (web) icon → register an app (any nickname).
3. Firebase shows you a `firebaseConfig` object with your `apiKey`, `projectId`, etc.
4. Open `index.html` in this folder, find the `firebaseConfig` block near the top of the
   `<script>` section, and paste your real values in place of the `"YOUR_..."` placeholders.

## 4. Put it online (pick one)
**Option A — Firebase Hosting (recommended, free):**
```
npm install -g firebase-tools
firebase login
firebase init hosting     # choose your project, public dir = this folder, single-page app = No
firebase deploy
```
Firebase gives you a live URL like `https://your-project.web.app`.

**Option B — any static host** (Netlify, GitHub Pages, Vercel, etc.):
Just upload `index.html` as-is — Firestore works from any domain, no extra setup needed.

## Notes & limits (good to know)
- Each wish (text + photos) is saved as one database document, capped at **1MB**. The site
  compresses photos automatically, but keep it to **2-4 photos** for a smooth experience.
- There's no payment step in this version — every link is free to create. If you want to
  add payments later (e.g. Razorpay), that plugs in right before the "Get My Link" button
  calls `generateShareLink()` — happy to build that when you're ready.
- Firebase's free tier (Spark plan) comfortably covers a personal/small project like this.
