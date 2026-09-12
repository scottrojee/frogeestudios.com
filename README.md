# frogeestudios.com

Static site for Frogee Studios. No build step: plain HTML + one stylesheet, hosted on GitHub Pages behind the
Namecheap domain.

| Path | What |
|---|---|
| `index.html` | studio home |
| `beam-them-back/index.html` | the game page (Steam's "website" link) |
| `privacy/index.html` | the privacy policy (Steam's "privacy policy" link: `https://frogeestudios.com/privacy/`) |
| `assets/site.css` | the one stylesheet |
| `assets/img/` | store art scaled for the web (source of truth: `Holdfast/Docs/store/`) |
| `CNAME` | tells GitHub Pages the custom domain |

## Deploy (once)

1. Create a **public** GitHub repo (e.g. `scottrojee/frogeestudios.com`) and push this folder to `main`.
2. Repo Settings > Pages > Source: **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Settings > Pages > Custom domain: `frogeestudios.com`. Wait for the DNS check, then tick **Enforce HTTPS**
   (the certificate can take up to an hour after DNS resolves).

## DNS at Namecheap (Domain List > Manage > Advanced DNS)

Remove Namecheap's parking records, then add:

| Type | Host | Value | TTL |
|---|---|---|---|
| A | `@` | `185.199.108.153` | Automatic |
| A | `@` | `185.199.109.153` | Automatic |
| A | `@` | `185.199.110.153` | Automatic |
| A | `@` | `185.199.111.153` | Automatic |
| CNAME | `www` | `scottrojee.github.io.` | Automatic |

Email: Namecheap's free **Email Forwarding** (Domain > Redirect Email) can send `contact@frogeestudios.com`
to a real inbox. Set it up before the store page goes live; the privacy policy names that address.

## Updating

Edit the HTML, push to `main`. Pages redeploys in about a minute. Re-scale art from the game repo with:

```
ffmpeg -i Holdfast/Docs/store/01_build.jpg -vf scale=1280:-2 -q:v 4 assets/img/01_build.jpg
```

## Placeholders

- `beam-them-back/index.html`: the **Coming to Steam** button points at `#steam` until the app id exists; then
  `https://store.steampowered.com/app/<APPID>/`.

## State (2026-09-11)

Live at https://frogeestudios.com over HTTPS. DNS and email forwarding (contact/legal/press/support) are done at
Namecheap. Still owed: tick **Enforce HTTPS** in the repo's Pages settings; optionally verify the domain at
github.com/settings/pages.

The game page carries the 2026-09-09 trailer (`assets/video/`, poster in `assets/img/`) and a screenshot lightbox
over the 1920x1080 stills in `assets/img/full/`. Three stills want a re-capture from the game before Steam:
`01_build` (music ticker names a cut track), `06_lobby` (shows `LAN :7787`), `05_end` (HUD kill count disagrees with
the honours card). The Steam button is still the `#steam` placeholder.

Gotcha learned the hard way: a Namecheap **URL Redirect Record** publishes a hidden A record (`192.64.119.212`)
that makes GitHub refuse the certificate. Delete it, wait out the 30-minute TTL, then remove and re-add the custom
domain in Pages settings.
