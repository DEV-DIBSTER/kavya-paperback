# Kavya — Setup Guide

This guide covers everything needed to develop, build, and deploy this extension from scratch.

---

## What This Project Is

Kavya is a [Paperback](https://paperback.moe/) source extension written in TypeScript. It connects Paperback (iOS manga reader) to a self-hosted [Kavita](https://www.kavitareader.com/) server. When pushed to `main`, GitHub Actions builds the extension and deploys it to GitHub Pages, where Paperback can install it from.

---

## Project Structure

```
kavya-paperback/
├── src/Kavya/
│   ├── Kavya.ts          # Main source class — all Paperback interface implementations
│   ├── Common.ts         # Request interceptor, auth, shared helpers
│   ├── Search.ts         # Search logic (title + tag filtering)
│   ├── Settings.ts       # Settings UI (DUI forms)
│   ├── CacheManager.ts   # In-memory 3-minute TTL cache
│   └── includes/
│       └── icon.png      # Extension icon shown in Paperback
├── docs/
│   └── GUIDE.md          # Full developer reference (architecture, API docs)
├── .github/workflows/
│   └── main.yml          # CI/CD — builds and deploys to gh-pages on push to main
├── package.json          # Dependencies and npm scripts
└── tsconfig.json         # TypeScript config
```

---

## Prerequisites

- Node.js 20+
- npm
- A Kavita server (self-hosted)

---

## Local Development

### 1. Install dependencies

```bash
npm install
```

### 2. Build

```bash
npm run build
```

### 3. Bundle (produces the Paperback-compatible output in `bundles/`)

```bash
npm run bundle
```

### 4. Serve locally (lets you test via Paperback's source URL)

```bash
npm run serve
```

---

## Deployment (GitHub Actions)

The workflow at `.github/workflows/main.yml` runs automatically on every push to `main`. It:

1. Installs dependencies (`npm install`)
2. Bundles the extension (`npm run bundle`)
3. Force-pushes the contents of `bundles/` to the `gh-pages` branch
4. GitHub Pages serves `gh-pages` at `https://DEV-DIBSTER.github.io/kavya-paperback`

### First-Time GitHub Setup

These steps only need to be done once per fork/repo:

**1. Create a GitHub Personal Access Token (fine-grained)**
- GitHub → Settings → Developer Settings → Personal Access Tokens → Fine-grained tokens
- Repository access: only `kavya-paperback`
- Permissions needed:
  - **Contents** → Read and write
  - **Actions** → Read and write
  - **Pages** → Read and write

**2. Add the token as a repo secret**
- Repo → Settings → Secrets and variables → Actions
- New repository secret → Name: `TOKEN`, Value: your PAT

**3. Enable GitHub Pages**
- Repo → Settings → Pages
- Source: Deploy from a branch
- Branch: `gh-pages`, folder: `/ (root)`
- Note: the `gh-pages` branch is created automatically by the first workflow run

**4. Push to `main`**
- The workflow fires and your extension goes live at `https://DEV-DIBSTER.github.io/kavya-paperback`

---

## Configuring the Extension in Paperback

### Step 1 — Add the source repository

Tap the button in the README, or manually add in Paperback:
- **URL:** `https://DEV-DIBSTER.github.io/kavya-paperback`

### Step 2 — Install Kavya

Find Kavya in the source list and install it.

### Step 3 — Configure Kavya settings

In Paperback → Sources → Kavya → Settings:

| Setting | Value |
|---|---|
| Server URL | Your Kavita server URL (e.g. `https://kavita.example.com`) |
| API Key | Your Kavita Auth Key (see below) |
| Page Size | 20 for iPhone, 40 for iPad |

### Finding Your Kavita API Key

Kavita 0.8+ uses named Auth Keys instead of a single UUID key:

1. In Kavita, go to the **Admin Panel** (gear icon, top right)
2. Navigate to **Auth Keys / OPDS**
3. Use an existing key or create a new one with **+ New**
4. Copy the key value and paste it into Paperback's Kavya settings

> The key does **not** need to be a UUID — the shorter named key format works fine.

---

## Bumping the Version

Version is defined in two places — keep them in sync:

- `package.json` → `"version"` field
- `src/Kavya/Kavya.ts` → `KavyaInfo.version`

---

## Known Limitations

### EPUB files are not supported
Paperback's reader only handles image-based pages. EPUB chapters will return 404 for every page because Kavita serves EPUBs through a web-based reader, not as images.

**What works:** Manga, comics, PDFs (CBZ, CBR, ZIP, PDF)
**What doesn't:** EPUB, Book/Novel type libraries

Enable **Exclude Book & Novel Type Libraries** in Kavya's settings to hide these from your home screen.

### `recentlyupdated` loads all results upfront
Kavita's recently-updated endpoint has no server-side pagination. All results are fetched at once and sliced locally.

### Read tracking requires the Paperback viewer
Marking a chapter as read outside the viewer (e.g. "mark as read" button) does not sync progress back to Kavita. Only reading through the viewer triggers the progress sync.

---

## Further Reference

See [docs/GUIDE.md](docs/GUIDE.md) for the full developer reference including:
- Complete Kavita API endpoint reference
- Paperback type/interface reference
- Request flow diagrams
- Caching strategy details
