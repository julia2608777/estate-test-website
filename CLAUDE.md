# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page marketing/branding website for "G Vision" (name inferred from the project's contact
email — confirm the real business/personal name before launch), a real-estate advisory business.
There is no build system, package manager, or framework: `index.html` is the entire site.

## Running it

There is no dev server, build, lint, or test command in this repo. Neither Node nor a working Python
is available in this environment as of this writing (`python` resolves to a broken Windows Store
stub, `node` is not installed) — do not assume `node serve.mjs`, `npm run *`, or similar will work
without first confirming the tool is actually installed.

To preview: open `index.html` directly in a browser. All image paths are relative, so this works
from `file://` as well as from any static file server.

## Architecture

Everything lives in `index.html`:
- **Styling**: Tailwind CSS via CDN (`<script src="https://cdn.tailwindcss.com">`) with an inline
  `tailwind.config` extending the default theme with the project's color tokens (`bg`, `bgalt`,
  `surface`, `ink`, `inksoft`, `muted`, `line`, `accent`, `accentdark`, `accentlight` — a monochrome
  black/white/gray palette matching the Zonora reference, not a warm/colored one) and font families
  (`font-display` and `font-sans` both map to DM Sans, loaded from Google Fonts, matching the single
  sans-serif family used throughout the Zonora reference — confirmed against its computed CSS and a
  close-up crop of its headline). Reuse these tokens rather than introducing new ad-hoc colors or
  fonts.
- **Layout**: one `<section>` per page block, in this fixed order: Header → Hero → Services → Call
  to Action → About You → Partners & Countries of Operation → Work Process → Testimonials →
  Feedback Form → Contacts. This order matches the client brief (`brief.docx`) and should be
  preserved unless the user asks to reorder.
- **JS**: a single inline `<script>` at the bottom of the file handles the mobile nav toggle,
  scroll-triggered fade-ins (`IntersectionObserver` + `.fade-up`/`.in-view`), the testimonials
  slider (two slide groups, prev/next + dots), and the feedback form (front-end only — `preventDefault`
  + show a success message, no backend wired up).
- **Custom CSS** (shadows, grain/noise texture, fade-up transition, slider transform) lives in the
  `<style>` block in `<head>`, not inline on elements.

## Assets

- `photos/IMG_8001.JPG`–`IMG_8006.JPG` — property/lifestyle stock imagery used EXCLUSIVELY in the
  Services, Work Process, Testimonials, and Feedback sections.
- `IMG_7992.JPG`, `IMG_8162.JPG`, `IMG_8163.JPG` (project root) — portrait photos of the person the
  site is built around, used ONLY in the Hero (badge + video poster), Call to Action, and About You
  sections/author blocks. Do not reuse these outside of top-section/author contexts, and don't pull
  from `photos/` for those sections either.
- `IMG_8166.MP4` (project root) — the Hero section's autoplaying background video (muted, looped,
  `playsinline`). Same usage restriction as the root portrait photos: top-of-page/author content only.
- `IMG_8164.MP4` (project root) — an autoplaying, muted, looped video (`playsinline`) used in the
  Work Process section in place of the old `photos/IMG_8006.JPG` still image (kept as the video's
  `poster`). This is a deliberate exception to the portrait-only root-asset rule above — it's a
  root-level file but not treated as a portrait/author asset, and it's used in a middle section, not
  Hero/About/CTA.
- `zonora-template.webflow.io_ (1).png` — the visual design reference the layout, spacing, and
  component style were built to follow. Consult this before changing section layout or styling.
- `brief.docx` — the original client brief (site goal, section list, asset list). Extract text with
  a zip/XML read if needed (it's a `.docx`, i.e. a zip of XML — `Expand-Archive` requires renaming
  the copy to `.zip` first on Windows).

Note: several of the source photos are AI-generated and show visible artifacts (e.g. garbled
document text reading "Title Deaths" instead of "Title Deeds" in `IMG_8002.JPG`) — the user has
chosen to use them as-is; this is a known, accepted tradeoff, not a bug to silently fix.

The previous portrait set (`IMG_7993.PNG`, `IMG_4913.PNG`) has been removed from the project and
replaced by the files above — don't reference the old filenames.

## Illustrative content — confirm before launch

Per the client's explicit instruction, the page ships with no bracketed placeholders or empty
fields — every block is filled with realistic, complete copy. The following, however, are still
fictional/illustrative and must be swapped for the real thing before this goes live:
- The founder name ("Gabrielle Voss") and bio details in Hero/About/Contacts
- Country list in the Partners & Countries section (currently: US, UK, Switzerland, UAE, Portugal,
  Singapore, France, Spain)
- Partner names/logos (placehold.co images — Meridian Realty, Northbridge Capital, etc.)
- Testimonial quotes, names, and cities
- Office address and phone number (currently a placeholder NYC address and a 555 number)
- The feedback form has no backend/email integration yet (front-end only: `preventDefault` + success message)
