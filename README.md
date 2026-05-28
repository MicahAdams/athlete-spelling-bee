# 🐝 Athlete Spelling Bee

A daily mobile-first spelling bee game featuring athletes. Five athletes per day, progressively harder, with Wikipedia photos and Scripps-style hint prompts.

---

## Deploying to GitHub Pages (step-by-step)

### What you need
- A free GitHub account (github.com)
- The `index.html` file from this folder

---

### Step 1 — Create a new GitHub repository

1. Go to **github.com** and sign in
2. Click the **+** icon in the top-right corner → **New repository**
3. Name it: `athlete-spelling-bee` (or anything you like)
4. Set it to **Public** (required for free GitHub Pages)
5. Check **"Add a README file"**
6. Click **Create repository**

---

### Step 2 — Upload the game file

1. On your new repository page, click **Add file** → **Upload files**
2. Drag and drop **index.html** into the upload area
3. Scroll down and click **Commit changes**

Your repository should now have two files: `README.md` and `index.html`

---

### Step 3 — Enable GitHub Pages

1. In your repository, click the **Settings** tab (top menu)
2. In the left sidebar, scroll down and click **Pages**
3. Under **Source**, select **Deploy from a branch**
4. Under **Branch**, select **main** and keep the folder as **/ (root)**
5. Click **Save**

GitHub will show a message: *"Your site is being published"*

---

### Step 4 — Get your live URL

1. Wait about 60–90 seconds
2. Refresh the Settings → Pages page
3. You'll see a green box with your URL:
   **`https://YOUR-USERNAME.github.io/athlete-spelling-bee/`**

That URL is your shareable link. It works on mobile and desktop.

---

### Step 5 — Share it

**On X (Twitter):**
```
🐝 Daily Athlete Spelling Bee — can you spell all 5?
New athletes every day, with photos and hints.
Play here: https://YOUR-USERNAME.github.io/athlete-spelling-bee/
```

**On Slack:**
Paste the URL directly — Slack will generate a preview card automatically.

---

## Setting up real Google Sign-In (optional, recommended for leaderboards)

The game ships with a mock Google login for testing. To enable real Google Sign-In:

### A — Get a Google OAuth Client ID (free)

1. Go to **console.cloud.google.com**
2. Click **Select a project** → **New Project** → name it and create
3. In the left menu: **APIs & Services** → **Credentials**
4. Click **+ Create Credentials** → **OAuth 2.0 Client ID**
5. Application type: **Web application**
6. Name it anything (e.g. "Spelling Bee")
7. Under **Authorized JavaScript origins**, add:
   - `https://YOUR-USERNAME.github.io`
8. Click **Create**
9. Copy the **Client ID** (looks like: `123456789-abc...apps.googleusercontent.com`)

### B — Add it to index.html

Open `index.html` and make these two changes:

**Change 1** — Find this line near the top and uncomment it (remove the `<!--` and `-->`):
```html
<!-- <script src="https://accounts.google.com/gsi/client" async defer></script> -->
```
Should become:
```html
<script src="https://accounts.google.com/gsi/client" async defer></script>
```

**Change 2** — Find this line:
```javascript
const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_CLIENT_ID';
```
Replace with your actual client ID:
```javascript
const GOOGLE_CLIENT_ID = '123456789-abc...apps.googleusercontent.com';
```

**Change 3** — Find the `mockGoogleLogin()` call in `showLoginModal()` and replace it with:
```javascript
onclick="google.accounts.id.prompt()"
```

**Change 4** — Uncomment the real Google auth functions in the script (they're clearly marked).

### C — Upload the updated file to GitHub

1. Go to your repository on GitHub
2. Click on `index.html`
3. Click the **pencil icon** (Edit)
4. Paste your updated code
5. Click **Commit changes**

The site updates within 30–60 seconds.

---

## Adding more athletes

Open `index.html` and find the `const ROSTER = [` array. Add entries following this format:

```javascript
{
  last: "LastName",          // the answer players must spell
  full: "First LastName",    // full name (used for audio and reveal)
  sport: "Sport Name",       // shown as a badge on the photo
  difficulty: 3,             // 1=Beginner, 2=Easy, 3=Medium, 4=Hard, 5=Expert
  wiki: "Wikipedia_Page_Title",  // exact title from the Wikipedia URL
  clue: "Description of this athlete shown to the player",
  sentence: "A sentence using their last name, as in the Scripps bee",
  origin: "Where the surname comes from, language/cultural background",
  context: "Extra career facts for the 'More context' hint button"
},
```

**Important:** Only add athletes whose Wikipedia page has a photo.
Test this by visiting: `https://en.wikipedia.org/wiki/[Wikipedia_Page_Title]`
If you see a photo in the infobox on the right, it will work.

---

## How the daily rotation works

- All athletes are shuffled using today's date as a random seed
- The first 5 that return a confirmed Wikipedia photo are used
- The same 5 athletes appear for every player on the same day
- At midnight, the date changes and a new set of 5 is selected automatically
- No server or scheduled job required — it's all calculated client-side

---

## Connecting a real leaderboard backend (optional)

The current leaderboard uses `localStorage`, which means scores are per-device and not shared across users. To make it truly global:

**Recommended options (all free tier available):**

| Service | Docs |
|---------|------|
| Firebase Firestore | firebase.google.com |
| Supabase | supabase.com |
| PocketBase | pocketbase.io |

Replace the `loadScores()`, `saveScores()`, and `recordScore()` functions in `index.html` with calls to your chosen backend's SDK.

---

## File structure

```
athlete-spelling-bee/
└── index.html    ← entire game (self-contained, no build step needed)
```

No npm, no build tools, no dependencies to install. Just one HTML file.
