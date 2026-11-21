# Overnite website

One-page site for Overnite: https://overnite-records.vercel.app

Plain static HTML, no build step. Everything (text, styles, script) is in `index.html`.

## Edit

- Text: search for the sentence in `index.html` and change it.
- Photos: `assets/img/` (the one remaining photo, in "Why Overnite", is a Wikimedia stand-in, credited in `credits.html`).
- Edit videos in the grid: `assets/edits/` (one square `.mp4` plus a `.jpg` poster each). To swap one, replace both files with the same names, or add a new `<figure class="tile">` in `index.html`.
- Hero background: `assets/hero/hero-row-a.mp4` and `hero-row-b.mp4`. Each row is 10 edits stitched side by side into one video (2560x240), so only a few videos play at once.
- Numbers: every number is a `<button class="num">` with `data-kind`, `data-num`, `data-title`, `data-text`, `data-note` and optional `data-media` (edit clip names from `assets/edits`, comma separated). Clicking it opens the drawer with that text, so to change what a number says, edit those attributes.
- Our creator base (`#numbers`, the starfield zoom): `data-x` / `data-y` on each `<li>` set which corner the number settles in (-1 left, 1 right).
- Campaign results (`#results`, the shrinking card): the list items, plus the stills in `assets/wall/` that slide past it.
- Numbers as of November 2026: platform data from Metabase, streams and TikTok creates from Chartex. Update them when they move.
- Colours: the `:root` block at the top of the `<style>` tag. `--accent` is for filled things (buttons), `--accent-hi` for blue text and outlines on the dark background. Keep both bright, the old navy blue was nearly invisible on black.

- Fonts: self-hosted in `assets/fonts/` (Anton, Inter, JetBrains Mono, latin only, SIL Open Font License), so the page never waits on Google Fonts.
- Hero rows on phones: `hero-row-a-sm.mp4` / `hero-row-b-sm.mp4`, a lighter copy of the same rows. Rebuild both if you change a row.

Preview locally by opening `index.html` in a browser, or run `python -m http.server` in this folder.

## Making new edit clips

Cut a 6 second loop from the middle or late part of the edit (the start is usually just the intro), crop it square and keep it small:

```
ffmpeg -ss 8 -t 6 -i input.mp4 -an -vf "scale=432:432:force_original_aspect_ratio=increase,crop=432:432,fps=30,format=yuv420p" -c:v libx264 -preset slow -crf 30 -movflags +faststart assets/edits/name.mp4
ffmpeg -i assets/edits/name.mp4 -frames:v 1 -q:v 5 assets/edits/name.jpg
```

If the edit has black bars, add `crop=W:H:X:Y` before `scale` (find the values with `ffmpeg -i input.mp4 -vf cropdetect -f null -`).

Videos only load and play while they are on screen. With reduced motion or Save-Data turned on, visitors see the posters only.

## Open items

- The demo form posts to the placeholder `mailto:demos@overnite.example`. It needs the real address or a form service (needed for audio file uploads).
- The "Why Overnite" photo is a stand-in.

## Deploy

Hosted on Vercel (project `overnite-records`). Every push to `main` goes live automatically within about a minute. Other branches get their own preview URL.
