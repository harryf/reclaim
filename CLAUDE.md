# CLAUDE.md — Reclaim

Guidance for anyone (human or AI) editing this repo. Read it before changing `index.html`.

**Live:** https://harryf.github.io/reclaim/ · **Repo:** public, GitHub Pages from `main` / root.

---

## What this is

A single-page **manifesto + strategy** arguing that we have entered an age of *personal, free-to-run, data-sovereign software*, and laying out, honestly, how it could actually happen. It is half polemic, half engineering plan, half honest accounting of the hard parts (yes, that is three halves; the piece is meant to over-deliver).

The page is its own proof: **one `index.html`, no build step, no backend, no tracking, hosted free.** If the argument is "software can be personal, free, and yours," the artifact has to be exactly that.

---

## The whole thing is one file

- **`index.html`** — everything. Structure, ~1000 lines of hand-written CSS in a single `<style>`, inline SVG diagrams, and a ~30-line vanilla-JS block at the very bottom (only for the glossary tooltips). No frameworks, no build, no dependencies to install.
- **`og-image.jpg`** — the social-share preview (1200×630). The only binary asset. Regenerate it if the masthead/tagline changes (see below).
- **`README.md`** — short public description.
- **Everything else is remote or inline.** Photos are hot-linked from **Wikimedia Commons**; diagrams are **inline SVG**. Nothing else needs hosting.

Do **not** add a bundler, a framework, a package.json, or a `node_modules`. If you are tempted to, re-read the manifesto.

---

## How to update and deploy

```bash
# edit index.html, then:
git add index.html
git commit -m "..."
git push            # GitHub Pages redeploys automatically (~30–60s)
```

Verify a change actually rendered before claiming it works. Because headless Chrome ignores `width=device-width` and lays out mobile pages at ~980px (cropping to the window), **do phone-width checks inside a fixed-width iframe**, e.g.:

```html
<!-- wrapper.html: gives the page a TRUE 390px viewport -->
<iframe src="index.html" style="width:390px;height:4000px;border:0"></iframe>
```
Screenshot the wrapper. This is the only reliable way to check the mobile layout and the tooltips.

---

## The OG image (social previews)

`og-image.jpg` is generated, not hand-drawn. The generator is an HTML card screenshotted to PNG then converted to JPEG (kept under ~300KB so WhatsApp renders it). It bakes in Delacroix's *Liberty Leading the People* behind the wordmark. To regenerate: build the card HTML, `chrome --headless=new --window-size=1200,630 --screenshot`, then `sips -s format jpeg`. The `<meta property="og:image">` uses an **absolute** URL (`https://harryf.github.io/reclaim/og-image.jpg`) — previews break with relative URLs. Social scrapers cache aggressively; warm a stale preview with a card validator after deploy.

---

## Adding images (rules)

Every image is either **inline SVG** (for diagrams) or a **Wikimedia Commons** photo/painting. When adding a Commons image:

1. **Find the real file via the API**, never guess a URL:
   `https://commons.wikimedia.org/w/api.php?action=query&generator=search&gsrnamespace=6&prop=imageinfo&iiprop=url|mime|extmetadata&iiurlwidth=1280&gsrsearch=...`
2. **Verify it returns `200 image/*`** with `curl -sI` before embedding. A hard-coded URL that 404s is the worst failure mode here.
3. **Attribute it.** Public-domain art needs a courtesy credit; CC-BY / CC-BY-SA need author + licence in the `.cred` line inside the `<figcaption>`.
4. **Duotone it** to match the zine: `.photo.duo` (blood-red, for the charged beats) or `.photo.gray` (grayscale, for the quieter ones). This keeps a wall of mixed photos and paintings feeling like one designed object.
5. Every image should **make an argument**, not decorate. Pair it with a one-or-two-sentence caption that lands a point.

---

## Writing rules (non-negotiable)

- **No em dashes.** Ever. A comma, colon, or parenthesis is almost always right. This is the single most-checked rule; grep `—` and expect zero.
- **Plain language for non-technical readers.** A layperson must never hit a wall. Say "a computer the size of a playing card," not "a Raspberry Pi SBC." Genuinely useful technical terms (PWA, WebRTC, GitHub Pages, localStorage, IndexedDB, LLM, BitTorrent) are kept but wrapped in a **tap/hover glossary** (see below) so the sentence still reads without opening them.
- **Bold sparingly.** Only for a genuinely load-bearing phrase.
- **The voice is a pamphlet:** short punches mixed with longer lines, quotable theses, a clear enemy (the middleman), a clear promise. Rousing, never corporate.

---

## The glossary tooltip system (the only JS)

Technical terms use `<span class="term">PWA<span class="pop">…definition…</span></span>`. The script at the bottom of `index.html`:

