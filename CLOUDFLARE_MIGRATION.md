# Cloudflare migration runbook — juanprieto.us

Status: blocked on Cloudflare connector authorization (stuck `pending`) and
Claude in Chrome (not connected). Nothing below has been executed yet.
This file is the exact sequence to run the moment either path is live.

## Current state (verified)
- Live at https://juanprieto.us via GitHub Pages, repo
  `prietojp007/jp-marketing-portfolio`, branch `main`, custom domain via
  `CNAME` file + GoDaddy DNS.
- GoDaddy DNS currently has 6 A records on the apex — 4 correct GitHub
  Pages IPs (185.199.108/109/110/111.153) plus 2 stray records
  (3.33.130.190, 15.197.148.33) that were never cleaned up. These should be
  removed as part of this migration regardless of what else happens, since
  they're already causing intermittent "restricted" loads for visitors.
- `_headers` file already committed (commit b0b7517) with HSTS, CSP, and
  the other security headers below — Cloudflare Pages reads this
  automatically once deployed, no extra step needed.

## Step 1 — Add the zone to Cloudflare
1. Cloudflare dashboard → Add a Site → `juanprieto.us` → Free plan.
2. Cloudflare scans existing DNS; confirm it picks up (or manually add) the
   apex and `www` records — doesn't matter much since Pages will own these
   once step 3 is done.
3. Cloudflare gives two nameservers (format `xxx.ns.cloudflare.com`). These
   are the values that go to GoDaddy in Step 4 — record them exactly.

## Step 2 — Create the Pages project and deploy
Two options, pick based on what's connected:
- **Git integration (preferred)**: Workers & Pages → Create → Pages →
  Connect to Git → authorize GitHub → select
  `prietojp007/jp-marketing-portfolio` → build settings: no build command,
  output directory `/` (root) → Save and Deploy.
- **Direct upload (if GitHub auth isn't available)**: Workers & Pages →
  Create → Pages → Upload assets → upload the contents of this repo
  (`jp-portfolio.html`, `index.html`, `_headers`, `CNAME` can be omitted —
  custom domain is set in Pages UI instead, not via a CNAME file).

Either way, confirm the deployed URL (`*.pages.dev`) actually renders the
site before touching DNS.

## Step 3 — Attach the custom domain to the Pages project
1. Pages project → Custom domains → Add `juanprieto.us` (and `www` if
   wanted as a redirect).
2. Cloudflare auto-creates the proxied DNS record for it in the zone from
   Step 1 — no manual CNAME/A record needed since the zone lives on
   Cloudflare now.
3. Pages provisions its own Universal SSL certificate for the custom
   domain automatically. No "Full (strict)" origin-cert step is needed
   here — unlike a proxied VPS/origin server, Pages terminates TLS itself
   and serves the static content directly, so there's no separate origin
   certificate to configure.

## Step 4 — GoDaddy nameserver change (the one step requiring JP directly)
1. Log into GoDaddy → `juanprieto.us` → DNS / Nameservers.
2. Switch from GoDaddy-managed DNS to **Custom** nameservers.
3. Enter the two Cloudflare nameservers from Step 1, exactly as given —
   remove any existing GoDaddy-default nameservers.
4. Save. Propagation is usually under an hour, can take up to 24h.
5. This change also naturally resolves the stray-A-record issue above,
   since GoDaddy's DNS zone stops being authoritative entirely — Cloudflare
   becomes the sole source of truth for records.

## Step 5 — Security/TLS settings in Cloudflare (do before or right after
nameservers go active; doesn't require nameservers to be live yet)
- SSL/TLS → Overview: mode **Full (strict)** is the setting for a proxied
  origin; for a pure Pages custom domain this is less critical since Pages
  handles its own cert, but set it anyway for consistency if other
  subdomains get added later.
- SSL/TLS → Edge Certificates:
  - **Always Use HTTPS**: on.
  - **Automatic HTTPS Rewrites**: on.
  - **Minimum TLS Version**: 1.2.
  - **HSTS**: enable, max-age 6-12 months to start (the `_headers` file
    already sends `preload` in its own HSTS header — don't submit to the
    HSTS preload list until this has run clean for a few weeks, preload is
    very hard to reverse).
- Security → Settings: leave the managed WAF ruleset on (default for Pages
  custom domains); Bot Fight Mode can be turned on for a personal portfolio
  site with no login/forms-with-side-effects beyond the mailto: contact
  form.
- DNSSEC (DNS → Settings): enable once nameservers are confirmed active —
  needs one more step at GoDaddy (adding a DS record GoDaddy gives a field
  for), optional but worth doing for a fuller security posture.

## Step 6 — Verify
- https://juanprieto.us loads the live Pages deployment, padlock valid.
- `curl -I https://juanprieto.us` shows the headers from `_headers`
  (`strict-transport-security`, `content-security-policy`, etc.) and
  `server: cloudflare` instead of `server: GitHub.com`.
- Load from a network that isn't this machine (or ask someone else) to
  confirm the "restricted" intermittent-load issue is gone now that the
  stray A records are no longer in play.
- Once stable, GitHub Pages can stay as a fallback (harmless, since DNS no
  longer points to it) or the custom domain can be removed from the GitHub
  Pages settings — not urgent either way.
