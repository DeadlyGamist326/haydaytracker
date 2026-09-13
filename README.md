[README.md](https://github.com/user-attachments/files/32154107/README.md)
# 🎮 Hayday Tracker

A daily/weekly checklist for tracking Hay Day tasks across multiple game accounts — with **live, multi-user sync** via Firebase. Built as a single self-contained HTML file (works standalone or hosted, e.g. on GitHub Pages).

---

## ✨ Features Overview

### Core tracking
- **12 game accounts**, each with its own set of checkable tasks.
- **15 tasks**, split into two groups:
  - 🕓 **Resets daily once 4 PM has passed** — most tasks reset automatically every day once the clock hits 4 PM (one task, *Greg Shop*, resets at 8 AM instead).
  - ♾️ **No specific time reset** — tasks with no fixed daily reset (Derby, Boat, Fishing, Town, Nurture).
- **Live sync (Firebase Realtime Database)** — any check/uncheck, by anyone, appears instantly on every connected device. No manual save/export needed.
- **Presence system** — see who else is online right now (clickable name pill lets you rename yourself).
- **Undo button** — reverts the last action (single check, drag-selected block, or a full "Reset All"), even across devices.

### Special task types
| Task | Behavior |
|---|---|
| **Event** | Number stepper, **0–29**. Reaches 29 → counts as completed. Manually decrementable back to 0. **Never auto-resets.** |
| **Nurture Event Missions** | Number stepper, **0–27**. Reaches 27 → completed. **Auto-resets to 0 every Monday at 8 AM** (independent of the daily 4PM/8AM reset cycle). |
| **Boat** | Cooldown timer — tap to start a **5 min 30 sec** countdown (per account, independent timers). Auto-becomes available again once the timer ends. |
| **Fishing Lures** | Two independent cooldown buttons in one card — left = **3 hours**, right = **20 hours**. Task counts as "done" only when *both* are active. |
| **Supercell Store Free Reward** | Clickable link → opens `store.supercell.com/hayday` (desktop table only). |

### Three ways to view it
1. **Phone View** — the real mobile experience. One account at a time, swipe left/right (or tap arrows) to switch accounts. Tasks are grouped into sized card rows (3-across, 2-across, full-width "wide" cards) matching their importance/size.
2. **PC Web (desktop table)** — the classic spreadsheet-style table: every account × every task, all visible at once. Includes:
   - Zebra-striped rows + full-row hover highlight.
   - **Click-and-drag rectangle selection** — drag across multiple cells to check/uncheck them all in one motion (always a clean rectangle, never a stray diagonal path).
   - Floating hover preview card (account name, big task photo, task name) when hovering any cell.
3. **Accounts Grouping** (desktop-only toggle) — shows several accounts side-by-side in the same mobile-card style, organized into 3 navigable groups:
   - **LOTUS Accounts** — Lotus, DeadlyGamist, Where's my Naia~, HotDudu
   - **NAIA Accounts** — Naia, Krzymlk, SexyBubu
   - **SUPPORT Accounts** — Support1/2/3, ExtraBarn1/2
   - Merged section headers span all columns; each account's column still collapses/reflows independently as tasks complete.
   - Click an account's name to **focus** it (dims all other columns).

### "Completed" auto-sorting (Phone View & Accounts Grouping)
When every task in a card-row is checked (or its cooldown/counter is satisfied), that row **animates down into a "✅ Completed" section** at the bottom. Unchecking anything inside it animates it back up to its original spot. Ordering is preserved top-to-bottom.

### Visual & UX details
- **Custom embedded fonts**: *SC SecretOrigins BB* (titles/headers) and *SC CCBackBeat* (body text/labels) — both baked into the file as base64, so nothing needs to load externally.
- **Splash screen** on open: floating Hay Day logo over a dimmed background, auto-fades after 3 seconds (or on first tap/click).
- **Urgent blink**: 4PM-reset tasks that are still unchecked between **12 PM–4 PM** blink red every 2 seconds as a reminder.
- Task photos throughout (small icon in the table, larger photo backgrounds in card views, dedicated "wide" images for full-width cards where set).

---

## 🔧 Tech Stack
- Single `index.html` file — no build step, no dependencies to install.
- **Firebase Realtime Database** (`firebase-app-compat.js` + `firebase-database-compat.js`, loaded from Google's CDN) for live sync and presence.
- Vanilla JavaScript + CSS (no frameworks).

## 🚀 Hosting / Deployment
This is deployed via **GitHub Pages**:
1. The repo's `index.html` *is* the whole site.
2. Any push to the `main` branch (via direct upload or edit) triggers an automatic rebuild, usually live within 1–2 minutes.
3. Check the **Actions** tab for deployment status (green check = live).

## 🔥 Firebase Setup (for reference)
The app connects to a Realtime Database using a `firebaseConfig` object embedded directly in the script (apiKey, databaseURL, etc.). Currently running in **test mode** — anyone with the link can read/write. Rules should be tightened for private use if the link is ever shared publicly.

## 📝 Data Model (Firebase)
```
games/
  checks/
    <taskId>::<accountId>              → true | <timestamp> | <number>
    <taskId>:short::<accountId>        → <timestamp>   (Fishing Lures, 3H side)
    <taskId>:long::<accountId>         → <timestamp>   (Fishing Lures, 20H side)
  lastResetDates/
    <resetHour>                       → "YYYY-MM-DD"  (daily reset tracking)
    weekly_<taskId>                   → "YYYY-MM-DD"  (weekly reset tracking)
presence/
  <deviceId>                          → { name, ts }
```

## 📌 Known Limitations
- Firebase is in test mode (no auth/security rules yet).
- "Boat" and "Fishing Lures" are simple checkboxes on the desktop table (no cooldown timer there) but full cooldown widgets in Phone View / Accounts Grouping — behavior can look inconsistent if the same task is used from both.
- No offline write queue — an action made while disconnected only applies once reconnected (uses last-known cached data in the meantime).

---

*Built iteratively, one feature request at a time. 🐔🚜*
