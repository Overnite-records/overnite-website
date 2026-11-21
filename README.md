# Overnite website

One-page site for Overnite: https://overnite-records.vercel.app

Plain static HTML, no build step. Everything (text, styles, script) is in `index.html`.

## Edit

- Text: search for the sentence in `index.html` and change it.
- `assets/edits/`: the clips shown in the number drawer (`data-media`). The edits grid that used to sit above Campaign results was removed.
- Hero background: `assets/hero/hero-row-a.mp4` and `hero-row-b.mp4`. Each row is 10 edits stitched side by side into one video (2560x240), so only a few videos play at once.
- Numbers: every number is a `<button class="num">` with `data-kind`, `data-num`, `data-title`, `data-text`, `data-note` and optional `data-media` (edit clip names from `assets/edits`, comma separated). Clicking it opens the drawer with that text, so to change what a number says, edit those attributes.
- Our creator base (`#numbers`, the starfield zoom): `data-x` / `data-y` on each `<li>` set which corner the number settles in (-1 left, 1 right). Each corner stat is centred on itself (number, label and pill on one middle line). The last stat settles at about 3/4 of the scroll run, so there is little dead scrolling after it.
- Campaign results (`#results`, the shrinking card): the list items. The clips sliding behind it are `assets/morph/m01.mp4` to `m19.mp4` (320px, 6 second loops) with `assets/wall/w01.webp` to `w19.webp` as their posters. Only the clips on screen play, at most 8 at once (4 on phones).
- "Press to show more stats": the small blue pill on every number. It shows on hover (centred under the line in Campaign results), and always on touch screens, so people know a number opens more.
- Numbers as of November 2026: platform data from Metabase, streams and TikTok creates from Chartex. Update them when they move.
- Colours: the `:root` block at the top of the `<style>` tag. `--accent` is for filled things (buttons), `--accent-hi` for blue text and outlines on the dark background. Keep both bright, the old navy blue was nearly invisible on black.

- Fonts: self-hosted in `assets/fonts/` (Anton for headlines, Archivo for text, JetBrains Mono for small labels, latin only, SIL Open Font License), so the page never waits on Google Fonts.
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

- Demo form: people paste links (one per line) and/or add several MP3 or WAV files. `data-endpoint` on the form is empty, so for now submitting opens an email to `data-to` (still the placeholder `demos@overnite.example`) and people attach their files there. Set `data-endpoint` to a form service that accepts audio files and it posts everything there instead. The form has no `mailto:` action on purpose: Chrome marks that as not secure and turns autofill off.
- The logo in "Why Overnite" is the placeholder mark until the real logo is done.

## Deploy

Hosted on Vercel (project `overnite-records`). Every push to `main` goes live automatically within about a minute. Other branches get their own preview URL.
