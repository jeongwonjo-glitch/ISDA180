# ISDA 180 · weekly learning site

Static site. No build step, no dependencies, no backend. Every page is one
self-contained HTML file: open any of them from disk and it works offline.

```
site/
  index.html              course index
  week-01/
    index.html            week overview  →  /week-01/
    lesson-1/index.html   →  /week-01/lesson-1/
    lesson-2/index.html   →  /week-01/lesson-2/
    lesson-3/index.html   →  /week-01/lesson-3/
  week-02/
    index.html            week overview  →  /week-02/
    lesson-1/index.html   →  /week-02/lesson-1/
    lesson-2/index.html   →  /week-02/lesson-2/
    lesson-3/index.html   →  /week-02/lesson-3/
    lesson-4/index.html   →  /week-02/lesson-4/
    lesson-5/index.html   →  /week-02/lesson-5/
  week-03/
    index.html            week overview  →  /week-03/
    lesson-1/index.html   →  /week-03/lesson-1/
    lesson-2/index.html   →  /week-03/lesson-2/
    lesson-3/index.html   →  /week-03/lesson-3/
    lesson-4/index.html   →  /week-03/lesson-4/
  week-04/
    index.html            week overview  →  /week-04/
    lesson-1/index.html   →  /week-04/lesson-1/
    lesson-2/index.html   →  /week-04/lesson-2/
    lesson-3/index.html   →  /week-04/lesson-3/
```

## Deploying

Vercel, connected to this repository:

1. vercel.com → Add New → Project → import this repo
2. Framework preset: **Other**
3. Root directory: **`site`**
4. Build command: leave empty · Output directory: leave empty
5. Settings → Deployment Protection → **off** (students must reach it without a Vercel login,
   and Canvas cannot iframe a login-gated page)

Every push to `main` redeploys at the same URL.

## Using it in Canvas

Both ways work, and the deadline pages use both:

- **Module link** — Modules → + → External URL → the page URL, e.g.
  `https://<your-domain>/week-01/lesson-2/`. Check "Load in a new tab".
- **Embedded** — a Canvas Page, HTML editor:

  ```html
  <p><iframe src="https://<your-domain>/week-01/lesson-2/"
    width="100%" height="1000" style="border:0"
    title="ISDA 180 Week 1 Lesson 2"></iframe></p>
  ```

  Canvas does not grow an iframe to fit its content, so the page scrolls inside the frame.
  Under 840px wide the sidebar collapses to a top bar, so a narrow Canvas column still reads.

## Editing

These files are **generated** from the design project (`Week 1 Overview.dc.html`,
`W01 Lesson 1/2/3.dc.html`, `Week 2 Overview.dc.html`, `W02 Lesson 1–5.dc.html`,
`Week 3 Overview.dc.html`, `W03 Lesson 1–4.dc.html`,
`Week 4 Overview.dc.html`, `W04 Lesson 1–3.dc.html`) — the
bundler inlines the design system, the interaction library, and the video manifest into each page. Editing `site/*.html` by hand works but is
lost on the next regeneration.

Two things change most often, and both live in the design project:

- **`w01-video-manifest.js` … `w04-video-manifest.js`** — Panopto video
  ids, durations, chapter timestamps, one file per week. One edit updates every timestamp chip,
  duration pill and sidebar line on that week's pages. Weeks 2–4 `duration` fields are
  still `null`; filling them turns on the `video 9:40` pills on the week overview cards and lesson
  headers.
- **`course-map.js`** — the week and lesson list the sidebar dropdown and progress rail read from.
  Adding Week 5 starts here.

## Student state

Checkboxes and activity answers persist in the browser under `isda180:<lesson-id>` keys.
Nothing is transmitted, nothing is recorded, and no page has a form or a backend — everything
graded leaves through a Canvas link. Renaming a storage key erases that lesson's saved state.
