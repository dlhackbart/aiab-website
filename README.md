# AI Automated Bookkeeping — Marketing Website

Live site: **https://aiautomatedbookkeeping.com**

This repo is the **deployed** marketing/landing site for AI Automated Bookkeeping
(AIAB), served free via **GitHub Pages**. It was created **2026-05-31** to move
the site off a lapsed GoDaddy hosting product (see [History](#history)).

---

## What's here

| File | Purpose |
|------|---------|
| `index.html` | Home / landing page ("QuickBooks-Quality Reports. Starting at $99/month") — How It Works · What's Included · Simple Pricing · Special Offers |
| `pricing.html` | Pricing page |
| `hero-image.png` | The large banner image at the top of the home page |
| `CNAME` | Tells GitHub Pages to serve at `aiautomatedbookkeeping.com` (do not delete) |
| `.nojekyll` | Disables Jekyll processing so files are served as-is |

> **Source note:** these files originated in the private repo
> `dlhackbart/aiautomatedbookkeeping` under `website/`. **This `aiab-website`
> repo is now the canonical, deployed copy** — edit *here*. (The private repo's
> `website/` folder is the historical source and is not auto-synced.)

---

## How it's hosted

- **Platform:** GitHub Pages (free), repo `dlhackbart/aiab-website`.
- **Source:** `main` branch, root (`/`).
- **Custom domain:** `aiautomatedbookkeeping.com` (set via the `CNAME` file).
- **HTTPS:** GitHub auto-provisions a Let's Encrypt certificate once DNS
  resolves; then "Enforce HTTPS" is enabled (Settings → Pages).
- **Why a separate public repo?** GitHub Pages on a free plan requires a
  **public** repo. The main `aiautomatedbookkeeping` repo is **private** and
  contains client data, so it must never be made public. This repo holds **only**
  the public marketing files.

---

## DNS configuration (at GoDaddy)

The domain `aiautomatedbookkeeping.com` is registered/managed at **GoDaddy**
(nameservers `ns59/ns60.domaincontrol.com`). To point it at GitHub Pages, these
records are set under **GoDaddy → the domain → Edit DNS → DNS Records**:

| Type | Name | Value | Notes |
|------|------|-------|-------|
| A | `@` | `185.199.108.153` | GitHub Pages IP |
| A | `@` | `185.199.109.153` | GitHub Pages IP |
| A | `@` | `185.199.110.153` | GitHub Pages IP |
| A | `@` | `185.199.111.153` | GitHub Pages IP |
| CNAME | `www` | `dlhackbart.github.io` | Recommended; `www` may also point at `@` |

These four GitHub Pages anycast IPs are stable/public (per GitHub's docs). If
GitHub ever changes them, update the A records to match.

---

## How to update the site

1. Edit `index.html` / `pricing.html` (and/or swap `hero-image.png`) in this repo.
2. Commit and push to `main`:
   ```bash
   git add -A
   git commit -m "Update site copy"
   git push
   ```
3. GitHub Pages rebuilds automatically within ~1 minute. Hard-refresh the browser
   (or use a private window) to bypass cache.

Do **not** delete `CNAME` or `.nojekyll` — removing `CNAME` drops the custom
domain (reverts to `dlhackbart.github.io`).

---

## Verifying / troubleshooting

```bash
# Is DNS pointing at GitHub? (should show the 185.199.108-111.153 IPs)
nslookup aiautomatedbookkeeping.com 8.8.8.8

# Is the site serving?
curl -sI http://aiautomatedbookkeeping.com      # expect HTTP/1.1 200, Server: GitHub.com

# Pages + cert status (needs gh CLI, authed as dlhackbart)
gh api repos/dlhackbart/aiab-website/pages
```

Common issues:
- **Browser shows nothing / "not secure" right after setup:** the SSL cert is
  still provisioning (can take minutes–24h after DNS resolves). The page works on
  `http://` meanwhile. If the browser refuses to load `http://` (it remembers the
  old site was `https://` — HSTS), clear it: visit `chrome://net-internals/#hsts`
  → "Delete domain security policies" → enter `aiautomatedbookkeeping.com` →
  Delete; or just use a different browser; or wait for the cert.
- **`DNS_PROBE_FINISHED_NXDOMAIN`:** the domain isn't resolving — check the A
  records exist at GoDaddy and that no "Domain Forwarding" is overriding them.
- **Site reverted to a 404 / github.io:** the `CNAME` file was removed — re-add it
  with `aiautomatedbookkeeping.com`.

---

## History

- **~Feb 2026:** site originally built (files in `aiautomatedbookkeeping/website/`)
  and hosted via GoDaddy.
- **2026-05-31:** site went down — DNS showed the GoDaddy zone intact (SOA) but the
  **A record had vanished**, the signature of a lapsed/disconnected GoDaddy hosting
  product (domain itself was *not* expired). Browser error:
  `DNS_PROBE_FINISHED_NXDOMAIN`.
- **2026-05-31:** redeployed to **GitHub Pages** (this repo) and repointed GoDaddy
  DNS A records to GitHub. Site restored, now free and self-hosted with no
  third-party hosting product to lapse again.
