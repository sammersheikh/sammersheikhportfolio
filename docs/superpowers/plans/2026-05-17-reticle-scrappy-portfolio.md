# Reticle + Scrappy Portfolio Pages Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add Reticle and Scrappy as polished portfolio case-study pages and homepage cards, without exposing private GitHub source links.

**Architecture:** Keep the portfolio as a static HTML/CSS site. Reuse the visual language from `workout-tracker.html` for two new standalone pages, and update `index.html` with clickable project cards that route to those pages.

**Tech Stack:** Static HTML, inline CSS, existing image assets, Python `http.server` for local preview.

---

## File Structure

- Modify `index.html`
  - Add Reticle and Scrappy cards to the Featured Work grid.
  - Link cards to `reticle.html` and `scrappy.html`.
  - Do not add GitHub/source links for either app.
- Create `reticle.html`
  - Standalone landing/case-study page for the Reticle macOS screenshot/annotation app.
  - Match the modern dark page style used by `workout-tracker.html`.
- Create `scrappy.html`
  - Standalone landing/case-study page for the Scrappy macOS menu-bar Android mirroring app.
  - Match the modern dark page style used by `workout-tracker.html`.
- No new build system or package manager changes.

---

### Task 1: Add Reticle Standalone Page

**Files:**
- Create: `reticle.html`

- [ ] **Step 1: Create the Reticle page**

Create `reticle.html` with:
- `<title>Reticle — Screenshot faster</title>`
- Meta description: `A minimalist macOS screenshot and annotation tool built for instant capture, markup, clipboard sharing, and smart OCR filenames.`
- Navigation back to `index.html`.
- Hero copy positioning Reticle as a faster macOS screenshot workflow.
- CTA area without a GitHub/source link.
- Three visual mock panels: capture, annotate, smart naming.
- Five feature sections:
  1. Capture without friction
  2. Annotate like you mean it
  3. Chain screenshots together
  4. Name files with OCR
  5. Tune the workflow
- Tech stack strip: Electron 28, Fabric.js, macOS Vision OCR, electron-store, electron-builder.

- [ ] **Step 2: Verify Reticle page statically**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
text = Path('reticle.html').read_text()
assert '<title>Reticle — Screenshot faster</title>' in text
assert 'github.com' not in text.lower()
assert 'Electron 28' in text
assert 'Fabric.js' in text
assert 'macOS Vision OCR' in text
print('reticle.html ok')
PY
```

Expected output:

```text
reticle.html ok
```

- [ ] **Step 3: Commit Reticle page**

```bash
git add reticle.html
git commit -m "feat: add reticle portfolio page"
```

---

### Task 2: Add Scrappy Standalone Page

**Files:**
- Create: `scrappy.html`

- [ ] **Step 1: Create the Scrappy page**

Create `scrappy.html` with:
- `<title>Scrappy — Android mirroring from the menu bar</title>`
- Meta description: `A tiny macOS menu-bar app for scrcpy mirroring, wireless ADB pairing, and per-device Android screen control.`
- Navigation back to `index.html`.
- Hero copy positioning Scrappy as a menu-bar control center for Android mirroring.
- CTA area without a GitHub/source link.
- A menu mockup inspired by the README example.
- Five feature sections:
  1. Mirror any attached device
  2. Pair over Wi‑Fi
  3. Control sessions individually
  4. See state at a glance
  5. Recover without Terminal
- Tech stack strip: Python, rumps, adb, scrcpy, launchd.

- [ ] **Step 2: Verify Scrappy page statically**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
text = Path('scrappy.html').read_text()
assert '<title>Scrappy — Android mirroring from the menu bar</title>' in text
assert 'github.com' not in text.lower()
assert 'scrcpy' in text
assert 'Wireless debugging' in text
assert 'launchd' in text
print('scrappy.html ok')
PY
```

Expected output:

```text
scrappy.html ok
```

- [ ] **Step 3: Commit Scrappy page**

```bash
git add scrappy.html
git commit -m "feat: add scrappy portfolio page"
```

---

### Task 3: Add Homepage Cards

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add Reticle and Scrappy cards to Featured Work**

