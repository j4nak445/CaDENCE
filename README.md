<div align="center">

# 🛡️ Cadence

**A local-first security dashboard — tasks, habits, and a daily CVE/PoC reading routine, with live cybersecurity & AI-security news.**

No build step · No backend · No accounts · No trackers · One `index.html`

</div>

<div align="center">

![Cadence on desktop](screenshots/desktop.png)

</div>

<div align="center">

<img src="screenshots/today.png" width="31%" alt="Today dashboard" />
<img src="screenshots/learn.png" width="31%" alt="Daily CVE & PoC reading" />
<img src="screenshots/ai.png" width="31%" alt="AI security news" />

</div>

---

## What it is

Cadence is a single static web page that helps you build a daily security habit.
It combines a to-do list and habit tracker with a **"read something real every
day"** routine: two *known-exploited* CVEs and one proof-of-concept to study,
drawn live from authoritative public sources — plus cybersecurity and AI/LLM
security news. Everything you create stays on your own device.

It's built for learners. I use it to give my students a small daily dose of
real-world vulnerabilities to understand and defend against.

## Features

| Tab | What it does |
|-----|--------------|
| **Today** | Greeting with your name, a progress ring, habits due, tasks due, and your reading streak. |
| **Tasks** | To-do list with due dates, priority, filters (Today / Upcoming / All / Done) and search. |
| **Habits** | Daily or weekday habits with current & best streaks, a 7-day row, and a 3-month heatmap. |
| **Learn** | **2 known-exploited CVEs + 1 proof-of-concept each day**, each with a "how to study this" guide and links to NVD, CVE.org and GitHub. Plus a **live search** across the CVE catalog and GitHub PoC repos. |
| **News** | Latest cybersecurity headlines. |
| **AI** | LLM / AI-security stories, auto-sorted into Jailbreaks, Prompt injection, Launches, and Defense. |

The daily picks are **deterministic per day** — everyone gets a stable set that
rotates at midnight (there's a shuffle button for a fresh draw). Mark all three
as read to keep your streak alive.

## Live demo

After you deploy (below), your copy lives at:

```
https://<your-username>.github.io/<repo-name>/
```

---

## 🚀 Deploy to GitHub Pages (≈2 minutes)

1. Create a new repository, e.g. `cadence`.
2. Upload the project files to the repo root. The only required file is
   **`index.html`** — the rest (`README.md`, `LICENSE`, `screenshots/`) are
   optional but make a nicer repo:
   ```
   your-repo/
   ├── index.html          ← the whole app
   ├── README.md
   ├── LICENSE
   └── screenshots/
       ├── today.png
       ├── learn.png
       ├── ai.png
       └── desktop.png
   ```
3. Go to **Settings → Pages**.
4. Under *Build and deployment*: **Source → Deploy from a branch**, branch
   **`main`**, folder **`/ (root)`**, then **Save**.
5. Wait ~1 minute. Your site is live at the URL above.

On your phone, open that URL in Chrome → **⋮ → Add to Home screen** to run it
full-screen like an app.

> ### ⚠️ Why it must be hosted
> The **Tasks** and **Habits** tabs work fully offline. But the **Learn / News /
> AI** tabs fetch live data, and browsers only allow those cross-site requests
> from a real `https://` origin. So the feeds work on GitHub Pages — but **not**
> if you just double-click the file (`file://`). That's the whole reason to host it.

---

## 📡 Data sources — one genuine source per task

All free, public, and **no API key required**.

| Purpose | Source | Why this one |
|---------|--------|--------------|
| **CVEs** | [CISA Known Exploited Vulnerabilities](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) (official U.S. gov catalog, via its GitHub mirror) | The authoritative list of vulnerabilities **actively exploited in the wild** — the highest-signal place to start. |
| **Proof-of-concept** | [GitHub](https://github.com/search?q=CVE&type=repositories) code search | The largest public index of researcher PoCs and write-ups. |
| **Cyber news** | [The Hacker News](https://thehackernews.com) | Widely-read daily security reporting. |
| **AI / LLM security** | [Hacker News](https://news.ycombinator.com) (Algolia search) | Surfaces LLM launches, jailbreaks and defense research as the community posts them. |

> **GitHub rate limit:** unauthenticated search allows ~10 requests/min per IP.
> The daily reading is cached, so normal use stays well under that.

---

## 🔒 Privacy & security

This is a security tool, so the model is spelled out in full.

**There is no server, so there is no central database to breach.** Every user's
data lives only in *their own browser*, on *their own device*. As the host you
never receive it, and there's nothing to leak.

**Measures built into the page:**
- **Strict Content-Security-Policy** — `default-src 'none'` with an explicit
  `connect-src` allowlist. The page can contact **only** these eight hosts and
  nothing else: `raw.githubusercontent.com`, `cdn.jsdelivr.net`,
  `api.github.com`, `thehackernews.com`, `feeds.feedburner.com`,
  `api.rss2json.com`, `api.allorigins.win`, `hn.algolia.com`.
- **No third-party JavaScript.** All script is inline and pinned by **SHA-256
  hash** in the CSP — no external libraries, nothing to hijack via a CDN.
- **All remote text is HTML-escaped** before display (no stored-XSS from a feed),
  and **outbound links are scheme-checked** (`http(s)` only).
- **`referrer` disabled** and `rel="noopener noreferrer"` on every external link.
- **No cookies, no accounts, no analytics.**
- **Offline-tolerant:** the last successful fetch of each feed is cached, so
  today's reading still opens without a connection.

**Honest residual notes (good to teach, not weaknesses in the code):**
- `localStorage` is **not encrypted** — it's plaintext in that device's browser
  profile. It's as private as the device. Don't use it on a shared computer account.
- Loading live data reveals the user's **IP address** to those public sources,
  exactly like visiting any website. No personal data is ever sent. (The
  Tasks/Habits tabs make no network requests at all.)
- GitHub Pages keeps normal **web-server logs** (IP + file requested), like any host.

Back up or move your data anytime from **Settings → Backup / Restore** (a JSON file).

---

## 🧪 Safety note

Vulnerability data is public information, and Cadence frames everything toward
**understanding and defense**. But **proof-of-concept code is unvetted and can be
harmful** — read it before running, and only run it inside an isolated lab
environment you own and have permission to test.

---

## 🛠️ Customizing

Everything is in `index.html`. Near the top of the script you'll find:
- `SRC` — the data-source endpoints
- `FRESH` — cache lifetimes per feed
- `AIQ` — the search terms used for the AI tab

> If you edit the inline script, the CSP's `script-src 'sha256-…'` hash must be
> recomputed to match, or the script won't run. While developing you can
> temporarily replace the hash with `'unsafe-inline'`, then restore a pinned hash
> for production.

## ⚙️ Tech

Vanilla HTML, CSS and JavaScript. No frameworks, no bundler, no dependencies —
one self-contained file (~100 KB). Works light/dark, phone to desktop, and
installs as a PWA.

## 📄 License

[MIT](LICENSE) — free to use, modify and share. Built by Janak Bhati.
