# www.taskpapr.com

The holding/product page for taskpapr, served at **taskpapr.com** (canonical) and
**www.taskpapr.com** (redirects to the apex).

Deliberately a single static `index.html` — no Jekyll, no Ruby, no build step,
no dependencies to rot. Just link and CTAs to:

- **https://docs.taskpapr.com** — full docs and feature guide (separate repo:
  [taskpapr/taskpapr.github.io](https://github.com/taskpapr/taskpapr.github.io))
- **https://app.taskpapr.com** — the live app

## Editing

Edit `index.html` directly, then commit and push to `main`. GitHub Pages
("Deploy from a branch") republishes automatically — no build step, nothing
to run locally. Open `index.html` directly in a browser to preview.

## Brand tokens

Colour and type match the docs/marketing site (`taskpapr.github.io`) — see
`:root` custom properties at the top of `index.html`'s `<style>` block. Keep
the two in sync if the palette changes.

## Custom domain setup (reference)

Already done — see below only if it needs redoing (e.g. domain transfer).

1. **DNS** (Cloudflare, `taskpapr.com` zone):
   ```
   Type: A      Name: @     Value: 185.199.108.153  (and .109.153 / .110.153 / .111.153)
   Type: CNAME  Name: www   Target: taskpapr.github.io
   Proxy: DNS only (grey cloud) until GitHub issues the cert
   ```
2. **GitHub Pages must have the domain registered explicitly** — a `CNAME`
   file alone is only auto-detected for the classic "Deploy from a branch"
   build type (which this repo uses, so it *should* work automatically, but
   verify — see [taskpapr.github.io's postmortem on this exact
   failure mode](https://github.com/taskpapr/taskpapr.github.io) for why we
   don't fully trust that anymore):
   ```bash
   gh api repos/taskpapr/www/pages --jq '.cname'   # should print taskpapr.com
   # if null:
   gh api -X PUT repos/taskpapr/www/pages -f cname='taskpapr.com'
   ```
3. Once `https_certificate.state` is `approved`:
   ```bash
   gh api -X PUT repos/taskpapr/www/pages -F https_enforced=true
   ```
