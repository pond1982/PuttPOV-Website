# PuttPOV marketing site

Static site that covers the three URLs App Store Connect needs:

| URL field in App Store Connect | Page |
| --- | --- |
| Marketing URL (optional) | `index.html` |
| Privacy Policy URL (required) | `privacy.html` |
| Support URL (required) | `support.html` |

No build step. Drop the folder on any static host (GitHub Pages, Netlify, Cloudflare Pages, S3, your own server).

## Local preview

```sh
python3 -m http.server --directory Website 8000
open http://localhost:8000
```

## Placeholders to swap before going live

These are committed as defaults so the pages render. Find-replace before publishing.

| Placeholder | Where | Replace with |
| --- | --- | --- |
| `pondd.me@gmail.com` | `index.html`, `privacy.html`, `support.html` | Your real support inbox |
| `puttpov.com` | `og:image` URLs / future absolute links | The domain you'll publish on |
| `#cta` (Coming soon button) | `index.html` hero | Your App Store URL once the app is live: `https://apps.apple.com/app/idXXXXXXXX` |
| `Last updated: 21 May 2026` | `privacy.html` | Bump whenever the policy changes |

Sample find/replace:

```sh
LC_ALL=C find Website -type f \( -name '*.html' -o -name '*.css' -o -name '*.md' \) \
  -exec sed -i '' 's/pondd.me@gmail.com/help@yourdomain.com/g' {} +
```

## Deploy via GitHub Pages

1. Push the repo to GitHub.
2. Repo → Settings → Pages → Source: **Deploy from a branch**.
3. Branch: `main`, folder: `/Website` (or move the contents into `/docs` if you prefer that convention).
4. After deploy, the URLs to enter in App Store Connect are:
   - Marketing: `https://<your-domain>/`
   - Privacy:   `https://<your-domain>/privacy.html`
   - Support:   `https://<your-domain>/support.html`

## What's inside

```
Website/
├── index.html        — marketing landing (hero, 5 features, gallery, FAQ CTA)
├── privacy.html      — privacy policy (Firebase Analytics + Crashlytics disclosure)
├── support.html      — FAQ + contact (Apple's required Support URL)
├── styles.css        — shared brand palette + responsive layout
├── README.md         — this file
└── assets/
    ├── apple-touch-icon.png    180x180 iOS home-screen icon
    ├── favicon-32.png          32x32 favicon
    ├── favicon-192.png         192x192 PWA-style icon
    ├── appicon-512.png         512x512 source for the OG card
    ├── og-image.png            1200x630 social/preview card
    └── shots/
        ├── 01_read_the_break.png   web-sized App Store screenshots (620 wide)
        ├── 02_aim_like_a_putt.png
        ├── 03_see_every_break.png
        ├── 04_stroke_dont_tap.png
        └── 05_daily_challenge.png
```

Full-resolution 1242×2688 versions live in `Marketing/AppStore/iPhone65/` and are what you upload to App Store Connect; the `Website/assets/shots/` copies are downscaled purely for page-load speed.

## App Privacy form (App Store Connect)

The privacy policy at `privacy.html` matches what to disclose on the App Privacy form:

- **Data Linked to You**: none.
- **Data Not Linked to You** (collected via Firebase):
  - *Analytics → Product Interaction*: yes (the `green_selected`, `hole_start`, `putt_finished`, etc. events).
  - *Diagnostics → Crash Data*: yes.
  - *Diagnostics → Performance Data*: yes (Crashlytics).
- **Data Used to Track You**: none. PuttPOV does not call `ATTrackingManager`.

If you add any other third-party SDK later (ads, social, attribution, etc.), update both the App Privacy form and `privacy.html`.

## Updating

- Brand color / palette → `:root` block at the top of `styles.css`.
- Headline + feature copy → top of each `<section>` in `index.html`.
- Add an FAQ entry → copy a `<details>` block in `support.html`.
- Refresh the OG card → re-run the asset block in the screenshot generator script or hand-edit `assets/og-image.png`.
