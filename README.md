# pushuptabata.com

The landing page for Push-Up Tabata. Static HTML, one stylesheet, no build step —
Netlify serves the folder as-is.

## Netlify setup

- **Build command:** none
- **Publish directory:** `.` (the repository root)

`netlify.toml` does the rest: it declares the `Content-Type` for the app
association file, caches images, and redirects `www` to the bare domain.

## Universal links

`.well-known/apple-app-site-association` is what makes `pushuptabata.com/start`
open the app instead of the website. Three things Apple insists on, each of which
fails silently:

- Served as `application/json`. The file has no extension, so the type has to be
  declared — `netlify.toml` does it.
- HTTPS, valid certificate, **no redirect** on that path.
- No CDN minification of the body.

Only `/start` deep-links; everything else is excluded, so the landing page stays
reachable on a phone that has the app installed.

Verify after deploying:

    curl -sI https://pushuptabata.com/.well-known/apple-app-site-association | grep -i content-type
    curl -s  https://pushuptabata.com/.well-known/apple-app-site-association | python3 -m json.tool

iOS caches the association per install, so test changes on a freshly installed
build.

## Pages

| Path | Purpose |
| --- | --- |
| `/` | Landing page, features, FAQ. Carries `SoftwareApplication` and `FAQPage` structured data. |
| `/support.html` | The App Store's required support URL. |
| `/privacy.html` | The App Store's required privacy policy URL. |

## Images

- `images/hero.png` — the title screen in a device frame, transparent background.
- `images/og.png` — 1200×630 social card, composed by `Tools/make_og.swift` in the
  app repository.
- `images/icon.png` — the app icon, used as favicon and touch icon.
