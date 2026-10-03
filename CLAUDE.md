# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Static bilingual marketing/portfolio site for Twin Site Creator, live at https://twinsitecreator.com/. Plain HTML5 + CSS3 + vanilla JS — no package manager, build step, linter, or tests. Edit files directly; to preview, serve the repo root (e.g. `python3 -m http.server 8000`) rather than opening files via `file://`, because the language switcher and `hreflang`/canonical URLs use root-absolute paths (`/en/...`).

## Rules

- The site is multilingual: any change to text or markup in one language must be made in every language version too.
- Code style: plain CSS with BEM naming (in `assets/css/style.css`; no SCSS/preprocessor yet — SCSS is planned for later), vanilla JS. Do not add new libraries without the owner's permission.
- Before large changes, first describe a short plan and wait for confirmation.
- Never `git commit` or `git push` unless explicitly asked.

## Structure

- Ukrainian pages live at the root (`index.html`, `<section>/index.html`); English mirrors live under `en/` with identical folder names (`en/index.html`, `en/<section>/index.html`). Six service sections exist in both languages: `website-development`, `landing-page-development`, `frontend-development`, `cms-platforms`, `website-optimization`, `ui-responsive-design`.
- `index-en.html` is only a redirect to `/en/`. `twinsite-notary-email.html` is a standalone email template, not part of the site.
- Every page is self-contained: header, nav, footer, cookie banner, GA snippet and JSON-LD are duplicated in each file. A change to shared markup must be applied to all 14 pages (UA + EN).
- Asset paths are relative and depth-dependent: `assets/…` from the root homepage, `../assets/…` from root section pages and `en/index.html`, `../../assets/…` from `en/<section>/`.

## Shared assets

- `assets/css/style.css` — the single stylesheet for all pages (no inline `<style>` blocks).
- `assets/js/script.js` — global behaviour: sticky header, mobile nav, active-link highlighting, `.reveal` scroll animations, hero video, back-to-top, FAQ accordion, cookie consent, and the `.skills` observer (assumes a `.skills` element exists on the page).
- `assets/js/gallery.js` — portfolio gallery animations; exits early if `#gallery` is absent.
- Pages also link `assets/css/gallery.css`, which does not exist in the repo (404 on every page).

## When adding or changing a page

Keep both language versions in sync and update, per page:
- `<html lang>` (`uk` / `en`), `<title>`, meta description, OG tags (`og:url`, image).
- `rel="canonical"` plus `hreflang` alternates for `uk`, `en` and `x-default` (x-default points to the Ukrainian URL).
- The `.lang-toggle` links (`/…` ↔ `/en/…`).
- JSON-LD blocks (`application/ld+json`: Organization/ProfessionalService/Service/FAQPage etc.) — keep FAQ schema consistent with visible FAQ text.
- `sitemap.xml` — lists all 14 URLs.

## Analytics

Google Analytics ID `G-CQWCQ5EF1Y` is included inline in each page's `<head>` and is also loaded by `loadAnalytics()` in `script.js` after cookie-consent "accept". The inline snippet fires regardless of consent — be aware of this when touching either.
