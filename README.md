# Bags — Professional Trade Journal

A feature-rich desktop trade journal with a dark neon aesthetic, 3D market visualizations, multi-account analytics, and a full gamification engine. Your trade data is stored locally on your machine (see [Your Data](#your-data)).

**Current version:** `v1.0.0` · **Platforms:** Windows · macOS · Linux · **License:** MIT

---

## Download

Bags is distributed from GitHub Releases. No sign-up, no installer, no telemetry.

### Step-by-step (Windows, the portable build)

1. Open the [**Releases page**](https://github.com/localhost9001/Bags/releases/latest).
2. Under **Assets** on the latest release, click `Bags.exe` to download it. Your browser may warn "this file is not commonly downloaded" — that's expected for unsigned builds. Choose **Keep**.
3. Move `Bags.exe` somewhere permanent (e.g. `C:\Tools\Bags\Bags.exe` or your Desktop). The file is portable — the folder you put it in is where future auto-updates will swap the binary.
4. Double-click `Bags.exe`. On first launch Windows SmartScreen will show a blue **"Windows protected your PC"** screen. Click **More info → Run anyway**. Future launches won't show the warning.
5. Bags opens straight into the app — there's no install wizard, no admin prompt, no account creation. Your trade data is stored locally (see [Your Data](#your-data)).

### Downloads table

| Platform | File | How to run |
|----------|------|------------|
| Windows 10 / 11 (x64) | `Bags.exe` | Double-click. Single portable file — no install needed. |
| macOS (Intel / Apple Silicon) | `Bags-<version>.dmg` | Open the DMG and drag Bags to Applications. |
| Linux (x64) | `Bags-<version>.AppImage` | `chmod +x Bags-*.AppImage && ./Bags-*.AppImage` |

### First-run notes

- **Windows SmartScreen**: The `.exe` is unsigned, so Windows will show a blue "Windows protected your PC" screen on first launch. Click **More info → Run anyway**. Future launches won't show the warning.
- **macOS Gatekeeper**: If macOS blocks the app, right-click Bags in Applications → **Open** → **Open** again. Only needed once.
- **No admin rights required.** Bags runs as a normal user.
- **No account required.** Bags is fully local — there's no login, no cloud sync, no telemetry.
- **Network access:** Bags only makes outbound network connections for auto-updates (fetching `version.json` from the public GitHub repo and downloading new releases). Your trade journal never leaves your machine.

### Auto-updates (Windows)

Bags checks for updates on launch (3 seconds after startup) and every 4 hours while it's running.

When a newer release is published on GitHub, the updater:

1. Fetches `version.json` from the public `localhost9001/Bags` repo (the manifest that pins the current latest version, download URL, and SHA256).
2. Compares it to your running version via semver. If the manifest is the same or older, nothing happens.
3. Downloads the new `Bags.exe` alongside the running one (as `Bags.new.exe`), with a 500 MB safety cap and a download timeout.
4. Verifies the file size and SHA256 against the manifest. If either check fails, the download is discarded and you're told why.
5. Renames the running `Bags.exe` to `Bags.old.exe` and moves the verified new build into place. If the swap fails for any reason, the original `Bags.exe` is automatically restored so you're never left in a bricked state.
6. Spawns the new build detached and quits the current one. On next launch the leftover `Bags.old.exe` is cleaned up.

Your trade data lives in a separate user-data folder (see [Your Data](#your-data)) and is never touched by the updater. You can also trigger a check manually from Settings → About → "Check for updates".

macOS and Linux clients don't auto-install — you'll be notified a new version is available in the manifest, but download and replace the app manually from the Releases page.

---

## Features

### Core Pages

- **Dashboard** — P&L overview, animated neon-glow equity curve (line + candlestick toggle with 1D/1W/1M timeframes), win/loss donut, day-of-week chart, recent trades, stat cards, gradient greeting text. Fully draggable/reorderable layout.
- **Trade Log** — Full CRUD with filters, sorting, multi-select bulk actions, split-pane chart view, CSV import/export with duplicate detection preview, contextual right-click menus, canvas minimap, virtual scrolling for 100+ trades.
- **Analytics** — P&L by symbol, hour, day of week, setup performance table, cumulative equity curve (lazy-loaded charts via IntersectionObserver).
- **Calendar** — Monthly P&L grid with green/red day coloring. Animated rotating glow ring on the current day.
- **Progress** — Gamification with 55 badges across 8 categories (volume, streak, profit, edge, discipline, special, degen, prestige) including the "WTF" badge for $1,000+ in under 2 seconds and degen badges like "Certified Degen", "Revenge Trader", and "Account Funeral". 11 progression levels from Rookie to Legend. Trading streaks and milestone tracking.
- **Calculator** — Position sizing, risk-reward meter, daily loss enforcement.
- **Playbook** — Trading strategy cards with entry/exit/risk documentation. Ships with 3 sample strategies: VWAP Reclaim Long, Gap & Go Momentum, and a Gambling/Revenge Trade anti-pattern tracker for quantifying the cost of emotional trading.
- **All Accounts** — Multi-account aggregated view with combined neon-glow equity curve, per-account stat cards with color-coded borders showing status badges and account numbers, 15 overall stat cards (gross P&L, commissions, wins/losses, profit factor, avg trade, best/worst day, trading days with green/red split, best/worst account), and a merged trade table spanning every account.
- **Simulations** — 3D simulation lab with Account Sims / Market Sims mode toggle. Account sims include Strategy DNA Helix, Trade Constellation, Heat Terrain, Trade Bias Compass, Trade Spiral Galaxy, Bootstrap Confidence Tubes, Trade Battlefield Map, Neural Network Decision Viz, Market Gravitational Lensing, and Probability Cloud. Full camera controls: left-drag orbit, right-drag axis pan, WASD movement (hold Shift for speed boost), scroll zoom. Favorites system with star buttons and last-used ordering, drag-and-drop card reordering, resizable viewport, and pop-out to new window.
- **Settings** — Tabbed layout: Preferences (timezone, date/time format, defaults, privacy blur, auto-backup — all apply instantly without a save button), Accounts (portfolios with status/number, blown accounts graveyard), Appearance (accent colors, star-field), Video/Sim (quality presets, GPU detection, anti-aliasing, shadows, particles, bloom, SSAO, ray tracing), Data (statistics, backups, danger zone), About (Run on Startup, System Tray, welcome tour, version info). Auto-detects GPU hardware and disables incompatible graphics settings.

### Visual & Animation

- **Animated SVG Bull Splash Screen** — Custom animated bull silhouette with market chart line animation, loading bar, and logo reveal on app startup.
- **Neon-Glow Equity Curve** — 3-pass rendering (outer bloom, inner glow, sharp core) with breathing animation and a traveling pulse that flows toward the current price.
- **Equity Curve Toggle** — Switch between animated line chart and daily candlestick view. Line mode colors each segment dynamically (green rising, red negative). Candlestick mode supports 1D, 1W, and 1M timeframes.
- **Live End Dot** — Constant pulse animation on the equity curve endpoint. Color reflects today's P&L: green for profit, red for loss, animated white for no trades.
- **Animated Star-Field Background** — Subtle parallax star particles drifting across the entire app background. Stars twinkle and glow with the accent color. Configurable density slider. Toggle in Settings.
- **Animated Page Transitions** — Smooth fade+slide animations between pages. Every page navigation scrolls to top.
- **3D Equity Mountain** — Press `Ctrl+G` to launch a fullscreen Three.js terrain visualization. Daily trades are aggregated into a living mountain landscape: green peaks form during winning streaks, red valleys carve during drawdowns, and purple tints color the midrange. The terrain breathes with sinusoidal wave animation, glowing particles float upward from peaks, day markers bob gently, and an animated ridge line draws in the cumulative P&L path. Three orbiting colored lights sweep across the surface. Click-drag to orbit, scroll to zoom.
- **3D Strategy DNA Helix** — Double helix where each trade is a node on a green (win) or red (loss) strand. Node size is scaled by P&L magnitude. Cross-rungs connect the strands every 3 trades. Click-to-inspect orb info, fly-to-next navigation, keyboard shortcuts.
- **3D Trade Constellation** — Star map grouping trades by symbol into ring-distributed clusters with collision avoidance. Lines rotate with orbs. Canvas-texture sprite labels. 600 ambient dust particles.
- **3D Heat Terrain** — Day-of-week × hour-of-day terrain map with a neon matrix wire grid aesthetic. Dimmed solid terrain beneath bright dual-layer neon wireframe (cyan + purple accent glow). Neon lime-green spike tips on profit peaks, bright neon-red valleys for losses. Tron-style glow.
- **3D Trade Bias Compass** — Pyramid with three faces representing Long, Short, and Neutral biases. Trade nodes scatter across faces using barycentric coordinates, colored green (win) or red (loss) with size scaled by P&L magnitude.
- **3D Trade Spiral Galaxy** — Trades spiral outward from a glowing central core in two arms. Time maps to angle (3 full rotations over trade history), P&L maps to height, magnitude scales node size.
- **3D Bootstrap Confidence Tubes** — Resamples your trades with replacement to build statistical confidence bands around your equity curve. Shows 5th/25th/50th/75th/95th percentile ribbons, ghost resample lines, your actual equity path in bright green, and labeled percentile markers. Custom resample count input (10–1000, default 100).
- **3D Trade Battlefield Map** — 3D terrain battlefield where each trade is a unit on the field. Wins advance forward, losses fall back. Smoke particles, waving flags, and a front-line equity path.
- **3D Neural Network Decision Viz** — Fully dynamic network driven by your actual trade data. Input layer auto-spawns one node per traded symbol, hidden layers bucket by setup and by direction, output layer shows Win and Loss nodes with live counters plus a probability %. Connection weight/color reflects shared trades and win rate between adjacent layers. Click any node to open a stats panel.
- **3D Market Gravitational Lensing** — Trades warp spacetime like gravitational masses. Large winners create deep gravity wells, losses create repulsion fields. Light rays bend around your biggest trades.
- **3D Probability Cloud** — Trades visualized as quantum probability distributions inside a physics chamber with magnetic field lines. Win/loss nodes pulse and occasionally collide, emitting flash rings.
- **3D Economic Calendar Timeline** — Your equity curve overlaid with generated economic event markers (FOMC, NFP, CPI, GDP, PPI, Retail Sales). See how your P&L reacts around major market catalysts.
- **3D Portfolio Flight Path** — Animated toggle from Equity Mountain view. Cumulative P&L rendered as a 3D tube snaking through space with trade markers, trailing particle streams, and glow effects.
- **SVG Icon System** — 50+ custom inline SVG icons replacing all emojis, themed to match accent colors via CSS variables.
- **Animated Candlestick Logo** — Sidebar and titlebar logos use a 5-candle uptrend SVG with a smooth looping animation: the live candle retraces 50% at random intervals then continues upward.
- **Splash Screen Moon Candle** — The final candle in the splash chart rally blasts dramatically off-screen with a fast scale-up animation and intensifying neon glow.
- **Welcome Tour** — Interactive 13-step animated walkthrough with spotlight highlighting, progress dots, and Ctrl+G shortcut introduction. Runs on first launch; restartable from Settings > About.
- **Window State Persistence** — Remembers whether you last had the app fullscreen/maximized, along with window position and size.
- **System Tray** — Enabled by default. When enabled, closing Bags minimizes it to the system tray instead of quitting. Double-click the tray icon to restore. Tray context menu with Open Bags, Quick Trade, and Quit.
- **Run on Startup** — Enabled by default. Launches automatically when you log into your computer. On macOS, opens hidden in the tray if system tray is also enabled.
- **Global Quick Trade Hotkey (`Ctrl+Shift+T`)** — When system tray is enabled, press `Ctrl+Shift+T` from anywhere on your system to pop up a Quick Trade window. Log a trade without opening the full app. Every field from the main trade modal: date, time in/out, symbol, direction, quantity, entry/exit price, commissions, setup, star rating, tags, mistakes, and notes — plus a live P&L preview. Supports Calculate P&L and Manual P&L entry modes.

### Customization

- **Custom Theme Builder** — Pick any accent color with a color picker in Settings. HSL palette generation derives the full UI theme from a single color input.
- **10 Accent Color Presets** — 6 solid colors (Purple, Ocean Blue, Emerald, Rose, Gold, Cyan) and 4 gradient themes (Neon, Sunset, Ocean, Aurora). Changes apply instantly across the entire UI.
- **Account Color Coding** — Assign any of ~48 curated colors (organized by hue family) to each trading account for visual distinction throughout the app.
- **Account Status Badges** — Animated glowing toggle in account modals to set status as Eval, Funded, Live, or Practice. Status badge displayed throughout the app.
- **Account Number Tracking** — Optional account number field displayed in account cards, settings, and the aggregate view.
- **Privacy Blur Mode** — Toggle in Settings > Preferences to blur all account numbers app-wide. Click any blurred number to temporarily reveal it.
- **Customizable Dashboard Layout** — Drag and reorder dashboard widgets (stats, equity, charts, recent trades, streaks) into your preferred layout. Order persists across sessions.
- **Blown Account Tracker** — "Blew" button on each account card in Settings. Pops a modal asking if it was a Trump tweet or self-inflicted, then closes the account and archives it to a blown accounts graveyard with final P&L, trade count, and date.

### Trade Management

- **Trade Screenshots / Image Attachments** — Attach chart screenshots to individual trades. Images stored as Base64 with thumbnail preview in the trade modal.
- **Trade Log Minimap** — Canvas-rendered minimap alongside the trade log showing green/red trade distribution. Click to jump to any trade. DPR-aware for retina displays.
- **Virtual Scrolling** — Spacer-based virtualization for trade logs with 100+ rows. Only visible rows are rendered, with buffered off-screen rows for smooth scrolling.
- **Multi-Select Trades** — Checkboxes on trade rows with select-all. Bulk delete, export selected as CSV.
- **Split-Pane Chart View** — Inline TradingView candlestick chart beside the trade log.
- **Right-Click Context Menu** — Quick actions: View Chart, Edit, Duplicate, Copy Symbol, Delete.
- **Universal CSV Column Mapper** — Interactive UI for mapping any broker's CSV columns to Bags fields. 3-pass auto-detection (exact, word-boundary, long substring).
- **External ID Dedup** — CSV import detects broker-unique trade IDs (TopstepX, Tradovate, etc.) for perfect duplicate detection of multi-contract order fills.
- **CSV Import Preview** — Summary showing new trades, duplicates, and parse errors before confirming import.
- **Topstep-aware commissions import** — When the active account's prop firm is **Topstep** (or Topstep Forex), the CSV's `Commissions` column is summed with the `Fees` column because Topstep's export reports commissions as *round-turn* (both sides of the trade combined, effective 04/12/2026). For **any other prop firm** the `Commissions` column is intentionally ignored on import to avoid double-counting, because non-Topstep brokers don't guarantee round-turn semantics — map your broker's total per-trade fees into the `Fees` column instead.

### UI Chrome

- **Collapsible Sidebar** — Toggle to icon-only mode for more screen space. Remembers collapsed state across sessions.
- **Account Edit Modal** — Full modal dialog for creating and editing accounts with prop firm assignment from a searchable dropdown of 55+ firms, account number, animated status toggle, and color picker.
- **Searchable Prop Firm Dropdown** — Type-ahead dropdown (Topstep, Apex, FTMO, Lucid, etc.) for assigning accounts to their funding source.

### Security & Stability

- Content Security Policy (CSP) headers
- Electron sandbox mode enabled
- IPC input validation on all handlers (including array size limits on batch operations)
- Atomic file writes (write-to-temp-then-rename)
- Automatic backup recovery on corrupt data file
- XSS prevention via HTML escaping on all user-rendered content
- Navigation restricted to app origin
- Navigation restricted to app origin and window-open handler rejects external URLs

---

## Your Data

All data is stored locally as a JSON file:

| OS | Path |
|----|------|
| Windows | `%APPDATA%\bags\bags_data.json` |
| macOS | `~/Library/Application Support/bags/bags_data.json` |
| Linux | `~/.config/bags/bags_data.json` |

Backups are saved to the `backups/` subfolder. Up to 20 backups are retained automatically. Data persists across app updates — updates only replace the executable, never your journal.

Your trade records (trades, accounts, strategies, notes, screenshots) are **never** sent over the network by Bags. They live only in the JSON file above. The only network traffic Bags generates is the auto-updater, which fetches `version.json` from the public GitHub repo and downloads new releases when available.

---

## Trade Fields

Date, Time In/Out, Symbol, Direction, Entry/Exit Price, Qty, Fees, Setup, Rating, Tags, Mistakes, Notes, Images (Base64 attachments).

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+N` / `Cmd+N` | Add new trade |
| `Ctrl+G` / `Cmd+G` | Open 3D Equity Mountain |
| `Ctrl+Shift+T` | Quick Trade popup (global — works even when Bags is minimized/hidden, requires System Tray enabled) |
| `Esc` | Close any modal / split pane / context menu / 3D view / Quick Trade popup |
| Right-click on trade row | Context menu with quick actions |
| Click sidebar toggle | Collapse/expand sidebar |

---

## System Requirements

- **Windows**: Windows 10 or newer (x64)
- **macOS**: macOS 11 Big Sur or newer (Intel or Apple Silicon)
- **Linux**: any modern 64-bit distro with glibc 2.28+ (Ubuntu 20.04+, Fedora 32+, etc.)
- **GPU**: Any GPU from the last decade. Graphics settings auto-tune to your hardware. Software-rendered fallback is supported if no GPU is detected.
- **Disk**: ~200 MB for the app, plus a few MB for your journal.

---

## Uninstall

- **Windows (portable)**: Just delete `Bags.exe`. To remove your data too, delete `%APPDATA%\bags\`.
- **macOS**: Drag Bags from Applications to the Trash. Data lives in `~/Library/Application Support/bags/`.
- **Linux**: Delete the AppImage. Data lives in `~/.config/bags/`.

---

## License

MIT. See [LICENSE](LICENSE).

---

*Built for serious traders.*
