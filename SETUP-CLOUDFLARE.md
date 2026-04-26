# Cloudflare Pages Setup Guide

## Prerequisites

- [x] GitHub repo created: https://github.com/to39r5qo8yp-cpu/personal-website
- [ ] Cloudflare account (free)
- [ ] Your actual HTML file placed in the repo

## Step 1: Create a Cloudflare Account

1. Go to https://dash.cloudflare.com/sign-up
2. Sign up with email (or GitHub/Google SSO)
3. No credit card required for the free tier

## Step 2: Connect GitHub Repository

1. Log in to the Cloudflare dashboard
2. Go to **Workers & Pages** → **Create application** → **Pages** tab
3. Click **Connect to Git**
4. When GitHub asks for authorization:
   - **Important for security:** Select **"Only select repositories"**
   - Check ONLY `personal-website` — do NOT grant access to all repos
5. Click **Install & Authorize**

## Step 3: Configure Build Settings

When prompted for build configuration:

| Setting | Value |
|---------|-------|
| **Project name** | `personal-website` (or your preferred subdomain name — this becomes `name.pages.dev`) |
| **Production branch** | `main` |
| **Framework preset** | `None` |
| **Build command** | Leave empty (or type `exit 0`) |
| **Build output directory** | `/` (root — since all files are in the repo root) |
| **Root directory** | `/` (default) |

6. Click **Save and Deploy**
7. First build runs automatically and deploys in ~30 seconds

Your site will be live at: `https://personal-website.pages.dev`

## Step 4: Configure Security in Cloudflare Dashboard

After deployment, go to your project settings and apply these:

### SSL/TLS Settings
1. Go to your project → **Settings** → **SSL/TLS**
2. Set encryption mode to **Full (strict)**
3. Set **Minimum TLS Version** to **TLS 1.2**
4. Enable **Always Use HTTPS**
5. Enable **Automatic HTTPS Rewrites**

### Access Controls
1. Go to **Settings** → **Access**
2. Consider enabling **Cloudflare Access** for preview deployments if needed

### Branch Controls
1. Go to **Settings** → **Builds** → **Branch control**
2. Set production branch to `main`
3. Under "Preview branches", you can restrict which branches get preview URLs

## Step 5: Verify Security Headers

After deployment, verify the _headers are working:

```bash
curl -sI https://personal-website.pages.dev | grep -E "X-Frame|X-Content|Strict-Transport|Content-Security"
```

Expected output:
```
x-frame-options: DENY
x-content-type-options: nosniff
strict-transport-security: max-age=31536000; includeSubDomains; preload
content-security-policy: default-src 'self'; script-src 'self'; ...
```

## Step 6: (Optional) Add Custom Domain

When you purchase a domain:

1. Go to your project → **Custom domains** → **Set up a custom domain**
2. Enter your domain name
3. Cloudflare will configure DNS automatically (if using Cloudflare DNS)
4. SSL certificate is provisioned automatically (~30 seconds)
5. Update the _headers file if you add external resources (fonts, analytics, etc.)

### Cheap Domain Options
- Cloudflare Registrar (at cost, no markup) — best if your domain uses Cloudflare DNS
- Porkbun (.xyz domains ~$2-3/year first year)
- Namecheap (.me domains ~$3/year first year)

## Updating the Site

Just push to `main`:

```bash
# Replace with your actual HTML file
cp /path/to/your-site.html index.html
git add index.html
git commit -m "Update website"
git push
```

Cloudflare Pages auto-deploys on every push to `main`. Preview URLs are generated for pull requests.

## Security Summary

| Layer | Protection |
|-------|------------|
| **Transport** | TLS 1.2+, HSTS preload, automatic HTTPS |
| **Headers** | CSP, X-Frame-Options DENY, COOP/COEP/CORP, no-sniff |
| **Permissions** | Camera, mic, geolocation, payment all disabled |
| **GitHub scope** | Cloudflare app scoped to single repo only |
| **No secrets** | No API keys, tokens, or credentials in the repo |
| **DDoS** | Cloudflare absorbs attacks before they reach origin |
| **Isolation** | Zero attack surface on your Oracle instance |