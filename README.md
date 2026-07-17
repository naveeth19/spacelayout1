# SpaceLayout — Website Project

**Architecture • Design • Interiors • Construction — Bengaluru**

## Folder layout

| Folder | Contents |
|---|---|
| `site/` | **The website** — Next.js 16 app. `cd site && npm run dev` to work on it, `npm run build` to build. |
| `design/` | Design references: the Claude Design hero export (`design_handoff_spacelayout_hero/` with `SpaceLayout Hero.dc.html`), the original handoff ZIP, and the hero facade source PNG. |
| `assets/brand/` | Logo source files (new SpaceLayout logos + legacy marks). The production logo used by the site is `site/public/brand/spacelayout-logo-white.svg`. |
| `assets/photos/` | `project_photos/` (88 photos organised by project) and `raw_photos/` (raw Canva extracts). Web-optimised copies live in `site/public/projects/`. |
| `archive/` | Superseded material: the old static site (`old-site/`), Canva PPTX exports (`canva-exports/`), and design-iteration screenshots (`screenshots/`). Safe to delete if space is needed. |

## Quick start

```bash
cd site
npm install
npm run dev        # http://localhost:3000
```

## Still to do

- Wire a real mail provider into `site/app/api/enquire` (currently logs only)
- Deploy to Vercel
- Replace placeholder photos for the Likith Residence project
