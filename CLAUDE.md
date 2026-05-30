# Blue Horizon Home Care — Company Website

This repo is the **public marketing website for Blue Horizon Home Care**, live at
**https://bluehorizonhomecare.org**. It is a single static page — no build step, no
framework, no dependencies. What you see in `index.html` is exactly what ships.

The business: a **family-owned, veteran-operated** non-medical home care agency serving
**New York**. Primary phone: **(347) 704-0583** · Secondary: **(845) 833-8751** ·
Email: **info@bluehorizonhomecare.org**.

## How it's built

- **`index.html`** — the entire site. HTML, all CSS (in one `<style>` block in the
  `<head>`), and a tiny bit of JavaScript (mobile nav + footer year) at the bottom.
  Everything lives here. There is no React, no Tailwind, no bundler.
- **`assets/`** — images. Currently just `blue-horizon-logo.png`. Reference images with
  **relative paths** (`assets/foo.png`), never absolute (`/assets/foo.png`) — absolute
  paths break on the GitHub Pages preview URL.
- **`CNAME`** — tells GitHub Pages to serve the site on `bluehorizonhomecare.org`. Do
  not delete or rename this file.

## How it's deployed

- Hosted on **GitHub Pages** from the `main` branch of `Tengri-group-inc/BlueHorizonHomeCare`.
- **Every push to `main` automatically redeploys** the live site within ~1–2 minutes.
  There is nothing else to run or click.
- Custom domain `bluehorizonhomecare.org` is configured in repo **Settings → Pages**.

## Editing guide (for Claude Code)

The page is organized as labeled `<section>` blocks, each with an `id`. To change
content, find the matching section:

| Section | `id` | What's in it |
|---|---|---|
| Hero | `top` (and the `.hero` block) | Tagline, headline, sub-copy, the two CTAs, trust dots |
| Services | `services` | The 6 service cards (`service-card`) |
| Why Blue Horizon | `why` | Family-owned/veteran narrative + 4 differentiators |
| How It Works | `process` | 3-step intake flow |
| Testimonial | `testimonial` | The Michelle quote (dark section) |
| Start Care / Contact | `start-care` | Phone/email/hours + the consultation request form |
| Footer | inside `<footer class="site-footer">` | Logo card, link columns, copyright |

### Conventions to follow when editing

- **Colors and fonts** are defined as CSS variables in `:root` near the top of the
  `<style>` block. Use them rather than hardcoding hex values:
  - `--navy` `#0E3A5C` (primary ink for headings)
  - `--navy-deep` `#082338` (footer / dark testimonial section)
  - `--blue` `#1F6FAB` (primary accent, buttons, links)
  - `--blue-light` `#4DABE2` (hover, eyebrow, accents)
  - `--sky` `#E6F1F8` (soft tinted backgrounds)
  - `--bg` `#F7FAFC` (page background)
  - `--surface` `#FFFFFF`, `--ink` `#0E2438`, `--ink-soft` `#4A5B6E`, `--line` `#D9E3EC`
- **Two fonts**: `Fraunces` (serif, for headings — warm, friendly) and `Inter` (sans,
  for body/UI). Loaded from Google Fonts in the `<head>`.
- The nav menu (`<nav id="main-nav">`), the footer "Explore" list, and the in-page CTAs
  all link to section `id`s. **If you add or rename a section, update both menus** so
  links don't break.
- Keep phone numbers consistent in three places: `tel:+13477040583` href + display text
  in (a) the header nav, (b) hero CTAs, (c) the contact list + footer. Same for the
  845 number.
- The form `action="mailto:info@bluehorizonhomecare.org"` is a no-backend fallback —
  submission opens the user's email client. **If/when conversion matters**, swap to a
  hosted form service (Formspree, Basin, Netlify Forms) — that's a one-line `action=`
  change. See "Common tasks" below.

## Common tasks

- **Change wording**: edit the text inside the relevant `<section>`.
- **Add a service**: copy an existing `.service-card` block in `#services` and pick an
  icon from [feathericons.com](https://feathericons.com) (they're 24×24 stroked SVGs,
  which match the existing icon style).
- **Add a testimonial**: the current testimonial section shows one quote. To rotate
  multiple, replicate `.quote` blocks or wrap them in a simple slider (small JS, no lib).
- **Replace the logo**: drop the new file in `assets/`, keep the filename
  `blue-horizon-logo.png` (or update every `<img src="assets/blue-horizon-logo.png">`
  reference and the favicon link in `<head>`). Use a **transparent-background** PNG/SVG.
  The footer keeps the logo on a **white card** (`.footer-brand img { background:#fff }`)
  because the footer is dark navy and the mark itself is dark; if you ever switch to a
  light/inverted logo, remove that white background.
