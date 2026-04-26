# Personal Website

Hosted on Cloudflare Pages with automatic deployments from GitHub.

## Structure

- `index.html` — Main page
- `_headers` — Security headers (applied by Cloudflare Pages)
- `_redirects` — URL redirects (optional)

## Security Headers

The `_headers` file enforces:
- No iframe embedding (X-Frame-Options: DENY)
- No MIME sniffing (X-Content-Type-Options: nosniff)
- Strict HTTPS (HSTS with preload)
- Strict CSP (no external scripts/styles by default)
- Cross-origin isolation (COOP/COEP/CORP)
- Disabled browser permissions (camera, mic, geolocation, payment)

## Deployment

Push to `main` → Cloudflare Pages auto-deploys to `*.pages.dev`.

## Local Preview

```bash
python3 -m http.server 8000
# Open http://localhost:8000
```