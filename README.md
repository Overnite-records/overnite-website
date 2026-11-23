# Overnite website

One-page site for Overnite: https://overnite-records.vercel.app

Plain static HTML, no build step. Everything (text, styles, script) is in `index.html`.

## Edit

- Text: search for the sentence in `index.html` and change it.
- `assets/edits/`: no longer used. These clips belonged to the number drawer, which was removed (numbers are plain text now). Safe to delete.
- Hero text: "Viral Overnite." with the motto "Blew up overnite." under it (from the Label Motto page in Notion).
- Browser tab and Google icon: the moon logo on a dark rounded square. `favicon.svg` is the master; `favicon.ico` (16/32/48), `favicon-96.png`, `icon-192.png` and `apple-touch-icon.png` (180) are made from it. If the logo changes, redraw `favicon.svg` and re-export those. Google only shows a new icon after it crawls the site again, which can take days to a few weeks.
- Logo and mascot: a full cream moon with a black orb in front of its right side, which makes the crescent. The orb has blue cat eyes. Everything is drawn in code, with no image files. It only appears in three places: the top bar logo (static, only the pupils move), the hero (top right) and the blue band at the end. The shared colours live in the hidden `<svg>` with the `mn-` ids right after `<body>`. Script part 8 draws the orb's face and moves it.
  - The face is flat, but every point of the eyes and grin is wrapped around a ball and turned toward the mouse. When it looks to one side, the far eye and that end of the grin squeeze toward the edge and slide out of sight.
  - The grin is modelled on the reference video: it starts as a thin slash drawn in from the right, then opens into a wide grin up to eye level with two rows of rounded interlocking teeth, and the eyes lift. When the mouth closes, it disappears at once.
  - On desktop the orb in the hero hunts (the one on the blue band only follows you with its eyes). When the mouse comes within about one orb width, it leaves the moon (the full moon stays behind), grins and follows the cursor on a soft spring, slower than the mouse. If the mouse gets far away, it floats home. If it catches the cursor, the jaw drops, it snaps shut and the cursor is gone for 3 seconds. Then it chews, floats home looking pleased, and rests for a moment.
  - The orb is always in front of the moon and moves freely, including straight over it (no invisible blockers). It is a little smaller than the moon, so right over the middle it swells just enough (up to about 20%) to cover it, so you never see a white ring around it. When you come close, it pounces straight at the cursor (about half a second), then follows at the slower speed.
  - It blinks now and then. On touch screens, and with reduced motion, there is no chase, only the eyes and the head turn.
- Hero background: two rows of edits in `assets/reel/`, one small video per edit (`eNN.mp4`, 400px tall, in the edit's real shape, 6 second loop) with a `eNN.webp` poster. Each `<video>` in `.reel__track` needs `width`/`height` matching the file so the row can be laid out before anything loads. `data-speed` on a row is its drift speed in row heights per second (negative = the other way). The script wraps each video in a `.reel__tile` div and slides the div with a browser animation that loops it round the row, so tiles are never duplicated and the drift costs no script per frame (moving the `<video>` itself made Chrome restyle it every frame, which was the main lag). Four times a second it works out from the animation clocks which tiles are on screen and plays only those. The IDs match the catalog of original edits (project doc `claude/overnite-source-edits-catalog.md`, originals in Kon's Drive).
- Numbers: plain text, nothing to click. The big "142B views" and the four corner stats are in `#numbers`, the four Campaign results lines in `#results`. Edit the text directly.
- Our creator base (`#numbers`, the starfield zoom): `data-x` / `data-y` on each `<li>` set which corner the number settles in (-1 left, 1 right). Each corner stat is centred on itself (number, label and pill on one middle line). The last stat settles at about 3/4 of the scroll run, so there is little dead scrolling after it.
- Campaign results (`#results`, the shrinking card): the list items. The section is 130vh tall (125vh on phones), so it takes about a quarter of a screen of scrolling to get through. The script makes each column longer by repeating 5 clips from two columns over, every third tile is tall (9:16), and the tiles grow with the screen, so the wall has no empty patches while it slides. The clips sliding behind it are `assets/morph/m01.mp4` to `m19.mp4` (320px, 6 second loops) with `assets/wall/w01.webp` to `w19.webp` as their posters. Posters load a screen or so early, clips load and play only while they are actually in view (a safety cap of 24 at once, 12 on phones).
- Why Overnite: four cards side by side (`.why__card`), each with a small line icon, a heading and the text, so every reason is readable at once. Two by two on tablets, stacked on phones. To change a reason, edit the card's heading and text.
- What we sign: each genre cell turns blue on hover (or keyboard focus), the name moves up and the description appears. On touch screens the description is always shown.
- How it works: three steps drawn as a timeline, circles and text centred, circles joined by arrows. The circles stay outlines (no hover fill, they are not buttons). On phones it runs top to bottom, left aligned, with down arrows. The space under the section matches the space above it (`#how` padding-bottom). Pure CSS on `.step`.
- Page end order: Questions, then the blue "We are ready / Are you?" band (Montserrat ExtraBold), whose "Let's talk" arrow jumps down to the demo form, then the form, then the footer. The form is the only contact route (no separate email link).
- Cursor: on desktop the mouse is a small dot that inverts what is under it and stretches with movement (script part 4). It is two stacked dots with blend modes (`saturation` takes the colour out, `difference` inverts), and over blue surfaces (the closing band, buttons, pills, a hovered genre) it is plain black. The list of blue surfaces is `BLUE` in part 4.
- Hover click: moving the mouse onto a Campaign results number plays a short click, generated in the browser (script part 6, no audio file). Browsers only allow it after the first click or key press on the page.
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

## Keeping it smooth

- All moving parts share one frame loop (`ticker` at the top of the script). It only runs while something moves and stops by itself.
- Nothing inside the loop reads layout: section positions are measured on load, resize and font load, and the scroll position comes from the scroll listener.
- The starfield canvas is drawn at most at 1.5x pixel density and drifts at about 20 fps when you are not scrolling. It stops when the section is off screen or the tab is hidden, like every other effect.
- Avoid `backdrop-filter` and animated `text-shadow` over video, and don't move `<video>` elements directly every frame: move a wrapper.

## Open items

- Demo form: picking "Something else" as genre opens a "Which genre?" field. Track links and TikTok links are lists of boxes: once a box has something in it, a new empty one appears below (up to 10). Every file pick adds to the list (no replacing), each file can be removed with its ×, and the button then reads "Add another file". All of it is script part 5. `data-endpoint` on the form is empty, so for now submitting opens an email to `data-to` (still the placeholder `demos@overnite.example`) and people attach their files there. Set `data-endpoint` to a form service that accepts audio files and it posts everything there instead. The form has no `mailto:` action on purpose: Chrome marks that as not secure and turns autofill off.

## Deploy

Hosted on Vercel (project `overnite-records`). Every push to `main` goes live automatically within about a minute. Other branches get their own preview URL.
