# Overnite website

One-page site for Overnite: https://overnite-records.vercel.app

Plain static HTML, no build step. Everything (text, styles, script) is in `index.html`.

## Edit

- Text: search for the sentence in `index.html` and change it.
- Colours: the `:root` block at the top of the `<style>` tag.
- Photos: `assets/img/` (current photos are Wikimedia stand-ins, credited in `credits.html`).

Preview locally by opening `index.html` in a browser, or run `python -m http.server` in this folder.

## Open items

- The demo form posts to the placeholder `mailto:demos@overnite.example`. It needs the real address or a form service (needed for audio file uploads).
- All photos are stand-ins.

## Deploy

Hosted on Vercel (project `overnite-records`). From this folder:

```
npx vercel@latest deploy --prod
```
