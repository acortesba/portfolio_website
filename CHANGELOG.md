# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.16.9] - 2026-05-03

### Fixed
- **Progress Bar Hardcaps Removed:** The skills progress bar now dynamically scales to support any number of milestones.
  - **Firestore Query:** Removed the hardcoded `.limit(5)` from the query; raised safety limit to 20.
  - **Cache Invalidation:** The `localStorage` cache is now busted automatically if the Firestore document count changes, preventing stale renders.
  - **Dynamic Rebuild:** `refreshProgressBarMilestones()` now detects if the milestone count changes mid-session and triggers a full DOM rebuild (`rebuildProgressBar()`) while seamlessly preserving the user's scroll state and completion animation.
  - **Dynamic DOM Generation:** `createProgressBar()` now dynamically generates `.progress-point` and `.mobile-milestone-item` elements based on `milestones.length` instead of hardcoding exactly 5.
  - **Dynamic CSS & Activation:** Removed hardcoded CSS `left` values and `index/6` thresholds. Dots and tooltips are now dynamically spaced evenly across the bar using JS `(index + 1) / (milestones.length + 1) * 100%`.
- **Progress Bar Animation Fixes:**
  - **Stale DOM Reference:** Fixed a bug where `updateProgressBar()` continued to operate on the detached initial DOM nodes after `rebuildProgressBar()` replaced them. It now dynamically queries the active DOM nodes on every scroll.
  - **Scroll State Shadowing:** Fixed an issue where the global `scrollProgress` was shadowed by a local `let` declaration, causing it to remain at 0 and wiping the bar during rebuilds.
  - **Completion State:** `scrollProgress` is now properly set to `1` when the bar is filled or restored from cache, ensuring rebuilds render the bar fully completed if it was already finished.
  - **Empty Load Fallback:** Removed an early return that completely aborted initialization if the initial Firestore payload was empty. The scroll listeners are now always attached so the bar works correctly when milestones are loaded lazily.
- **Dynamic Tooltip Collision Resolution:**
  - Added a responsive algorithm (`resolveTooltipCollisions()`) that calculates horizontal overlaps between adjacent milestone tooltips using `getBoundingClientRect()`.
  - When overlaps are detected, it symmetrically nudges both tooltips apart using a new CSS `--nudge-x` variable, iterating until no overlaps remain.
  - Added CSS rule for `.milestone-text::before` (the arrow) to apply a reverse `calc(50% - var(--nudge-x))` shift, ensuring the arrow always stays perfectly anchored over its respective progress dot even when the tooltip card is pushed horizontally.
  - The resolution logic runs smoothly on initial load, after real-time Firebase refreshes, and via a debounced window resize listener.
  - CSS updated to remove `white-space: nowrap` and add `max-width: 180px` and `word-wrap: break-word` to tooltips, allowing long titles to elegantly wrap across multiple lines, saving horizontal space and reducing the need for aggressive collision shifting.

---

## [1.16.8] - 2026-05-03

### Changed
- **Pong game — final 2-column layout:**
  - **Left panel (15%, `min-width: 120px`, `max-width: 200px`):** Contains all info stacked vertically with `overflow: hidden` enforced at every level — Title, Round X/3 + round wins (P — 0 — 0 — AI), Score squares (P □□□ / AI □□□), How to Play tips, Start/Reset buttons at the bottom.
  - **Canvas column (`flex: 1`):** Takes all remaining width and height. No other elements compete for space.
  - Removed separate `.pong-game-header`, `.pong-info-bar`, `.pong-game-area`, `.pong-side-panel` — replaced by `.pong-layout` + `.pong-panel` system.
  - Added `pong-panel-*` utility classes: `title`, `section`, `section--grow`, `label`, `row`, `side`, `vs`, `tip`.

---

## [1.16.7] - 2026-05-03

