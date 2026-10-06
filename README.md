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
<img src="screenshots/habits.png" width="31%" alt="Habits & streaks" />

</div>

---

## What it is

Cadence is a single static web page that helps you build a daily security habit.
It combines a to-do list and habit tracker with a **"read something real every
day"** routine: one *known-exploited* CVE and two proof-of-concepts to study,
drawn live from authoritative public sources. Everything you create stays on
your own device.

It's built for learners — a small daily dose of real-world vulnerabilities to
understand and defend against.

## Features

| Tab | What it does |
|-----|--------------|
| **Today** | Greeting with your name, a progress ring, habits due, tasks due, and your reading streak. |
| **Tasks** | To-do list with due dates, priority, filters (Today / Upcoming / All / Done) and search. |
| **Habits** | Daily or weekday habits with current & best streaks, a 7-day row, and a 3-month heatmap. |
| **Learn** | **1 known-exploited CVE + 2 proof-of-concepts each day**, each with a "how to study this" guide and links to NVD, CVE.org and GitHub. A **live search** sits at the top of the tab, across the CVE catalog and GitHub PoC repos. |

The daily picks are **deterministic per day** — everyone gets a stable set that
rotates at midnight (there's a shuffle button for a fresh draw). Mark all three
as read to keep your streak alive.

---

## 📡 Data sources — one genuine source per task

All free, public, and **no API key required**.

| Purpose | Source | Why this one |
|---------|--------|--------------|
| **CVEs** | [CISA Known Exploited Vulnerabilities](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) (official U.S. gov catalog, via its GitHub mirror) | The authoritative list of vulnerabilities **actively exploited in the wild** — the highest-signal place to start. |
| **Proof-of-concept** | [GitHub](https://github.com/search?q=CVE&type=repositories) code search | The largest public index of researcher PoCs and write-ups. |

The **Tasks** and **Habits** tabs work fully offline. The **Learn** tab loads
live data, so it runs from a hosted `https://` page (browsers block those
cross-site requests from a local `file://`).

---

## 🔒 Privacy & security

This is a security tool, so the model is spelled out in full.

**There is no server, so there is no central database to breach.** Every user's
data lives only in *their own browser*, on *their own device*. Nothing is sent
anywhere, and there's nothing to leak.

**Built into the page:**
- **Strict Content-Security-Policy** — `default-src 'none'` with an explicit
  `connect-src` allowlist. The page can contact **only** these three hosts and
  nothing else: `raw.githubusercontent.com`, `cdn.jsdelivr.net`,
  `api.github.com`.
- **No third-party JavaScript.** All script is inline and pinned by **SHA-256
  hash** in the CSP — no external libraries, nothing to hijack via a CDN.
- **All remote text is HTML-escaped** before display (no stored-XSS from a feed),
  and **outbound links are scheme-checked** (`http(s)` only).
- **`referrer` disabled** and `rel="noopener noreferrer"` on every external link.
- **No cookies, no accounts, no analytics.**
- **Offline-tolerant:** the last successful fetch of each feed is cached, so
  today's reading still opens without a connection.

**Honest residual notes (worth knowing, not weaknesses in the code):**
- `localStorage` is **not encrypted** — it's plaintext in that device's browser
  profile. It's as private as the device. Avoid shared-computer accounts.
- Loading live data reveals the user's **IP address** to those public sources,
  exactly like visiting any website. No personal data is ever sent. (The
  Tasks/Habits tabs make no network requests at all.)

Your data can be backed up or moved anytime from **Settings → Backup / Restore**
(a JSON file).

---

## 🧪 Safety note

Vulnerability data is public information, and Cadence frames everything toward
**understanding and defense**. But **proof-of-concept code is unvetted and can be
harmful** — read it before running, and only run it inside an isolated lab
environment you own and have permission to test.

---

## ⚙️ Tech

Vanilla HTML, CSS and JavaScript. No frameworks, no bundler, no dependencies —
one self-contained file (~100 KB). Works light/dark, phone to desktop, and
installs as a PWA.

## 📄 License

[MIT](LICENSE) — free to use, modify and share. Built by Janak Bhati.