- **Switch the form to Formspree** (or similar) for real lead capture:
  1. Sign up at [formspree.io](https://formspree.io), create a form, copy your endpoint
     (looks like `https://formspree.io/f/xxxxxxx`).
  2. In `index.html`, change the form's
     `action="mailto:info@bluehorizonhomecare.org"` to `action="https://formspree.io/f/xxxxxxx"`,
     remove `enctype="text/plain"`, keep `method="post"`.
  3. (Optional) Add a `<input type="hidden" name="_subject" value="New consultation request">`
     and a `_replyto` field tied to the email input.
- **Change service area copy**: search the file for "New York" — the phrase appears in
  the hero eyebrow, the contact "Serving" line, and the footer. Update all three.
- **Update hours**: the contact-list "Hours" entry in `#start-care` is the source of
  truth. Mirror any change there.

## Git workflow

Keep the history clean and professional — these commits are public on the company's
GitHub.

**Before committing**
- Open `index.html` in a browser and confirm: the logo shows in the header and footer,
  the mobile menu opens, the form submits (opens the mail client), nav links jump to
  the right sections, and the sticky mobile call bar shows on small screens.
- Make sure the commit contains **one logical change**. Don't bundle unrelated edits
  (e.g. a content rewrite and a color change) into the same commit.

**Commit messages**
- Short, imperative subject line, ≤ ~60 chars, no trailing period.
  Good: `Add respite care service card`, `Update primary phone number`,
  `Switch consultation form to Formspree`.
  Avoid: `update`, `changes`, `stuff`.
- If the change needs context, add a blank line then 1–2 sentences explaining **why**.
- **Never** add Claude as author or co-author — no `Co-Authored-By: Claude` trailer and
  no Claude/AI mention. Commits are authored solely by the user.
- Don't commit secrets, API keys, or large unnecessary files.

**Pushing = publishing**
- Every push to `main` goes **live within ~1–2 minutes**. Push only when the change is
  ready for the public to see.
- Want to preview or iterate before going live? Work on a branch, then merge to `main`
  when it's ready.
- If a push is rejected with a "fast-forward" / "fetch first" error, run
  `git pull --rebase origin main` and push again. (GitHub occasionally auto-commits the
  `CNAME` file, which puts a commit on the remote you don't have locally.)

## Using Context7 (live documentation lookup)

Context7 is an MCP tool that fetches **current** docs for libraries, frameworks, SDKs,
APIs, CLIs, and cloud services — useful because model knowledge can be out of date.

- This site is plain HTML/CSS/JS with **no libraries or build step**, so Context7 is
  **rarely needed for routine content edits**. Don't reach for it just to change text.
- **Do** use it when a task brings in something external, e.g.: wiring up Formspree or
  a different form service, adding analytics (Google Analytics, Plausible, Fathom),
  embedding a scheduling widget (Calendly, SimplePractice), pulling in a JS library, or
  questions about **GitHub Pages** behavior or the **`gh` CLI**. In those cases, fetch
  the latest docs via Context7 instead of relying on memory.

## Gotchas & lessons learned

**If a task took several tries or threw confusing errors before you got it working, add
an entry here** — symptom, cause, fix — so the same mistake isn't repeated next time.
Keep entries short. Treat this section as the project's memory.

- **Images must use relative paths, never absolute.** Use `assets/blue-horizon-logo.png`,
  not `/assets/blue-horizon-logo.png`. Absolute paths break on the GitHub Pages preview
  URL.
- **DNS lives in Wix, not a normal registrar.** Nameservers for `bluehorizonhomecare.org`
  are `*.wixdns.net`. Edit records at **Wix → Domains → Manage DNS Records**, not at a
  registrar control panel.
- **In Wix, the root/apex domain = a _blank_ Host name.** Wix's "Add Record" form
  rejects `@` and only creates subdomains. To point the apex (`bluehorizonhomecare.org`),
  leave the Host name field empty. The four GitHub Pages IPs are
  `185.199.108.153`, `.109.153`, `.110.153`, `.111.153`. `www` is a CNAME →
  `tengri-group-inc.github.io`.
- **NEVER touch the email DNS records.** `info@bluehorizonhomecare.org` runs on an
  external mail provider. Leave the MX records and any associated `autodiscover` /
  `enterpriseenrollment` / `enterpriseregistration` / `lyncdiscover` / `sip` (Microsoft
  365) or DKIM/SPF (`_dmarc`, `selector1._domainkey`, etc.) records alone — editing or
  deleting any of them breaks company email. **Only ever touch the website's A and
  `www` records.**
- **The HTTPS certificate lag after a DNS change is normal.** GitHub takes anywhere
  from a few minutes to an hour to issue the SSL cert once DNS is correct. A
  `NET::ERR_CERT_COMMON_NAME_INVALID` browser warning during that window is expected,
  not a bug — it clears itself. Don't repeatedly re-trigger the Pages API; that can
  slow issuance. Just wait, then turn on "Enforce HTTPS."
- **The repo must be public** for GitHub Pages on the free plan (or use a Pro/paid
  plan to keep it private). If `gh api ... /pages` returns a billing/visibility error,
  flip the repo to public with `gh repo edit Tengri-group-inc/BlueHorizonHomeCare --visibility public --accept-visibility-change-consequences`.