### Changed
- **Pong game layout final reorganization:**
  - Removed left column entirely (Round info / How to Play moved to inline elements).
  - Added **`.pong-info-bar`**: two compact horizontal rows stacked above the game area.
    - Row 1 (Rounds): `PLAYER — [playerRounds] — ROUND X/3 — [aiRounds] — AI`
    - Row 2 (Score):  `PLAYER — □□□ — SCORE — □□□ — AI`
  - **`.pong-game-area`** replaces `.pong-game-container`: `flex-direction: row` with canvas (`flex:1`) + side panel (`width: 160px`). Canvas now uses the maximum available width and height.
  - **`.pong-side-panel`**: How to Play instructions + Start/Reset buttons, vertically centered on the right.
  - Added `pib-*` CSS classes for info-bar typography (label, value, separator, center).

---

## [1.16.6] - 2026-05-03

### Changed
- **Pong game layout reorganized** to maximize canvas space:
  - **Header** reduced to title (1.4rem, was 2rem) + subtitle (0.8rem) with tighter gap — gives the game container more vertical room.
  - **Scoreboard moved** from the header into the right column, stacked above the Start Game button. Uses new `.pong-scoreboard--compact` modifier: `flex-direction: column`, smaller padding (0.6rem × 1rem), 14×14px score squares (was larger).
  - **Right column** now vertically centered with the scoreboard on top and controls below, aligned to the canvas height.
  - **Center column gap** reduced to `0` (was `1rem`) — canvas is the only child, no gap needed.
- **`portfolio/js/playgroundSection.js`:** Updated HTML template to reflect new structure.
- **`portfolio/css/playgroundSection.css`:** Added `.pong-scoreboard--compact` and all child overrides; shrunk `.pong-title` and `.pong-subtitle`; updated `.pong-game-header`, `.pong-right-column`, `.pong-center-column`.

---

## [1.16.5] - 2026-05-03

### Fixed
- **Pong canvas still overflowing — root cause identified:** `TowerDefenseModule` was being constructed (and `canvas.width/height` measured) *before* the module content had `display: flex` applied. `col.clientHeight` returned `0`, so the scale factor was always `0`, the canvas got the minimum clamped size (280px width), and CSS then had no reliable size to constrain against.
- **Scoreboard floating over canvas:** The `.pong-scoreboard` was a flex sibling of `#pongCanvas` inside `.pong-center-column`. Both competed for the column's height, causing the scoreboard to visually overlap the canvas when the column was too short.

### Changed
- **`portfolio/js/playgroundSection.js`:**
  - **`expandToModule()`:** Removed the premature `initializeModule()` call that ran before layout was calculated. `switchToModule()` now owns all init/resume logic.
  - **`switchToModule()`:** Wrapped `initializeModule()` / `resumeModule()` in a double `requestAnimationFrame()` so the browser has time to complete the layout reflow after adding `.active` to the content element before any canvas dimension is read.
  - **HTML template:** Moved `.pong-scoreboard` from inside `.pong-center-column` into `.pong-game-header`. Center column now contains only `#pongCanvas`, so `col.clientHeight` returns the pure canvas-available height with no scoreboard interference.
  - **`TowerDefenseModule.init()`:** Removed the `- 100` scoreboard offset from `availH` (scoreboard no longer in this column).
- **`portfolio/css/playgroundSection.css`:**
  - `.pong-game-header`: changed to `display: flex; flex-direction: column; align-items: center; flex-shrink: 0` so the title, scoreboard, and subtitle stack vertically and the header never competes with the game container for flex space.

---

## [1.16.4] - 2026-05-03

### Fixed
- **Pong canvas overflowing container (hardcoded 700×450 dimensions):** The canvas was always initialized at exactly 700×450 logical pixels regardless of the available space. Even with CSS `max-height: 100%`, the element's natural size would overflow smaller containers. Replaced the hardcoded dimensions with a dynamic fit-to-container calculation.

### Changed
- **`portfolio/js/playgroundSection.js`** — `TowerDefenseModule.init()`:
  - Measures `.pong-center-column` dimensions at init time.
  - Computes `sf = Math.min(availW / 700, availH / 450, 1)` — the largest scale factor that fits both axes, never exceeding 1× (original size).
  - Sets `canvas.width = round(700 * sf)` and `canvas.height = round(width * 450/700)` — always exact 700:450 proportions.
  - Scales all `config` values (`paddleWidth`, `paddleHeight`, `ballRadius`, `initialBallSpeed`, `maxBallSpeed`, `paddleSpeed`, `aiSpeed`) by the same factor. All game physics math already uses `canvas.width`/`canvas.height` as coordinate references, so no other game logic required changes.
  - Minimum canvas width clamped to 280px to prevent the game becoming unplayable on very narrow viewports.
