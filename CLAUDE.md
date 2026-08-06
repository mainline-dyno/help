# Mainline Dyno Help — AI editing guide

This repository is the **Mainline Dyno** public help site, built with [Hugo](https://gohugo.io/) and the [Relearn](https://mcshelby.github.io/hugo-theme-relearn/) theme. Live site: https://help.mainlinedyno.com.au/

Use this file when adding or editing help topics so changes match existing conventions.

## Stack (do not reinvent)

| Piece | Location / notes |
|--------|------------------|
| Config | `hugo.toml` |
| Theme | `themes/hugo-theme-relearn/` (vendored; avoid editing unless required) |
| Content | `content/**/*.md` |
| Images | `img/` (mounted to `static/img` — see `hugo.toml`) |
| Build output | `public/` (gitignored) |

Preview locally:

```bash
hugo server
```

Build check:

```bash
hugo
```

## Content layout

Sections are folders under `content/` with an `_index.md`. Pages are **flat markdown files** next to that section index — **not** nested page bundles.

```
content/
  _index.md                          # home
  quickstart/
    _index.md
    ccs-cabinet-connections.md
  functions/
    _index.md
    maths.md
    obd2.md
    ramptestsetup.md
    dragtest.md
    drivetest.md
    canpresets.md
  accessories/
    _index.md
    ex7.md
```

### Adding a new page

1. Pick the right section (`quickstart`, `functions`, `accessories`, or a new section).
2. Create a **single** file: `content/<section>/<slug>.md`  
   Example: `content/functions/my-feature.md` → URL `/functions/my-feature/`
3. Do **not** create `content/<section>/<slug>/index.md` with images beside it.
4. Section `_index.md` already lists children via:

   ```md
   {{% children sort="weight" depth=2 description="true" %}}
   ```

   New pages appear automatically once they have front matter. No manual menu edit required.

### New section (rare)

1. `content/<section>/_index.md` with `title` and `weight`.
2. Include the `children` shortcode (copy from `content/functions/_index.md`).
3. Add page files under that folder.

## Front matter

Minimum for a page:

```yaml
---
title: "Human-readable title"
description: "One-line summary for listings and SEO"
weight: 50
---
```

- `title` — shown in nav and page heading.
- `description` — used by the `children` shortcode when `description="true"`.
- `weight` — menu / children order (lower first). Existing pages often omit it; set it when order matters.

Section indexes only need `title` and `weight`.

## Images (important)

All images live under the repo root **`img/`**, grouped by topic. Hugo mounts `img` → `static/img`.

```
img/
  prohub.jpg
  obd2/
  ex7/
  dragtest/
  drivetest/
  canpresets/
  css-cabinet-connections/
```

### Rules

1. Put new screenshots in `img/<topic>/` (create the folder if needed). Prefer clear names: `drag-test-tab.jpg`, not hash names.
2. From a page under `content/<section>/`, reference images as:

   ```md
   ![Descriptive alt text](../../img/<topic>/filename.png)
   ```

3. Absolute-style paths also work (used in some older pages):

   ```md
   ![ProHub 2WD Models](</img/css-cabinet-connections/image2.jpg>)
   ```

   Prefer the relative `../../img/...` form for new work (matches `obd2`, `ex7`, drag/drive/CAN pages).

4. **Never** put images inside `content/` (no leaf bundles with co-located images). That diverges from this site’s layout and confuses contributors.

5. Use meaningful alt text, not `alt text`.

## Writing style

- Audience: dyno operators and techs using **Mainline CCS** software and hardware.
- Prefer step-by-step procedures, UI labels in **bold**, and short tables for settings.
- UI paths: `Setup → Channels`, tab names, button labels as the software shows them.
- Keep product terms consistent: CCS, ProHub, CAN, Hub RPM, Engine RPM, etc.
- Do not invent product behaviour; only document what the user/source material states.

## What not to change casually

- `themes/hugo-theme-relearn/` — upstream theme; leave alone unless fixing a theme bug.
- `hugo.toml` mounts — `img` → `static/img` is required for image URLs.
- `baseURL` / deploy workflow under `.github/workflows/` unless you intend a site/config change.
- Do not commit `public/` or `.hugo_build.lock`.

## Quick checklist for a new help topic

- [ ] Flat page: `content/<section>/<slug>.md` (not a folder + `index.md`)
- [ ] Front matter: `title`, `description`, optional `weight`
- [ ] Screenshots in `img/<topic>/`
- [ ] Image links: `../../img/<topic>/file.ext` with real alt text
- [ ] `hugo` / `hugo server` — page in nav under the section, images load
- [ ] No images left under `content/`

## Example skeleton

`content/functions/example-feature.md`:

```markdown
---
title: "Example Feature"
description: "How to configure the example feature in CCS"
weight: 70
---

## Overview

Short what/why.

## Procedure

1. Open **Setup → Example**.
2. Enable the feature and set the interval.

![Example settings](../../img/example-feature/settings.png)

## Notes

Any caveats or related pages.
```

Images for that page: `img/example-feature/settings.png`.
