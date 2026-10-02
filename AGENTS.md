# AGENTS.md — PKBM

## What this is
Website **PKBM Lam Alif** (Pusat Kegiatan Belajar Masyarakat) — konversi dari stock **TemplateMo 590 "Topic Listing"** Bootstrap 5 static template. Seluruh copy sudah bahasa Indonesia. 4 pages, all relative links:
- `index.html` — single-page home, sections `#section_1` … `#section_5` (Beranda, Program, Cara Mendaftar, FAQ, Kontak)
- `topics-listing.html` (daftar program) → `topics-detail.html` (detail Program Paket C; satu halaman detail untuk semua kartu)
- `contact.html` — form is `action="#"`, no backend

No `package.json`, no build, no tests, no lint, no own git repo. Do not look for npm scripts; there are none.

## Running
- Preview: open `index.html` directly, or serve statically (`npx serve .`, `python -m http.server`). Nothing to install.
- Google Fonts (Montserrat/Open Sans) come from CDN — offline preview loses typography; vendored Bootstrap Icons fonts live in `fonts/`.

## Structure gotchas (easy to miss)
- **Navbar (desktop + mobile collapse) and footer are copy-pasted into all 4 HTML files** (nav appears twice per file). Any nav/footer/link edit must be repeated in every `.html`.
- Nav labels are Indonesian: Beranda, Program, Cara Mendaftar, FAQ, Kontak + dropdown "Halaman" (Program Belajar → `topics-listing.html`, Hubungi Kami → `contact.html`). On `index.html` the 5 links are `href="#section_N"`; on the other 3 pages they are `index.html#section_N`. Footer links also use `index.html#section_N` (must work from every page).
- `js/click-scroll.js` is loaded **only in `index.html`** and hardcodes `sectionArray = [1..5]`. It highlights nav items **by DOM position** (nth `.click-scroll` ↔ nth `#section_N`). Adding/removing a nav item or section breaks active-state highlighting; keep the three orders (nav links, section ids, `sectionArray`) in sync.
- `js/custom.js`'s scroll handler dereferences `#vertical-scrollable-timeline` (exists only in `index.html`) **inside** a loop over its `li`s — other pages survive only because that list is empty. Keep the dereference inside the loop or non-index pages throw on scroll.
- Script order at end of body is fixed: `jquery.min.js` → `bootstrap.bundle.min.js` → `jquery.sticky.js` → `click-scroll.js` (index only) → `custom.js`. Sticky navbar is produced by `jquery.sticky.js` auto-init (`$(".navbar").sticky({topSpacing:0})`), not CSS.
- Tab ids in the Program section are still the template's (`design-tab`, `marketing-tab`, `finance-tab`, `music-tab`, `education-tab`) but labels are Paket A/B/C, Kursus, Kegiatan — ids are referenced by `data-bs-target`; renaming ids requires updating both sides.
- `index.html` explore section has **one unclosed `<div>`** (container opened before the tabs, 136 opens / 135 closes) — this is present in the official TemplateMo demo too; browsers auto-close it. Do not "fix" it casually or diff against upstream will confuse you.
- Do not edit vendored minified files: `css/bootstrap.min.css`, `css/bootstrap-icons.css`, `js/bootstrap.bundle.min.js`, `js/jquery.min.js`, `js/jquery.sticky.js`.

## Known issues / edits
- **Placeholder contact data** to replace with real info: addresses (`Jl. Pendidikan Raya No. 12` / `Jl. Melati No. 45, Banda Aceh`), phones (`+62 812-3456-7890`, `+62 813-7890-1234`), email `info@pkbmlamalif.or.id`, and both Google Maps iframes (`maps?q=PKBM+Lam+Alif+Indonesia&output=embed`) — they appear in `index.html` + `contact.html` (footer phone/email in all 4 files).
- Broken image from the template (`businesswoman-...-office-room.jpg`) was fixed → `images/businesswoman-using-tablet-analysis.jpg`.
- Theme: change the CSS custom properties in `:root` of `css/templatemo-topic-listing.css` (primary `#13547a`, secondary `#80d0c7`, section bg `#f0f8ff`) instead of per-rule colors.
- Header backgrounds (`.hero-section` + `.site-header` on all pages) are overridden by a **slideshow block appended at the end of `css/templatemo-topic-listing.css`**: base layer `images/16.jpeg`, `::before` = `17.jpeg`, `::after` = `18.jpeg`, crossfading every 5s (15s cycle, semi-transparent brand gradient overlay keeps white text readable). To swap photos, edit the three `url("../images/…")` there — no HTML changes needed. The original gradient rule higher in the file is intentionally left untouched (overridden by specificity/order).
- Footer credit + HTML comment carry TemplateMo attribution; the template must not be redistributed in another template collection (stated in original `index.html` footer text — keep the `Design: TemplateMo` line).