- **`portfolio/css/playgroundSection.css`** — `.pong-canvas`: removed `aspect-ratio: 700/450` and `width: 100%` (the JS now sets the correct pixel dimensions directly). Kept `max-width: 100%; max-height: 100%` as overflow-prevention safety constraints only.

---

## [1.16.3] - 2026-05-03

### Fixed
- **Model Viewer — side panels squeezing the canvas:** The previous vertical layout (canvas on top, 3D Models + Textures below) left the canvas with too little height and no control over the panel width. Redesigned to a **3-column grid layout**: `[Models 15%] [Canvas 1fr] [Textures 15%]`. The side panels are now fixed-width columns flanking the canvas, which takes all remaining space.
- **Model Viewer — panels not scrollable:** Added `overflow-y: auto` to `.models-content` inside each side panel so long asset lists scroll independently without affecting the canvas height.
- **Model Viewer — panels not collapsible:** Added a `▼` collapse button to the header of each side panel. Clicking it toggles `.collapsed` on the parent `.asset-card`, hiding `.models-content` via CSS. After the CSS transition completes (320ms), `handleResize()` is called on the Three.js renderer so the WebGL viewport recalculates to the new canvas width.
- **Pong canvas overflowing container:** `.pong-center-column` now has `overflow: hidden` to clip the canvas to the flex track. Canvas gets `aspect-ratio: 700 / 450; width: 100%; max-height: 100%; height: auto` — it scales proportionally to fill the column width without ever exceeding the container height.

### Changed
- **`portfolio/css/playgroundSection.css`:**
  - `.model-viewer-module`: `flex-direction: column` → `display: grid` with `grid-template-areas: "models canvas textures"`.
  - `.model-lists-row`: `display: flex` → `display: contents` (children participate directly in parent grid).
  - `.models-list.asset-card` and `.textures-list.asset-card`: assigned to `grid-area: models` and `grid-area: textures` respectively.
  - `.model-viewer-wrapper`: `flex: 1` → `grid-area: canvas`.
  - `.pong-center-column`: added `overflow: hidden`.
  - `.pong-canvas`: added `aspect-ratio: 700 / 450; width: 100%; display: block`.
- **`portfolio/js/playgroundSection.js`:**
  - Added `▼` collapse button markup to each `asset-card-header`.
  - Added `ModelViewerModule.setupCollapseButtons()` — wires collapse toggle with CSS class + Three.js resize recalculation.

---

## [1.16.2] - 2026-05-03

### Reverted
- **`portfolio/css/playgroundSection.css`** — `.playground-grid` reverted from `repeat(auto-fill, minmax(min(280px, 100%), 1fr))` back to **`repeat(2, 1fr)`** (the original v1.14 two-column layout). The auto-fill change incorrectly produced 3+ columns at wide viewports and broke the intentional 2×2 card layout.
- **`portfolio/js/modelViewerCanvas.js`** — `ResizeObserver` removed; reverted to `window.addEventListener('resize', () => this.handleResize())`. The observer was firing on `--ui-scale` CSS variable changes and triggering `renderer.setSize()` calls that resized the Three.js canvas away from its intended fixed dimensions.
- **`portfolio/js/playgroundSection.js`** — `TowerDefenseModule`: `ResizeObserver` removed from `init()`, `pause()`, and `resume()`. The playground section intentionally uses fixed pixel dimensions inherited from v1.14; the observer was causing unwanted re-renders on parent-element dimension changes.

### Design note
The playground section (grid, Three.js canvas, Pong canvas) uses intentionally fixed dimensions from v1.14. UI scaling (`zoomCompensation.js`) affects only token-driven properties; the playground maintains its own fixed-size layout independently.

---

## [1.16.1] - 2026-05-03