Add two clickable cards in the Featured Work grid near the other personal app cards:

- Reticle
  - Tech: `Electron • Fabric.js • macOS Vision OCR`
  - Copy: `A menu-bar screenshot tool with instant clipboard capture, a full annotation editor, screenshot chaining, and OCR-powered smart filenames.`
  - Metric: `1.3.0` / `Current Release`
  - Link: `reticle.html`

- Scrappy
  - Tech: `Python • rumps • adb • scrcpy`
  - Copy: `A tiny macOS menu-bar app for Android mirroring, wireless debugging pairing, per-device scrcpy sessions, and ADB recovery controls.`
  - Metric: `5s` / `Device Refresh`
  - Link: `scrappy.html`

Do not include GitHub links.

- [ ] **Step 2: Verify homepage links**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
text = Path('index.html').read_text()
assert 'href="reticle.html"' in text
assert 'href="scrappy.html"' in text
assert 'Reticle' in text
assert 'Scrappy' in text
print('index.html project links ok')
PY
```

Expected output:

```text
index.html project links ok
```

- [ ] **Step 3: Commit homepage cards**

```bash
git add index.html
git commit -m "feat: feature reticle and scrappy on homepage"
```

---

### Task 4: Local Preview and Validation

**Files:**
- No file changes expected.

- [ ] **Step 1: Run static link/title validation**

Run:

```bash
python3 - <<'PY'
from html.parser import HTMLParser
from pathlib import Path
from urllib.parse import urlparse

pages = ['index.html', 'workout-tracker.html', 'reticle.html', 'scrappy.html']

class Parser(HTMLParser):
    def __init__(self):
        super().__init__()
        self.refs = []
        self.title = ''
        self.in_title = False
    def handle_starttag(self, tag, attrs):
        if tag == 'title':
            self.in_title = True
        d = dict(attrs)
        for attr in ['href', 'src']:
            if attr in d:
                self.refs.append(d[attr])
    def handle_endtag(self, tag):
        if tag == 'title':
            self.in_title = False
    def handle_data(self, data):
        if self.in_title:
            self.title += data

for page in pages:
    parser = Parser()
    parser.feed(Path(page).read_text(errors='ignore'))
    assert parser.title.strip(), f'{page} missing title'
    for ref in parser.refs:
        if ref.startswith(('#', 'mailto:', 'tel:', 'data:')):
            continue
        parsed = urlparse(ref)
        if parsed.scheme in ('http', 'https'):
            continue
        target = Path(page).parent / ref.split('#')[0].split('?')[0]
        assert target.exists(), f'{page} missing local ref {ref}'
    print(f'{page}: {parser.title.strip()}')
print('static validation ok')
PY
```

Expected output includes:

```text
index.html: Sammer Sheikh
workout-tracker.html: Workout Tracker — Track your lifts
reticle.html: Reticle — Screenshot faster
scrappy.html: Scrappy — Android mirroring from the menu bar
static validation ok
```

- [ ] **Step 2: Start local server on a stable port**

Run:

```bash
python3 -m http.server 8765
```

Expected output:

```text
Serving HTTP on :: port 8765 ...
```

- [ ] **Step 3: Verify local pages over HTTP**

Run:

```bash
for path in / /reticle.html /scrappy.html /workout-tracker.html; do
  curl -s -o /dev/null -w "%{http_code} %{url_effective}\n" "http://127.0.0.1:8765$path"
done
```

Expected output:

```text
200 http://127.0.0.1:8765/
200 http://127.0.0.1:8765/reticle.html
200 http://127.0.0.1:8765/scrappy.html
200 http://127.0.0.1:8765/workout-tracker.html
```

- [ ] **Step 4: Share Tailscale-accessible URL for review**

Determine the Tailscale IP/name with:

```bash
tailscale ip -4 2>/dev/null || hostname
```

Share:

```text
http://<tailscale-ip-or-hostname>:8765/
```

- [ ] **Step 5: Do not deploy yet**

Stop after sharing the local preview URL. Wait for Sammer's visual review and approval before merging/pushing/deploying.
