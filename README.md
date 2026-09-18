# دِلالة | DALALAH

An Arabic (RTL) real estate & vehicle marketplace prototype for Saudi Arabia — properties from
multiple offices, office pages, vehicle listings, a full multi-step "add listing" wizard,
a simple office dashboard, favorites, and a basic admin panel.

**Live features:** homepage with hero search, property search & filters, map/list toggle,
property & vehicle detail pages, public office pages, 8-step add-listing wizard (property or
vehicle), office dashboard (stats, listings, messages, settings), favorites, account screen,
admin panel (verify offices, moderate listings, reports, featured). Full RTL Arabic UI, SAR
pricing, Saudi cities/neighborhoods, light/dark theme support, mobile bottom navigation.

## Project structure

```
dalalah-marketplace/
├── public/
│   └── index.html      ← the entire application (markup + styles + JS)
├── package.json         ← optional local dev server (no build step needed)
├── vercel.json           ← static deployment config for Vercel
├── .gitignore
├── .env.example          ← documents that no env vars are required
└── README.md
```

## Architecture — please read before extending

This prototype is intentionally built as **one self-contained static HTML file**
(`public/index.html`), not a Next.js/React application:

- All markup, CSS, and JavaScript (routing, filtering, the listing wizard, the dashboard,
  the admin panel, favorites, etc.) live in that single file.
- **Tailwind CSS** is loaded via the Tailwind Play CDN (`cdn.tailwindcss.com`), and the
  **El Messiri** / **Tajawal** Arabic typefaces are loaded from Google Fonts. Both require
  an internet connection when the page loads — there is nothing to `npm install` for styling.
- All icons are inline SVG and all "property photos" are CSS gradients with an SVG icon
  overlay — there are no external image files, so there is nothing to go missing.
- All demo data (15 properties, 5 offices, 6 vehicles) is defined as plain JavaScript objects
  near the top of the `<script>` block in `index.html`.
- Anything a visitor "creates" (a published listing, a favorite, a draft, an admin action)
  is written to the browser's `localStorage`. There is no backend, database, or shared state
  between visitors — refreshing in another browser will not show another visitor's changes.

Because of this, there is no bundler, no `package-lock.json` to generate from a dependency
tree, and no build artifacts — the "build" is just the static file itself. The `package.json`
here only adds a tiny local dev server (`serve`) for convenience; it is not required to run
or deploy the site.

## Run it locally

**Option A — no install needed:** just open `public/index.html` directly in a browser.

**Option B — via a local server** (recommended, avoids any browser file:// restrictions):
```bash
npm install
npm run dev
```
Then open `http://localhost:3000`.

## Deploy to Vercel

1. Push this folder to a GitHub repository (see below).
2. In Vercel: **Add New → Project**, import the repository.
3. Framework preset: **Other**. Vercel will read `vercel.json`, which points the output
   directory at `public/` — no build step runs (`npm run build` just prints a message).
4. Deploy. No environment variables are required.

## Deploy to GitHub Pages

1. Push this repo to GitHub.
2. In **Settings → Pages**, set the source to the `main` branch, folder `/public` (or move
   `public/index.html` to the repo root as `index.html` if you'd rather serve from `/`).
3. Save — GitHub will publish the site at `https://<username>.github.io/<repo>/`.

## Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit — DALALAH marketplace prototype"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

## Known limitations of this prototype

- **No real backend.** Listings you publish, favorites, and admin actions are stored only in
  your own browser's `localStorage` — they are not shared across devices or visitors, and
  clearing browser data resets the demo back to the original 15 properties / 5 offices / 6
  vehicles.
- **No authentication.** The login/signup screen is a visual placeholder.
- **No real map or photos.** The "map" is a stylised placeholder, and listing photos are
  gradient/icon placeholders rather than uploaded images.
- **Requires internet access** on load, since Tailwind CSS and the Arabic fonts are pulled
  from CDNs rather than bundled locally.

## Turning this into a production app

The original brief called for Next.js + TypeScript + Tailwind + Supabase, with real auth,
a relational database, image storage, and per-office access control. This prototype
deliberately stops short of that so it can be reviewed and iterated on quickly. Migrating it
into that stack would mean: standing up a Next.js project, moving the data model
(`users`, `offices`, `listings`, `listing_media`, `favorites`, `saved_searches`, `reports`,
`contact_requests`) into Supabase, replacing the inline demo data and `localStorage` calls
with real queries, and splitting `index.html` into routed pages/components. Happy to help
with that migration as a separate, larger piece of work.

## License

Demo project — no license restrictions apply to the code itself.