### Fixed
- **Milestones written to Firestore not appearing in the progress bar (High):** `refreshProgressBarMilestones()` was a silent no-op in two cases:
  1. When `createProgressBar()` had rendered a milestone without a date (the `dateStr` was empty), the `.milestone-date` element was never injected into the HTML. The old refresh function used `if (dateEl)` — so it could never update a date that wasn't there at initial render time. If a date was later added to that milestone record in Firestore, the `onSnapshot` fired, but the DOM update silently dropped the new date.
  2. If the bar had not yet been injected into the DOM when a snapshot fired (race condition on slow pages), `querySelectorAll('.milestone-text')` returned an empty NodeList with no error — the call was a no-op.

### Changed
- **`portfolio/js/progressBar.js`** — `refreshProgressBarMilestones()` rewritten:
  - **Early-exit guard:** if `milestoneTexts.length === 0` the function returns immediately, preventing silent no-ops on pre-init snapshots.
  - **On-demand `.milestone-content` creation:** if the element is somehow absent, it is created and prepended before updating.
  - **On-demand `.milestone-date` creation:** if `dateStr` is non-empty but no `.milestone-date` element exists in the wrapper, one is created and appended. This makes date updates fully idempotent regardless of what was present at initial render time.
  - **Graceful date removal:** if a milestone's date is cleared in Firestore, the existing `.milestone-date` element has its text cleared without being removed from the DOM.

---

## [1.16.0] - 2026-05-03

### Fixed
- **Playground section breaking layout after UI resize (Critical):** The playground card grid, Three.js canvas, and Pong canvas all failed to reflow after `--ui-scale` changes because they relied on `window.addEventListener('resize')`, which fires only on viewport size changes, not on parent-element dimension changes caused by CSS custom-property updates.

### Changed
- **`portfolio/css/playgroundSection.css`** — `.playground-grid`:
  - Changed `grid-template-columns` from `repeat(2, 1fr)` (fixed 2-column) to `repeat(auto-fill, minmax(min(280px, 100%), 1fr))`. Cards now flow into 1 or 2 columns depending on available width. `min(280px, 100%)` prevents overflow on very narrow containers (e.g. a single-column mobile view). The `768px` media-query override to `1fr` is retained for complete mobile coverage.
- **`portfolio/js/modelViewerCanvas.js`** — `ModelViewerCanvas`:
  - Replaced `window.addEventListener('resize', () => this.handleResize())` with a `ResizeObserver` on `this.canvas.parentElement` (the `.model-viewer-container`). The observer fires whenever the container's own dimensions change — including after `--ui-scale` CSS token updates — and calls the existing `handleResize()` which already does `renderer.setSize()` + `camera.aspect` + `updateProjectionMatrix()`.
  - Falls back to `window.addEventListener('resize')` on browsers without `ResizeObserver` support.
  - `destroy()` now calls `this._resizeObserver.disconnect()` to prevent stale callbacks on a disposed renderer.
- **`portfolio/js/playgroundSection.js`** — `TowerDefenseModule` (Pong):
  - Added a `ResizeObserver` on `.pong-center-column` (or `canvas.parentElement` as fallback) in `init()`. The Pong canvas coordinate space remains at 700×450 (intentional — CSS scales it with `max-width/height: 100%` and mouse events already apply the correct `scaleY = canvas.height / rect.height`). The observer only triggers a `this.render()` call to flush a fresh frame at the new visual size.
  - `pause()` now calls `_resizeObserver.disconnect()` to avoid callbacks on hidden modules.
  - `resume()` re-attaches the observer so it's active when the module is visible again.

---

## [1.15.9] - 2026-05-03

### Fixed
- **Progress bar milestones not updating on new database entries (Critical):** Milestone data was loaded via a one-time cache-first `get()` call. New or edited milestones in Firestore only appeared after a page reload.

