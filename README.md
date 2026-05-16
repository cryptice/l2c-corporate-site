# L2 Consulting AB — Website

Single-page static site for L2 Consulting AB.

- **Live:** https://l2c.se
- **Stack:** Plain HTML + CSS, no build step
- **Host:** GitHub Pages

## Update content

Edit `index.html` (text and structure) and `styles.css` (look). Push to `main` — GitHub Pages redeploys automatically.

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Any static file server works.

## Deploy (first time)

1. Push this repo to GitHub.
2. Repo Settings → Pages → Source: `Deploy from a branch`, Branch: `main`, Folder: `/ (root)`.
3. Custom domain: `l2c.se` (already set in `CNAME`).
4. Tick **Enforce HTTPS** once the certificate is issued (can take a few minutes).

### DNS

For apex `l2c.se`, set A records to GitHub's four published IPs:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

For `www.l2c.se`, set a CNAME to `<github-username>.github.io`.

GitHub's current Pages IPs: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site
