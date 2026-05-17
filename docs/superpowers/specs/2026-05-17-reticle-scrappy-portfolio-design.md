# Reticle + Scrappy Portfolio Pages Design

## Goal
Add the two most recent GitHub projects — Reticle and Scrappy — to the portfolio in the same polished, product-focused style as the Workout Tracker page. The pages should market the apps and workflows without linking visitors to the private GitHub source.

## Scope
- Add homepage project cards for Reticle and Scrappy.
- Create `reticle.html` as a standalone case-study/landing page.
- Create `scrappy.html` as a standalone case-study/landing page.
- Keep source/GitHub links hidden for now because both repos are private.
- Serve locally from the feature branch so Sammer can review over Tailscale before anything is merged or deployed.

## Existing Patterns
- `index.html` is a static single-page portfolio with featured work cards.
- `workout-tracker.html` is the style reference: bold hero, concise copy, feature sections, screenshots/cards, and a simple footer.
- There is no package/build system; implementation stays plain HTML/CSS/assets.

## Page Content

### Reticle
Position Reticle as a minimalist macOS screenshot and annotation tool for faster capture-to-clipboard workflows.

Key points:
- Area/fullscreen/window capture with shortcut-driven workflow.
- Instant clipboard copy and corner preview.
- Annotation editor with arrows, shapes, text, highlight, blur, counters, crop, undo/redo.
- Screenshot chaining for multi-step captures and paste-all workflows.
- OCR-based smart filenames, with optional local AI naming support.
- Tech stack: Electron, Fabric.js, electron-store, electron-builder, macOS Vision OCR.

### Scrappy
Position Scrappy as a tiny macOS menu-bar tool that makes Android mirroring and wireless debugging faster.

Key points:
- Lists ADB-attached devices in the menu bar.
- Starts/stops scrcpy mirroring per device.
- Shows mirror state in the menu-bar icon.
- Supports wireless pairing and reconnect-by-IP flows.
- Provides refresh, stop-all, and ADB restart controls.
- Tech stack: Python, rumps, adb, scrcpy, launchd, macOS app bundling.

## Homepage Changes
Add Reticle and Scrappy to the featured work grid as clickable cards. Keep the current portfolio tone: outcome-oriented and product-builder focused.

Suggested placement:
1. Existing work/project cards remain.
2. Add Reticle and Scrappy near Workout Tracker/Nourish so personal apps are grouped together.

## Visual Direction
Match `workout-tracker.html` rather than the older Colorlib template:
- Dark, clean, high-contrast layout.
- Large headline and short subtitle.
- Compact feature sections with product screenshots/visual mock cards.
- Trendy, minimal app-marketing feel.

If screenshots are not available, use tasteful UI mock panels instead of leaving empty image placeholders.

## Review Flow
1. Implement on branch `feature/add-reticle-scrappy-pages`.
2. Run a local static server.
3. Share the local/Tailscale URL for review.
4. Adjust based on feedback before merging/deploying.

## Non-goals
- Do not make Reticle or Scrappy public.
- Do not add GitHub/source buttons for these apps yet.
- Do not deploy without explicit approval after local review.
- Do not add a new build system.