### Changed
- **`portfolio/js/progressBar.js`:**
  - **`loadMilestones()` rewritten:** Now returns a `Promise` and uses `onSnapshot()` instead of a one-time `get()`. localStorage cache is still served immediately on first call so the bar renders fast (no visible delay). The 8-second timeout guard remains in place — if Firestore is unreachable, the function resolves with the cached/fallback data and onSnapshot errors are caught without crashing.
  - **`milestonesUnsubscribe`** — module-level variable stores the listener cleanup handle. Any previous listener is torn down before a new one is created (prevents duplicate listeners on re-init).
  - **`refreshProgressBarMilestones()` (new):** Surgically updates only `.milestone-content`, `.milestone-date`, and mobile list text nodes on subsequent snapshot events, without resetting scroll lock state, animation progress, or completion status.
  - **`visibilitychange` cleanup:** When the tab is hidden, `milestonesUnsubscribe()` is called to release the Firestore connection and prevent unnecessary reads on background tabs.
- **`portfolio/css/resume.css`** — Verified: `.skills-grid` already uses `grid-template-columns: repeat(auto-fit, minmax(200px, 1fr))` with `overflow: visible`, correctly reflowing on any number of skill entries. No CSS changes were required.

---

## [1.15.8] - 2026-05-03

### Fixed
- **Custom scrollbar drag position incorrect (Critical):** Drag logic used `mouseY - thumbHeight / 2` on every `mousemove`, snapping the thumb to be centred under the cursor on every frame instead of moving it relative to where the user grabbed it. This produced erratic jumps and wrong scroll positions.

### Changed
- **`shared/js/simpleScrollbar.js`** — Full rewrite (v2.0):
  - **Grab-offset delta dragging:** `mousedown` now records `grabOffsetY = e.clientY - thumbRect.top` (where inside the thumb the user clicked). `mousemove` then computes `pointerInTrack = e.clientY - trackTop - grabOffsetY` — thumb moves exactly with the cursor at the grab point, no snapping.
  - **Track-relative coordinate math:** All position calculations use `scrollbar.getBoundingClientRect()` as the coordinate origin, eliminating viewport-offset errors.
  - **Touch support (new):** `touchstart`/`touchmove`/`touchend` implemented with the same grab-offset logic using `e.touches[0].clientY`. All touch listeners set `{ passive: true }`.
  - **Keyboard support (new):** `thumb` is `tabindex="0"` and responds to `ArrowUp/Down`, `PageUp/Down` via `scrollBy`.
  - **ARIA (new):** `role="scrollbar"`, `aria-orientation`, `aria-valuenow/min/max` updated on every `updateThumb()` call.
  - **Drag/scroll conflict resolved:** `scroll` event handler skips `updateThumb()` while `isDragging` is true, preventing the scroll event from fighting the drag mid-gesture.
  - **`window.syncScrollbar()` (new):** Public API that forces a `updateThumb()` recalculation — called by `zoomCompensation.js` after every `--ui-scale` change.
- **`portfolio/js/zoomCompensation.js`:** Added `requestAnimationFrame(() => window.syncScrollbar?.())` after every `--ui-scale` `setProperty` call in `applyScaleToken()`, `setUIScale()`, and `toggleZoomCompensation()` so the thumb stays geometrically correct after zoom changes.

---

## [1.15.7] - 2026-05-03