- **Desktop:** hover shows the note anchored above the term (CSS `@media(hover:hover)`).
- **Mobile (≤640px):** tapping a term drops the note **into the page body** as an absolutely-positioned, full-width panel just under the term, so it **scrolls with the page** and can never run off-screen sideways.
- **Closing:** an injected `×` button, a **second tap** on the term, a **tap anywhere else**, or **Escape**. Reading the note itself never closes it.

If you add a new `.term`, you get all of this for free; just include the `.pop` child. Keep the JS dependency-free and syntax-check it (`node --check`) before pushing.

---

## Key philosophical ideas (the argument, in order)

1. **Current → ideal state.** The old age of software is rented, centralized, surveilled. The ideal is software that is personal, free to run, and lives on your own devices.
2. **Convenience is the only thing that ever wins.** Decentralization did not fail on technology; it failed on convenience. That is a *design* failure, now fixable.
3. **Personalization is the missing variable.** An app built for exactly one person is *more* convenient than any mass platform, and has no reason to phone home. This is the hinge the whole argument turns on.
4. **First-principles reduction.** An app needs five things (somewhere to run, to serve, to store, to sync, to build). You already own all five for free; the browser, GitHub Pages, your device, WebRTC, and an LLM for pennies. Accounts, servers, and subscriptions were a *business model*, not a requirement.
5. **Incentive (the keystone).** Freedom is not enough of a reason. You profit by (a) keeping the platform's 30% cut, (b) selling *skill and service* around free software (the Linux/WordPress precedent), (c) building once and selling to the many who can't, (d) being paid at the edges with existing rails or decentralized money used *as plumbing, not a casino*, and (e) riding the trillion-dollar migration of value from the center to the edges. **The real dividend is time:** software you own stops billing you and mining your attention.
6. **GitHub is leverage, not a cage.** We are not locked into GitHub; GitHub is locked into us. Git is distributed, Pages is free, and it is too load-bearing for the world to switch off. Use it, owe it nothing.
7. **Honesty about the walls.** Discovery, identity, payments, availability, connectivity, the human pull to centralize, trust/spam, and the last mile of usability are real and unsolved. Name them, give each a direction of attack, do not pretend.
8. **It is a seed, not a startup.** It spreads because everyone already builds with LLMs. Hand them the pattern (the PAI approach: a framework + prompts) and each personal app is a spore for the next.

---

## The narrative approach (how it persuades)

The piece is deliberately built as an **emotional rollercoaster**, because a pure logic argument does not move people to act. The arc, in images and tone:

- **Hook** (Andreessen's "software is eating the world", the night-lit Earth, "you were the meal") — set the scene, name the stakes, promise two minutes.
- **Anger / the low** (wall of CCTV cameras in blood-red) — what the last twenty years actually built.
- **Defiance** (Delacroix's revolution; the ten theses) — take it back.
- **The click** (Gutenberg Bible; the five-parts reduction) — the "obvious in hindsight" insight.
- **Proof** (the tailor; three real field-study exhibits; a child's hands) — this is not a thought experiment.
- **Scope and honesty** (the reclaim map; the Great Wall over the hard parts) — the whole landscape, and the walls.
- **The turn to self-interest** (Caillebotte's market; where's the money) — you profit, and you get your time back.
- **Emergence and action** (Margaret Hamilton; the murmuration; the first steps) — developers light the first match; a leaderless flock becomes unstoppable.
- **Freedom** (Friedrich's *Wanderer above the Sea of Fog*) — the oldest picture of freedom, then the rallying cry: *Fork it. Ship it. Keep your data.*

Techniques borrowed from the most effective persuasion: a single clear enemy, repeated slogans ("no server, no account, no middleman"; "freedom is the reason, money is the plan, your time is the prize"), direct address ("you"), concrete images over abstractions, identity ("you are the kind of person who…"), the objection answered exactly when doubt peaks (the money section right after "it's free"), and a subversive visual now and then (a red flock of sheep: the herd never notices the fence).

## Section map (for quick orientation)

`masthead → #open (epigraph) → 00 the claim → 01 manifesto → 02 first principles → 03 strategy (+ Why-GitHub callout) → 04 field evidence → 05 the reclaim map (+ small-business) → 06 the hard parts → 07 the cost → 08 where's the money → 09 a seed → 10 start the avalanche → #freedom → sign-off`.

Section numbers are hand-written in `.secnum`. If you insert a numbered section, renumber the ones after it.

---

## Gotchas

- **Headless Chrome mis-renders mobile.** Always verify phone layout via a fixed-width iframe (above).
- **Remote images can change.** They are hot-linked; if Commons ever moves a file, re-run the API lookup and re-verify `200`.
- **`og:image` must be absolute** and the file must be deployed (scrapers cannot read `file://` or a localhost URL).
- **The `.txt` idea-notes are git-ignored on purpose.** Do not commit personal source material.
- **Never leak real user data.** The field studies describe real people; keep it to what they have agreed to make public, and never embed screenshots of their actual app content.
