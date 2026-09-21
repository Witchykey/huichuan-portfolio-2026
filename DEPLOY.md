# Deploying to slowllm.com on Cloudflare Pages

Everything in this folder is a static file. No build step, no dependencies, no framework.

## Files

| File | What it does |
|---|---|
| `index.html` | The site. Self-contained — all CSS is inline. |
| `404.html` | Not-found page. Cloudflare Pages serves this automatically. |
| `og-image.png` | Social preview card (1200×630) for LinkedIn, Slack, iMessage, X. |
| `favicon.svg` | Browser tab icon. |
| `robots.txt` | Allows crawling, points to the sitemap. |
| `sitemap.xml` | One URL. Update `lastmod` when you change the page. |
| `_headers` | Security headers and cache rules. Cloudflare Pages reads this. |
| `_redirects` | Sends `www.slowllm.com` to the apex domain. |
| `DEPLOY.md` | This file. Safe to delete before uploading. |

## Option A — drag and drop (fastest, about 5 minutes)

1. Sign in at `dash.cloudflare.com`.
2. Left sidebar: **Workers & Pages** → **Create** → **Pages** tab → **Upload assets**.
3. Name the project, e.g. `slowllm`.
4. Drag this entire folder into the upload area. Deploy.
5. You'll get a live `your-project.pages.dev` URL. Open it and confirm the page looks right.

Then attach the domain:

6. In the project: **Custom domains** → **Set up a custom domain** → enter `slowllm.com`.
7. If `slowllm.com` is already on your Cloudflare account, the DNS record is created for you. If the domain is registered elsewhere, Cloudflare will show you the nameservers to point at it — that change can take a few hours to propagate.
8. Optional: repeat for `www.slowllm.com`. The `_redirects` file then forwards it to the apex.

HTTPS is automatic. No certificate setup.

To update later, go to the project and upload the folder again — it creates a new deployment and you can roll back to any previous one.

## Option B — connect a Git repo (better if you'll edit often)

1. Put this folder in a GitHub repository.
2. Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
3. Select the repo. Leave the build command **empty** and set the build output directory to `/` (or to the folder name if the files aren't at the repo root).
4. Deploy, then attach the custom domain as in steps 6–8 above.

Every push to the main branch redeploys automatically.

## Using a subdomain instead

If you'd rather have `huichuan.slowllm.com`, use that hostname in the custom-domain step, then find-and-replace `https://slowllm.com/` with `https://huichuan.slowllm.com/` in:

- `index.html` — the canonical link, the four `og:`/`twitter:` URLs, and the JSON-LD `url`
- `sitemap.xml` — the `<loc>` value
- `robots.txt` — the sitemap line

## After it's live

- Open the page on your phone. The layout is responsive but worth seeing for yourself.
- Paste the URL into Slack or iMessage to confirm the preview card renders.
- In the Cloudflare project, **Analytics** gives you pageviews with no script to add and no cookie banner needed.
- If you later add a resume PDF, drop it in this folder as `huichuan-li-resume.pdf` and link to `/huichuan-li-resume.pdf` from the contact section.

## Things that are deliberately absent

- No contact form. A form needs somewhere to POST; the `mailto:` link works everywhere and costs nothing.
- No third-party analytics. Cloudflare's built-in analytics covers it without slowing the page or requiring a consent notice.
- No JavaScript. The page is HTML and CSS only, which is why it loads instantly.