### Verified (No Change Required)
- **Footer links audit (ticket #8):** Full audit confirmed no absolute hardcoded paths (`href="/portfolio/..."`) exist anywhere in the codebase. Specific findings:
  - **`portfolio/index.html` footer** — Quick Links already use clean hash anchors (`#hero`, `#projects`, `#about_me`, `#playground`, `#contact`). ✅
  - **`blog/index.html` footer** — Uses `../portfolio/index.html#hero` etc., which are *correct* relative paths for cross-page navigation from the blog subdirectory. Cannot be shortened to `#anchor` as that would target elements on the blog page itself. ✅
  - **`navigation.js` / `fullBlog.js`** — Use `../portfolio/index.html#blog` for programmatic navigation back to the portfolio. Correctly relative. ✅
  - `grep href="/portfolio/"` (leading-slash absolute) returned **zero matches** across all HTML and JS files. ✅

---

## [1.15.6] - 2026-05-03

### Fixed
- **Empty category tabs in Projects section (Low):** Tabs for "Apps", "Internal Projects", "Modeling" etc. displayed raw "No projects in this category yet." text — a bare `<p>` with no design treatment, creating a hollow first impression.

### Changed
- **`projects-portfolio.js`:**
  - `render()` — "no items" path now injects a premium `.empty-state` card (icon, title, contextual message, "COMING SOON" pill) instead of a bare paragraph. Message is dynamic: says "No **apps** projects yet" for a specific tab, or a generic admin hint for the "All" view.
  - Added `updateTabVisibility()` — after projects load, sets `hidden` on any category filter button whose `data-category` has zero matching projects. "All Projects" is always shown. Prevents empty tabs from ever being clickable.
  - `initialize()` — calls `updateTabVisibility()` between `loadProjects()` and `setupEventListeners()`.
- **`projects-new.css`:** `.no-projects` hidden (superseded). New `.empty-state`, `.empty-state-icon` (CSS-masked folder SVG), `.empty-state-title`, `.empty-state-message`, `.empty-state-hint` rules with fade-in animation. `.category-filter-btn[hidden]` selector ensures hidden tabs take no layout space.

---

## [1.15.5] - 2026-05-03

### Fixed
- **Copyright year shows "© 2025" (Medium):** Hardcoded year replaced with a dynamic value across all pages.

### Changed
- **`shared/js/footer.js`:** Added `setCopyrightYear()` — reads `new Date().getFullYear()` and writes it to `#copyright-year` on every page load. Runs on both `DOMContentLoaded` and immediate execution paths. Being in the shared footer script means every page gets this for free with no duplication.
- **`portfolio/index.html`** and **`blog/index.html`:** `© 2025` replaced with `© <span id="copyright-year">2026</span>`. The `2026` fallback ensures a correct display in no-JS environments; JS overwrites it at runtime.

---

## [1.15.4] - 2026-05-03

### Fixed
- **Accessibility: `aria-invalid` on untouched form fields (Medium):** The contact form had no ARIA error management at all — validation was communicated only via `borderColor` style changes (invisible to screen readers) and a blocking `alert()` on submit. Fixed with a full ARIA-compliant validation layer:
  - `aria-invalid` is **never set in markup**; only added by JS after the user attempts to submit with an empty required field.
  - `aria-invalid` is **removed** as soon as the field receives a valid value.
  - Each required field (`name`, `email`, `role`) has an `aria-describedby` pointing to a hidden `<span class="form-error" aria-live="polite">` — screen readers announce errors in-place without interrupting the user's flow.
  - `aria-required="true"` added to all required fields.
  - Replaced `alert('Please fill in all required fields.')` with inline per-field error messages; focus moves to the first invalid field automatically.
  - On `resetForm()`, all `aria-invalid` and error message states are cleared.

### Changed
- **`contactSection.js`:** New `setFieldError(field, message|null)` helper centralises all ARIA + visual error state. `validateField()` refactored to use it. Submit handler rebuilt around per-field error objects.
- **`portfolio/index.html`:** Added `aria-required`, `aria-describedby`, and `<span class="form-error">` to `name`, `email`, and `role` fields.
- **`contactSection.css`:** Added `.form-error` styles (VT323 font, red, fade-in animation) and `[aria-invalid="true"]` CSS selector for consistent visual feedback across all themes.

---

## [1.15.3] - 2026-05-03

### Fixed
- **Page title audit (ticket #3):** No `"Professional"` title found — all pages already had correct, descriptive titles. Confirmed across all HTML files.

### Changed
- **Title separator standardised to en-dash (–):** Updated `<title>`, `og:title`, and `twitter:title` on `portfolio/index.html`, `landing/index.html`, `blog/index.html`, and `blog/post.html` from a hyphen `-` to an en-dash `–` for typographic correctness and consistency with the ticket specification.

---

## [1.15.2] - 2026-05-03

### Fixed
- **Mini Blog stuck on "Loading…" (High):** The blog section could hang indefinitely due to two compounding issues:
  1. **Missed-event race condition in `firebase-init.js`:** `waitForFirebase` registered a `firebase-ready` listener that could be added *after* the event had already fired (lazy-loaded observer triggers after fast Firebase init). Fixed by racing the in-flight `initPromise` directly against the timeout, so no event needs to be re-dispatched.
  2. **No overall deadline on `initializeBlogSection`:** The blog skeleton had no hard maximum wait time. Fixed with an 8-second `Promise.race` deadline in `initializeBlogSection` — skeleton is always replaced (with posts, empty state, or error state) within 8 s.

### Changed
- **`firebase-init.js`:** `waitForFirebase` default timeout lowered from 10 s → 6 s; race-condition logic rewritten (see above).
- **`blogSection.js`:** Full rewrite of error-handling flow. Distinguishes network error (Firebase unreachable) from soft-empty state (no posts). `displayErrorState(isNetworkError)` renders either a "Retry" button (network error) or friendly "Coming Soon" placeholders (empty collection).
- **`portfolio/index.html`:** Blog skeleton HTML upgraded: 3 shimmer skeleton cards instead of 1; removed literal "Loading..." text strings that persisted if JS failed.

---

## [1.15.1] - 2026-05-02

### Fixed
- **Screen Lock on UI Resize (Critical):** Removed `transform: scale()` + `width`/`minHeight` overrides applied to `document.body` in `zoomCompensation.js`. These caused scroll events to desynchronise from element geometry, freezing page scrolling on HiDPI and zoomed displays.

### Changed
- **`zoomCompensation.js` (portfolio/js):** Fully rewritten. Zoom detection logic is preserved; compensation now sets only `--ui-scale` on `:root` via `document.documentElement.style.setProperty`. No layout geometry is mutated.
- **`style.css` (portfolio/css):** Added a `--ui-scale` CSS custom property system at `:root` with `clamp()`-bounded type (`--text-xs` → `--text-2xl`) and spacing (`--space-1` → `--space-16`) tokens. Removed the now-obsolete `.zoom-compensated` overflow hack.
- **New public API:** `window.setUIScale(value)` allows future UI sliders to drive the scale (clamped 0.65–2) without touching transforms. `window.toggleZoomCompensation(bool)` API retained.

---

## [1.14.1] - 2026-03-27

### Added
- Added `.gitignore` with a whitelist approach to focus on documentation and core configuration.
- Initialized comprehensive version history in `CHANGELOG.md`.

### Changed
- Renamed `00-README.md` to `README.md` in the documentation folder for standard naming.
- Consolidated project structure for public release.

---

## [1.14.0] - 2026-03-26
### Changed
- Final refinements to documentation before archiving version 1.14.

## [1.13.0] - 2026-03-20
### Improved
- Documentation consolidation: Refined `02-technical-deep-dive.md` with detailed implementation notes.
- Performance optimizations for 3D model loading.

## [1.12.0] - 2026-03-10
### Changed
- Internal architecture cleanup and shared script refinements.

## [1.11.0] - 2026-02-28
### Added
- Comprehensive Zoom Compensation System (`zoomCompensation.js`) to handle HiDPI and system scaling.
- Mobile and tablet detection for adaptive layout scaling.

## [1.10.0] - 2026-02-15
### Added
- `CV-RESUME-DATA.md` with structured career data.
- Expanded technical documentation regarding Firebase initialization.
### Fixed
- Duplicate Firebase app instances bug resolved via centralized `firebase-init.js`.

## [1.9.0] - 2026-02-01
### Added
- Dual-sticky Projects Banner with smooth horizontal marquee.
- Low-spec mode exemptions for essential UI animations.

## [1.8.0] - 2026-01-20
### Added
- Guest Authentication System for secure NDA project viewing.
- Session management with localStorage persistence and expiration.
- Floating NDA Reminder Box for authenticated sessions.

## [1.5.0] - 2025-12-28
### Added
- NDA Access System with manual approval workflow.
- Automated PDF generation for signed NDA agreements using jsPDF.
- Email notifications for admin via EmailJS.
- Cookie Consent System (GDPR-compliant) with analytics control.
- Private project visibility settings in Admin Dashboard.

## [1.1.0] - 2025-11-15
### Added
- Initial production-ready release.
- Three.js 3D project showcase integration.
- Multi-theme system (Day, Night, Cyberpunk, Monochrome, Blueprint).
- Full Admin Dashboard (Projects, Blog, About Me, Resume, Milestones).
- Firebase Authentication and Firestore integration.
- BIOS-style landing page.
