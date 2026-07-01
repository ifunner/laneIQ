# LaneIQ — Cursor build prompt (merge redesign onto the working app)

**Setup before you paste:**
1. Keep the working app as `index.html` (this is the one you'll edit — it has all the logic).
2. Save the Claude Design output as `redesign.html` in the same folder (visual reference only).
3. In Cursor, open the folder, open Composer (⌘I), add both files to context: `@index.html` `@redesign.html`.
4. Paste everything below.

---

## Goal
Make `index.html` **look like `redesign.html`** while **behaving exactly like it already does**. This is a visual transplant, not a rewrite. `index.html` is the source of truth for **all behaviour**; `redesign.html` is the source of truth for **all visuals** (palette, typography, spacing, component styling, layout, motion). Edit `index.html` in place. Do **not** rebuild the app from `redesign.html`.

## Why this direction
`index.html` is a single-file vanilla-JS PWA with a real engine: localStorage (with in-memory fallback), a drill runner, an 18-hole course engine with golf scoring + medals, an XP/level/achievement system, stats, and history — all rendered by template-string functions in the `<script>`. `redesign.html` from a design tool may contain restructured markup, renamed classes, or mock/stripped logic. Reconstructing the engine from it is risky. Instead, port the *look* into the working file.

## Step 1 — Audit (do this first, output a short table)
Compare the two files and produce a mapping for each screen/component:
- the design's markup + class names, vs
- the working app's rendered markup + the classes/ids/data-attrs its `<script>` depends on.
Flag every place the design renamed or restructured something the script relies on.

## Step 2 — Transplant the visual system
1. Replace the `:root` CSS custom-property block in `index.html` with the design's token set, **mapped 1:1 to the existing variable names** (`--bg --surface --surface-2 --surface-3 --line --line-soft --ink --muted --muted-2 --green --green-deep --green-glow --clay --amber --blue --display --mono --body --safe-b --safe-t`). If the design introduces new tokens, add them; don't rename the existing ones.
2. Port the design's component CSS (cards, buttons, nav, panels, overlays, level card, achievement tiles, scorecard, the lane) into the existing `<style>`, keeping the existing class names.
3. If a component's **markup** must change to match the design, update the matching **template string inside the render function** in the `<script>` — not just the CSS — and keep/rewire the `id`s, `data-*` hooks, and event-bound classes.
4. Update the **hardcoded hex colours inside the SVG functions** `laneSVG()`, `matGuideSVG()`, `matThumbSVG()`, and `sparkline()` to the new palette (or refactor them to read CSS variables via `getComputedStyle`). Otherwise the lane and charts keep the old colours.

## Step 3 — Preserve exactly (do not break)
- Every `id` and every `class` referenced in the `<script>`. If you rename one, update **all** occurrences in the render templates too.
- The entire engine: storage layer + in-memory fallback, drill runner, course engine, XP/achievement logic, router, toast, haptics.
- Data hooks: `data-drill`, `data-course`, `data-guide`, `data-tab`, `data-replay`, `data-k`, `data-d`, `data-n`.
- Handler ids: `#app #screen #nav #overlay-root #toast #go #finish #undo #close #next #ok #again #done #balls #seg #callout #wipe #play #ok`.
- `manifest.json`, `sw.js`, PWA meta, the localStorage key, and the graceful fallback (never make the app hard-depend on localStorage).

## Step 4 — Guardrails
- Stay **single-file vanilla JS**. No React/Vue/framework, no bundler, no build step, no npm.
- No `localStorage`/`sessionStorage` refactor that removes the in-memory fallback.
- Fonts via Google Fonts `<link>` with a system fallback (must still work offline).
- Keep mobile-first: large tap targets, `env(safe-area-inset-*)`, dark UI.
- Keep accessibility: visible keyboard focus, `prefers-reduced-motion` respected, WCAG-AA text contrast.
- Do **not** invent features. Match current behaviour 1:1. If the design shows a component with no backing data, wire it to existing state or leave it inert and flag it — don't fabricate logic.

## Step 5 — Verify before you finish (click through, fix any error)
Run/open the file and confirm, with **no console errors**:
- All four tabs render: Practice, Rewards, Stats, History.
- Each single drill (Make/Good Zone/Start Line) opens setup, shows the 📍 "from → to" callout, runs, undoes, and **saves** (session appears in History; Stats update).
- **The 21** chains its three legs and saves one combined score.
- A full **18-hole course** plays through: per-hole result screens, running to-par, final scorecard with medal, front/back totals, and the **+XP / level-up / new-badge** row.
- **XP + level** advance; the header level chip updates; **achievements** unlock and show in Rewards with progress bars.
- **Mat guide** overlay renders the labelled mat + 5 instruction rows.
- **History** groups by day and course rows re-open the course on tap.
- **Reset all data** clears everything.
- Renders correctly at ~380px width; safe-area padding intact; drill buttons are thumb-reachable.

## Deliverable
A single updated `index.html`: visually matching `redesign.html`, functionally identical to the original. Then give me:
- the list of any class/markup renames and confirmation the `<script>` templates were updated to match,
- any design elements you couldn't wire without changing logic (and what they'd need).
