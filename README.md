# Congregation Cart Program

> ### ⚠️ Do this first
> `public/firebase-config.js` in this repo still contains **placeholder values**
> (`PASTE_API_KEY_HERE`). Open it on GitHub, click the pencil icon, paste in your
> real Firebase settings, and commit — **before** you run the deploy workflow.
> Deploying with the placeholders will publish a site that cannot reach your
> database. Your real values are in Firebase Console → **Project settings →
> Your apps → SDK setup and configuration → Config**.

Public witnessing cart and poster check-in / check-out for the Scenic Woods,
Homestead and Hopper congregations.

Built on Firebase (free Spark plan): Firestore for data, Firebase Authentication
for sign-in, Firebase Hosting for the site. Pushing to GitHub deploys it.

---

## What is in this folder

| Path | What it is |
|---|---|
| `public/index.html` | The whole app — one self-contained file |
| `public/firebase-config.js` | Your Firebase project's settings |
| `public/manifest.webmanifest` | Home Screen / installed-app settings |
| `public/*.png`, `public/favicon.ico` | App icons (must stay at the root) |
| `firestore.rules` | Security rules — who may read and write what |
| `firestore.indexes.json` | Firestore indexes |
| `firebase.json` | Hosting and deploy settings |
| `.github/workflows/firebase-deploy.yml` | Deploys automatically on every push |

---

## One-time GitHub setup

### 1. Create the repository

1. Go to <https://github.com/new>.
2. Name it, for example, `congregation-cart-program`. **Private** is fine.
3. Do **not** add a README — this folder already has one.
4. Click **Create repository**.

### 2. Upload these files

On the new repository page choose **uploading an existing file**, then drag in
everything from this folder. Keep the folder structure: `public/` must stay a
folder, and `.github/workflows/` must come along too.

> If drag-and-drop skips `.github`, create it manually: **Add file → Create new
> file**, type `.github/workflows/firebase-deploy.yml` as the name, paste the
> file's contents, and commit.

### 3. Create a deploy key for GitHub

GitHub needs permission to deploy on your behalf.

1. In the [Firebase Console](https://console.firebase.google.com), open your
   project → gear icon → **Project settings** → **Service accounts**.
2. Click **Generate new private key** → **Generate key**. A `.json` file downloads.
3. Open that file in Notepad/TextEdit and copy **all** of it.

### 4. Add the two secrets

In your GitHub repository: **Settings → Secrets and variables → Actions →
New repository secret**. Add these two:

| Name | Value |
|---|---|
| `FIREBASE_SERVICE_ACCOUNT` | The entire contents of the `.json` file from step 3 |
| `FIREBASE_PROJECT_ID` | Your project ID, e.g. `cart-program` |

> The downloaded `.json` is a password to your project. Never commit it — it is
> already listed in `.gitignore`. Keep it only in the GitHub secret.

### 5. Deploy

Go to the **Actions** tab, open **Deploy to Firebase**, and click **Run
workflow**. It takes about a minute. From then on it runs automatically on every
push, and publishes **hosting, security rules and indexes** together.

---

## Making future edits

You can change the app straight from the GitHub website — no computer setup, no
terminal:

1. Open your repository and click the **Code** tab.
2. Click into the file you want to change (usually `public/index.html`).
3. Click the **pencil icon** (✏️) at the top right to edit it.
4. Make your change.
5. Scroll down and click **Commit changes**.

The Action runs by itself and **the live site updates a minute or two later**.
Watch it finish on the **Actions** tab — a green tick means it is live.

---

## Everyday admin

Sign in to the app and open **More → Admin**:

- **App settings** — name, overdue days, congregations, service-year start month
- **Carts** — add carts per congregation
- **Manage posters & inventory** — add or edit posters, set total copies
- **Import posters from a spreadsheet** — paste rows from your Feature Poster Index
- **Repair poster image links** — convert Drive/Dropbox share links to direct links
- **Manage people & administrators** — approve new sign-ups, promote admins
- **View reported issues** — problems people have reported in the app

New registrations must be **approved** before they can check anything out. When
someone is waiting you will see a red badge on the **More** tab and a banner on
the Home screen.

---

## Deploying from a computer instead (optional)

```bash
npm install -g firebase-tools
firebase login
firebase deploy --only hosting,firestore:rules,firestore:indexes
```

Always include `firestore:rules` when the rules have changed — deploying only
hosting leaves the old rules in place.

---

## Troubleshooting

**"Missing or insufficient permissions" in the app**
The rules did not publish. Re-run the Action, or paste `firestore.rules` into
Firestore → **Rules** → **Publish**.

**The Action fails on the deploy step**
Check both secrets. `FIREBASE_SERVICE_ACCOUNT` must be the whole JSON file
including the outer `{ }`, and `FIREBASE_PROJECT_ID` must match your project ID
exactly.

**The site deploys but looks unchanged**
Your browser cached it. Pull down to refresh, or close and reopen the tab.

**Home Screen icon shows a letter instead of the logo**
Confirm `https://<your-site>.web.app/apple-touch-icon.png` opens the logo, then
delete the Home Screen icon and add it again from Safari.
