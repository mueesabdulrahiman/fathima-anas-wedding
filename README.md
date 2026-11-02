# Blossomoud — Wedding Invitation (reorganized)

Original source: raw wget-style mirror of a Tilda-built page
(`webgencyinvitations.com/blossomoud.html`), scattered across ~10 CDN-domain
folders (`static.tildacdn.net`, `thb.tildacdn.net`, `optim.tildacdn.net`,
`neo.tildacdn.com`, `fonts.gstatic.com`, an R2 bucket, etc.) with hash names
like `tild6364-3438-4161-b964-613234643831`.

## Structure

```
index.html                     (was blossomoud.html)
assets/
  css/
    vendor/                    Tilda engine CSS (grid, animation, forms, error box, Ovo @font-face)
    project-blocks.min.css     This project's generated block styles
  js/
    vendor/                    Tilda engine JS (scripts, lazyload, animation, zero-*, forms, events, fallback, forms-dict, stat)
    project-blocks.min.js      This project's generated block behavior
    email-capture.js           Custom feature (see Bug fixed below)
  fonts/                       5 real fonts used on this page + the Google-Fonts Ovo file
  img/
    favicon.svg, rectangle-242.svg   Real full-resolution assets that were actually captured
    placeholders/              16 tiny (20px) blur-up thumbnails — see note below
  video/
    fade-video-intro.mp4       Canvas fade-in hero video
    background-video.mp4       Looping decorative background video
```

## Bug fixed

`custom.css` did **not** contain CSS — it contained the JS for the "save
visitor's email to localStorage on form submit" feature, but was wired up
with `<link rel="stylesheet">`. A browser never executes CSS as JS, so this
feature was silently dead on the live site. Moved the code to
`assets/js/email-capture.js` and load it with `<script>` before `</body>` —
same logic, now actually runs.

## Important: this scrape never captured the full-resolution photos

Tilda lazy-loads images: only a 20px blurred placeholder (`thb.tildacdn.net`)
is fetched eagerly; the real image (`static.tildacdn.net/tildXXXX/name.png`)
is fetched by JS only once the element scrolls into view. The mirroring tool
that produced this zip apparently never scrolled the page, so **not a single
full-resolution content photo/illustration exists in the source files** —
only the 16 blurry placeholders in `assets/img/placeholders/`.

I did **not** invent or fake these — I left every `data-original="..."` in
`index.html` pointing at the real, still-live Tilda CDN URL, exactly as the
original page does. Result: the page will look identical to production as
long as it's viewed with internet access. If you need a fully offline copy,
those ~20 images need to be fetched directly from `static.tildacdn.net`
(a domain I don't have network access to from here).

Also left pointing at their original remote URLs, for the same reason
(never present in the scrape):
- `favicon-light_1.png` / `favicon-dark_1.png` / `apple-touch-icon_1.png` (the SVG favicon, which *was* captured, covers all modern browsers)
- `og:image` (social-share preview image)
- `tilda-polyfill-1.0.min.js` (legacy-browser-only fallback, loaded with `nomodule`)
- the background-music MP3 (`pub-...r2.dev/...mp3`) — the file captured in the zip was a 178-byte "No Content" error page, not audio. The floating play/pause button and `<audio id="invitation-audio">` element are otherwise fully intact; worth double-checking that R2 link is still public.
- 6 extra `@font-face` declarations in `project-blocks.min.css` (ArticaPro, Canela, BeauRivage, Rufina x2, TheSilverEditorial) — declared for the wider Tilda project but not used on this page, so never downloaded either. Harmless to leave remote.

## Features preserved (verified against the original HTML)

- Tap-to-open envelope/cover intro screen + "Appuyez pour ouvrir" trigger
- Canvas fade-in hero video + looping background video
- Background music with floating play/pause toggle
- Countdown / date, RSVP form (Tilda forms engine), Google Maps embed
- All Tilda scroll/step animations (`animation`, `animation-sbs`, `zero-*`)
- Responsive breakpoints (320/480/640/960)
- Email-capture on form submit (now actually working — see Bug fixed)

Nothing was removed; every `<script>` block, block ID, and animation config
in `index.html` is untouched — only asset **paths** were rewritten.
