# ModelCollectr Landing Site — Claude Project Context

## What This Is

Marketing and distribution site for the ModelCollectr desktop app. Static HTML/CSS/JS deployed to Cloudflare Pages.

- **Local path:** ~/projects/modelcollectr
- **Live URL:** https://www.modelcollectr.co.uk
- **Repo:** ChrisBaldwin984/modelcollectr (PUBLIC — also hosts .exe release assets)
- **Deploy:** git push to main → Cloudflare Pages auto-builds

## Key Pages

| File | Purpose |
|------|---------|
| `index.html` | Landing/marketing page |
| `download.html` | Fetches latest .exe from GitHub releases API and auto-downloads |
| `success.html` | Post-payment confirmation — polls `/license/lookup` and shows key |
| `privacy.html` | Privacy policy (GDPR compliant — email held by Stripe, not us) |
| `cancel.html` | Stripe checkout cancelled page |
| `version.json` | `{"version":"1.2.12"}` — desktop app polls this on startup for update check |

## Important Details

### success.html
- Polls `https://api.modelcollectr.co.uk/license/lookup?session_id=xxx` every 2s, up to 30s
- Shows loading spinner → key display → copy button on success
- CSS fix: `show()` function must set `display:'block'` not `display:''` — empty string falls back to stylesheet `display:none`

### version.json
- Must be bumped when a new desktop build is released
- Desktop app fetches this on startup to show "update available" banner

### Privacy Policy (privacy.html)
- Email is held by Stripe only, never stored in our database
- Distribution: Microsoft Store only (no Mac/Linux)
- Updated 2026-05-17

### URLs
- All internal/external links must use `https://www.modelcollectr.co.uk` (www)
- Bare domain `modelcollectr.co.uk` is redirected to www by the Cloudflare Worker

## Fonts & Branding
- Headings: Crimson Pro (serif)
- Body: Instrument Sans (sans-serif)
- Colours: navy `#1c1f26`, gold `#e8920a`, cream `#ede9e0`

## Release Asset Hosting
The PUBLIC `modelcollectr` GitHub repo also hosts .exe release assets uploaded by the desktop build workflow. `download.html` fetches:
```
https://api.github.com/repos/ChrisBaldwin984/modelcollectr/releases/latest
```
and auto-downloads the first `.exe` asset found.

## Deploy Process
```bash
cd ~/projects/modelcollectr
git add -p          # stage specific changes
git commit -m "..."
git push origin main
```
Cloudflare Pages picks up main branch automatically.
