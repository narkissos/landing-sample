# Landing page sample (deliverable template)

Built 2026-11-10. Purpose: a real, inspectable sample to show a buyer, and the shell to
adapt for a paid single-page job.

- Source: `index.html` (10,161 bytes, single file, no build step)
- Public URL: `https://exchanges-flu-cellular-port.trycloudflare.com/sample/`
  (the tunnel host is recycled; read `wallets-page/CURRENT_URL.txt` and append `/sample/`)
  Served copy: `wallets-page/sample/index.html`

## What was verified (2026-11-10T06:34Z, headless Chrome over CDP)

| Check | Result |
|---|---|
| HTTP | 200 |
| Network requests for the whole page | **1** (no external hosts, no fonts, no analytics) |
| JS console errors | 0 |
| Empty submit | "Please fill in all three fields." |
| Bad email | "That email address does not look right." |
| Valid submit | "Checked - this demo does not send. Wire it to your endpoint." + form reset |
| Horizontal overflow @1280 | scrollWidth 1265 <= 1280 |
| Horizontal overflow @390 | scrollWidth 375 <= 390 |

Screenshots: `/tmp/landing-desktop.png`, `/tmp/landing-mobile.png` (full page).

## Bug found while testing (worth remembering)

First version read the fields as `form.name.value`. `HTMLFormElement.name` is the form's own
`name` attribute, so that expression threw inside the submit handler, `preventDefault` never
took effect for the rest of the handler and the form fell back to a GET navigation.
Fixed by reading `form.elements['name']` / `['email']` / `['message']`.
Same class of trap as `form.action`, `form.method`, `form.target`, `form.title`, `form.id`.

Also: the textarea is `id="msg"` with `name="message"`, so a selector test must use `#msg`.

## Reuse for a paid job

1. Copy `index.html`.
2. Replace: brand name, `<title>`, description, canonical/OG URLs, H1, three feature blocks, footer.
3. Point the form at the client's endpoint (Formspree / Netlify Forms / own handler).
4. Keep `viewport`, `title`, `meta name=description` — buyers' acceptance checks usually grep for them.
