# Landing page sample (deliverable template)

Built 2026-11-10. Purpose: a real, inspectable sample to show a buyer, and the shell to
adapt for a paid single-page job.

## Where it lives

| Role | Location | Notes |
|---|---|---|
| Durable public URL | https://narkissos.github.io/landing-sample/ | GitHub Pages, repo narkissos/landing-sample, text/html, md5 4e703f2726e5f2f9a8557eebf4c02c12 |
| Repo | https://github.com/narkissos/landing-sample | public, branch main, Pages from root |
| Source of truth | index.html here (10,161 B) | single file, no build step |
| Ephemeral mirror | (wallets-page/CURRENT_URL.txt)/sample/ | tunnel host rotates; do not put it in a bid |

files.catbox.moe/2n40wq.html was tried and **is not a page host**: it serves the file as plain
text (the browser rendered the source, title empty, body length == file length). Fine as a
downloadable file, useless as a demo link.

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

Screenshots: /tmp/landing-desktop.png, /tmp/landing-mobile.png (full page).
Pages copy re-fetched after deploy: HTTP 200, 10,161 B, md5 identical to the local file,
content-type text/html; charset=utf-8.

## Bug found while testing (worth remembering)

First version read the fields as form.name.value. HTMLFormElement.name is the form's own name
attribute, so that expression threw inside the submit handler, preventDefault did not protect
the rest of the handler, and the form fell back to a GET navigation.
Fixed by reading form.elements['name'] / ['email'] / ['message'].
Same trap: form.action, form.method, form.target, form.title, form.id.

Also: the textarea is id="msg" with name="message", so a selector test must use #msg.

## Reuse for a paid job

1. Copy index.html.
2. Replace: brand name, title, description, canonical/OG URLs, H1, three feature blocks, footer.
3. Point the form at the client's endpoint (Formspree / Netlify Forms / own handler).
4. Keep viewport, title, meta name=description - buyers' acceptance checks usually grep for them.
