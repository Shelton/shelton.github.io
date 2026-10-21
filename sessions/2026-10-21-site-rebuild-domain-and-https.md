# Session: Site Rebuild, sheltondavis.com Domain, and HTTPS

**Date:** 2026-10-21
**Type:** Infrastructure / build / content

## Context
The site was a 2016-era Skeleton template on GitHub Pages at `shelton.github.io`, with no custom domain since 2020. We audited it and found:
- lorem ipsum on the live homepage
- unfinished `play.html` and `project_title.html` stubs
- a 16MB InVision export of Operable 2.0 that nothing linked to
- `http://` assets blocked as mixed content, so Share Tech wasn't loading
- a dead Universal Analytics tag
- a malformed LinkedIn link
- a bio that still had Empathy Lab taking clients

The goal: one page that reflects Shelton's current work, on sheltondavis.com.

## Decisions
- **Mediation leads.** SD Mediates is the practice and DivorceSucks.wtf is the research.
- **Empathy Lab is the umbrella and innovation hub.** It isn't taking clients, and it's home to the Empathy Decathlon® and its coaches.
- **Helper-ID and Provenance Label** are listed as active products under the Empathy Lab umbrella. Helper-ID's domain is **helper-id.com**, with the hyphen; `helperid.com` is a parked domain for sale.
- **Certification wording:** "working toward certification in domestic mediation." Never "certified" until it's true.
- **Removed:** Operable 2.0, Design Cannon, Repurposed Goods, the stub pages, Twitter/X and Dribbble.
- **Visual design: keep the original look.** A new "running track" design (blue lanes, Big Shoulders and Atkinson Hyperlegible) was built, then rejected. What shipped is the old style, modernized:
  - the grey `noisy_grid.png` background
  - Share Tech for all text
  - grey highlighted links that invert on hover
  - magenta text selection and dotted dividers
  - a large circular photo on the left with content on the right
- **Voice:** keep the playfulness ("Keep it simple," "110HH, day two, it won't be pretty"). Tagline: "Mediator, designer, empathy coach, and recovering decathlete" (not "superhero in training").
- **No email addresses on the site, to avoid spam.** Contact goes to each site's existing Formspree form:
  - mediation → `sdmediates.com/#contact`
  - research → `divorcesucks.wtf/#screener`
  - personal → LinkedIn

  A personal Formspree form was considered and deferred until there's a need.

## Built
| File | What |
|---|---|
| `index.html` | One page. Photo on the left. Content: Hello!, What I'm up to (SD Mediates, DivorceSucks.wtf, Helper-ID, Provenance Label, Empathy Lab plus coaches), Before this, Contact. Includes canonical and Open Graph tags. |
| `styles.css` | One stylesheet replacing Skeleton. It keeps the old look with no jQuery, icon fonts or IE hacks. |
| `404.html` | Old style, original copy: "YOU'RE LOST! It's cool, turn around here." |
| `images/shelton-davis.jpg` | Headshot from sdmediates.com, resized to 617×720. |
| `CNAME` | `sheltondavis.com` |
| `.nojekyll` | Serve files as-is (removed the unused `_config.yml` Jekyll theme). |
| `.gitignore` | Adds `.DS_Store`; committed `.DS_Store` files were removed. |
| `README.md` | Explains the structure and how to preview locally. |

The repo went from about 16MB to about 160KB.

Coaches listed, each linked to LinkedIn: Theresa M. Ward (lead facilitator and facilitator trainer), Amy Small, Pame Barba, Mary Bolander, Susan Lambert, Nadine Doyle and Eustacia Mahoney.

Commits: `a2b0a74` rebuild, `c652d7e` restore the original look, `428a99e` Theresa's LinkedIn, `df6a787` contact routing, `59e05f3` Helper-ID, Provenance Label and recovering decathlete.

## Domain and HTTPS
1. GitHub Pages DNS points the apex at `185.199.108–111.153` (A records) and `2606:50c0:8000–8003::153` (AAAA records), with a CNAME from `www` to `shelton.github.io`. DNS for the domain is managed at **WordPress.com** (nameservers `ns1–3.wordpress.com`). GoDaddy is only the registrar.
2. Setting the custom domain in Settings → Pages created a "Create CNAME" commit on GitHub. The local `CNAME` conflicted with it during the rebase, and the conflict was resolved with the same value.
3. The browser error `ERR_CERT_COMMON_NAME_INVALID` meant GitHub was still serving its `*.github.io` certificate. Arc wouldn't allow a bypass because WordPress.com had set HSTS for the domain earlier.
4. The certificate for `sheltondavis.com` was issued later in the session (valid until 2027-01-19). GitHub then showed "Detected a change to DNS settings. Requesting a new certificate," most likely to add `www`.
5. Shelton confirmed the site loads on a phone.

## Lessons
- **GitHub account:** this machine's `gh` and git credentials are **TheEmpathyLab**, not **Shelton**. The first push got a 403. Fixed by adding TheEmpathyLab as a collaborator on `Shelton/shelton.github.io`.
- **An old commit was never pushed.** `687d11e Updates` (Nov 2025) only existed locally. It went up with the rebuild, and the rebuild replaced its changes.
- **Local preview:** run `python3 -m http.server 8931 --bind 127.0.0.1` from the repo root and open http://localhost:8931. Port 8765 was already taken by another local server, probably DivorceSucks.wtf.
- **Headless Chrome can't do a 390px window.** It enforces a minimum width of about 500px, so check phone layouts by loading the page in a 390px iframe.

## Open items
- **Check Enforce HTTPS** in Settings → Pages once the checkbox is available (after the www certificate finishes).
- **Verify the domain** under profile Settings → Pages → Add a domain (TXT record at WordPress.com DNS). This protects against domain takeover.
- **Confirm the LinkedIn URL** `linkedin.com/in/sheltonjdavis` goes to the right profile. LinkedIn blocks automated checks.
- **Bio wording:**
  - "competitive athlete" sits oddly next to "recovering decathlete." Drop it or reword it.
  - Confirm "son, friend, husband, father" is still current.
- **Photo crop:** the circle cuts off part of the "empathy" on the shirt. Move the crop or leave it.
- **Old email exposure:** `shelton@sheltondavis.com` was on the old site for years. Filter or alias it if spam shows up.
- **Optional, later:**
  - a personal Formspree form (name, email, message only, with a honeypot) if LinkedIn isn't enough
  - replacement analytics, if wanted
  - an updated favicon or apple-touch icon

## Sessions folder decision
- **`sessions/` lives at the repo root and is committed,** so it's a referable source of truth. It's public at `sheltondavis.com/sessions/…`. `robots.txt` asks search engines not to index it.
- Created the **session-notes** personal skill (`~/.claude/skills/session-notes/SKILL.md`), based on the sdmediates and provenancelabel session files, so every project uses the same format.
