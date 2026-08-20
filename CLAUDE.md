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
  `surface`, `ink`, `inksoft`, `muted`, `line`, `accent`, `accentdark`, `accentlight`) and font
  families (`font-display` = Fraunces, `font-sans` = Inter, loaded from Google Fonts). Reuse these
  tokens rather than introducing new ad-hoc colors.
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

- `photos/IMG_8001.JPG`–`IMG_8006.JPG` — property/lifestyle stock imagery used across the CTA,
  Partners & Countries, and Feedback sections.
- `IMG_7992.JPG`, `IMG_7993.PNG`, `IMG_4913.PNG` (project root) — portrait/collage photos of the
  person the site is built around, used in Hero and About You.
- `zonora-template.webflow.io_ (1).png` — the visual design reference the layout, spacing, and
  component style were built to follow. Consult this before changing section layout or styling.
- `brief.docx` — the original client brief (site goal, section list, asset list). Extract text with
  a zip/XML read if needed (it's a `.docx`, i.e. a zip of XML — `Expand-Archive` requires renaming
  the copy to `.zip` first on Windows).

Note: several of the source photos are AI-generated and show visible artifacts (e.g. garbled
document text reading "Title Deaths" instead of "Title Deeds" in `IMG_8002.JPG`) — the user has
chosen to use them as-is; this is a known, accepted tradeoff, not a bug to silently fix.

## Known placeholder content

The following are intentionally fake and still need real information before this goes live —
don't treat them as settled facts about the business:
- Country/market names in the Partners & Countries section (`[ Country Name ]` placeholders)
- Partner logos (placehold.co placeholders)
- Testimonial quotes and names
- Phone number, office address
- The feedback form has no backend/email integration yet
