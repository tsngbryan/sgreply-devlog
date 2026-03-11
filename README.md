# SGReply DevLog Viewer

A personal dev dashboard that reads the session log from the sgreply repo and renders it beautifully — random theme, motivational quote, and milestone progress on every load.

## Setup (5 min)

### 1. Edit `index.html` — set your GitHub username
Open `index.html` and find the `CONFIG` block near the top of the `<script>`. Change:
```js
githubOwner: 'YOUR_GITHUB_USERNAME',
```
to your actual GitHub username. The sgreply repo must be **public** for the raw fetch to work without auth.

### 2. Deploy to GitHub Pages (free)
```bash
cd sgreply-devlog
git init
git add .
git commit -m "init devlog viewer"
gh repo create sgreply-devlog --public --push --source=.
# Then: GitHub repo → Settings → Pages → Branch: main, folder: / (root)
```
Your viewer will be live at `https://YOUR_USERNAME.github.io/sgreply-devlog/`

### 3. Bookmark it
Open it every morning. Theme and quote change randomly on each load.

---

## Features
- **8 colour themes** — switch via dots in top-right corner; preference saved in localStorage
- **Random motivational quote** from 22 curated founders/thinkers on each load
- **Animated star field** — subtle twinkling background
- **Mission + milestone progress bars** — edit in `CONFIG.milestones`
- **Session log cards** — fetched live from `.claude/session-log.md` in your sgreply repo
- **Fade-in animations** on scroll

## Customisation
All editable in the `CONFIG` object in `index.html`:
| Key | What it does |
|---|---|
| `githubOwner` | Your GitHub username |
| `githubRepo` | The repo containing the session log |
| `githubBranch` | Branch (default: `main`) |
| `logPath` | Path to the markdown log file |
| `mission` | The text shown in the Mission card |
| `milestones` | Array of `{ name, pct }` for progress bars |

## Session log auto-update
The `sgreply` backend repo's `CLAUDE.md` now instructs Claude to automatically update `.claude/session-log.md` at the end of every session where changes are made — no prompting needed.