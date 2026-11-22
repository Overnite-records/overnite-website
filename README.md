# Overnite website

One-page site for Overnite: https://overnite-records.vercel.app

Plain static HTML, no build step. Everything (text, styles, script) is in `index.html`.

## Edit

- Text: search for the sentence in `index.html` and change it.
- `assets/edits/`: the clips shown in the number drawer (`data-media`). The edits grid that used to sit above Campaign results was removed.
- Hero background: two rows of edits in `assets/reel/`, one small video per edit (`eNN.mp4`, 400px tall, in the edit's real shape, 6 second loop) with a `eNN.webp` poster. Each `<video>` in `.reel__track` needs `width`/`height` matching the file so the row can be laid out before anything loads. `data-speed` on a row is its drift speed in row heights per second (negative = the other way). The script moves every tile and wraps it round, so tiles are never duplicated and only the ones on screen load and play. The IDs match the catalog of original edits (project doc `claude/overnite-source-edits-catalog.md`, originals in Kon's Drive).
- Numbers: every number is a `<button class="num">` with `data-kind`, `data-num`, `data-title`, `data-text` and optional `data-media` (edit clip names from `assets/edits`, comma separated). Clicking it opens the drawer with that text, so to change what a number says, edit those attributes.
- Our creator base (`#numbers`, the starfield zoom): `data-x` / `data-y` on each `<li>` set which corner the number settles in (-1 left, 1 right). Each corner stat is centred on itself (number, label and pill on one middle line). The last stat settles at about 3/4 of the scroll run, so there is little dead scrolling after it.
- Campaign results (`#results`, the shrinking card): the list items. The clips sliding behind it are `assets/morph/m01.mp4` to `m19.mp4` (320px, 6 second loops) with `assets/wall/w01.webp` to `w19.webp` as their posters. Only the clips on screen play, at most 8 at once (4 on phones).
- Why Overnite: four tabs (`.why__tab`, each with a small line icon) and one panel per tab (`.why__panel`). Clicking a tab or using the arrow keys shows its panel; the chosen tab turns blue. To change a reason, edit the tab label and the matching panel text.
- What we sign: each genre cell turns blue on hover (or keyboard focus), the name moves up and the description appears. On touch screens the description is always shown.
- How it works: three steps drawn as a timeline, circles and text centred, circles joined by arrows. Hovering a step fills its circle blue (on touch screens the last circle is filled instead). On phones it runs top to bottom, left aligned, with down arrows. Pure CSS on `.step`.
- Page end order: Questions, then the blue "We are ready / Are you?" band (Montserrat ExtraBold), whose "Let's talk" arrow jumps down to the demo form, then the form, then the footer. The form is the only contact route (no separate email link).
- Hover click: moving the mouse onto a Campaign results number plays a short click, generated in the browser (script part 6, no audio file). Browsers only allow it after the first click or key press on the page.
- "Press to show more stats": the small blue pill on every number. It shows on hover (centred under the line in Campaign results), and always on touch screens, so people know a number opens more.
- Numbers as of November 2026: platform data from Metabase, streams and TikTok creates from Chartex. Update them when they move.
- Colours: the `:root` block at the top of the `<style>` tag. `--accent` is for filled things (buttons), `--accent-hi` for blue text and outlines on the dark background. Keep both bright, the old navy blue was nearly invisible on black.

- Fonts: self-hosted in `assets/fonts/` (Anton for headlines, Archivo for everything else including the small labels, Montserrat ExtraBold only for the blue closing band; latin only, SIL Open Font License), so the page never waits on Google Fonts.

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

- Demo form: picking "Something else" as genre opens a "Which genre?" field. Track links and TikTok links are lists of boxes: once a box has something in it, a new empty one appears below (up to 10). Every file pick adds to the list (no replacing), each file can be removed with its ×, and the button then reads "Add another file". All of it is script part 5. `data-endpoint` on the form is empty, so for now submitting opens an email to `data-to` (still the placeholder `demos@overnite.example`) and people attach their files there. Set `data-endpoint` to a form service that accepts audio files and it posts everything there instead. The form has no `mailto:` action on purpose: Chrome marks that as not secure and turns autofill off.
- The logo in "Why Overnite" is the placeholder mark until the real logo is done.

## Deploy

Hosted on Vercel (project `overnite-records`). Every push to `main` goes live automatically within about a minute. Other branches get their own preview URL.
