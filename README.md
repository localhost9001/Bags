# Bags — Professional Trade Journal

A feature-rich desktop trade journal with a dark neon aesthetic, 3D market visualizations, multi-account analytics, and a full gamification engine. Your trade data is stored locally on your machine (see [Your Data](#your-data)).

**Current version:** `v1.0.3` · **Platforms:** Windows · macOS · Linux · **License:** MIT

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
- **Network access:** Bags only makes outbound network connections for auto-updates (fetching `version.json` from the public GitHub repo and downloading new releases) and — if you explicitly configure it — Discord webhook posts to share trades or equity snapshots with your own server (see [Discord Integration](#discord-integration)). Your trade journal itself never leaves your machine.

### Auto-updates (Windows)

Bags checks for updates on launch (3 seconds after startup) and every 4 hours while it's running.

When a newer release is detected you see an **Update Available** modal with a clear **Update Now / Update Later** choice. Dismissing is remembered per-version so the same release won't re-prompt until you restart. Once you hit Update Now, a progress pill in the bottom-right takes over with an animated fill bar through download → verify → install → relaunch.

Under the hood, the updater:

1. Fetches `version.json` from the public `localhost9001/Bags` repo (the manifest that pins the current latest version, download URL, and SHA256).
2. Compares it to your running version via semver. If the manifest is the same or older, nothing happens.
3. Downloads the new `Bags.exe` alongside the running one (as `Bags.new.exe`), with a 500 MB safety cap and a download timeout.
4. Verifies the file size and SHA256 against the manifest. If either check fails, the download is discarded and you're told why.
5. Renames the running `Bags.exe` to `Bags.old.exe` and moves the verified new build into place. If the swap fails for any reason, the original `Bags.exe` is automatically restored so you're never left in a bricked state.
6. Tears down the system tray, spawns the new build detached, and quits the current one. On next launch the leftover `Bags.old.exe` is cleaned up.

After the relaunch, a **What's New** modal summarizes what changed in the new version — it only fires for returning users on a genuine version bump, and only once per release.

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
- **Settings** — Tabbed layout: Preferences (timezone, date/time format, defaults, privacy blur, auto-backup — all apply instantly without a save button), Accounts (portfolios with status/number, blown accounts graveyard), Appearance (accent colors, star-field), Video/Sim (quality presets, GPU detection, anti-aliasing, shadows, particles, bloom, SSAO, ray tracing), Discord (webhook URL, auto-send toggles, test button — see [Discord Integration](#discord-integration)), Data (statistics, backups, danger zone), About (Run on Startup, System Tray, welcome tour, version info). Auto-detects GPU hardware and disables incompatible graphics settings.

### Visual & Animation

- **Animated SVG Bull Splash Screen** — Custom animated bull silhouette with market chart line animation, loading bar, and logo reveal on app startup.
- **Neon-Glow Equity Curve** — 3-pass rendering (outer bloom, inner glow, sharp core) with breathing animation and a traveling pulse that flows toward the current price.
- **Equity Curve Animation Capture** — Click the camera button on the dashboard equity curve to record a 3/5/8/12-second MP4/WebM animation of your equity path drawing in with glow, pulse, and live-end-dot effects intact. The chart's draw-in animation automatically replays at the moment recording starts, so the saved video always captures motion instead of a frozen frame. Pick **Save to file** to drop it into any folder, or **Send to Discord** to pipe the clip straight into your configured webhook (see [Discord Integration](#discord-integration)). Recordings use `canvas.captureStream()` + `MediaRecorder` and fall back gracefully when hardware encoders are unavailable. Internally: the capture pipeline force-renders the canvas at full frame-rate for the duration of the clip (so clicking off the window mid-record no longer throttles to 5fps), uses `Blob` URLs for the preview + save paths (cheap, revoked on close), only pays the base64 cost at Discord-send time, and starts its duration timer from `MediaRecorder.onstart` so the encoder's warm-up doesn't eat the front of short clips.
- **Equity Curve Toggle** — Switch between animated line chart and daily candlestick view. Line mode colors each segment dynamically (green rising, red negative). Candlestick mode supports 1D, 1W, and 1M timeframes.
- **Trailing Liquidation Line** — When an account has a Max Loss Limit configured, both the line *and* candlestick equity curves paint a dashed yellow "marching-ants" rail that tracks the account's live trailing drawdown threshold. The line starts at `-(maxLoss + buffer)` and walks upward one step per new high-water mark until your HWM climbs past the buffer — at which point the rail locks permanently at your starting balance (0 on the P&L axis), mirroring Topstep-style trailing MLOL physics. An outer glow + crisp core pass keeps it legible against the green/red area fills, and the dashes animate in sync with the neon breath phase so the line pulses instead of looking static. A pulsing yellow **"LIQUIDATED"** label is anchored at the rightmost visible point of the rail on both curves — alpha + glow breathe in sync with the marching ants so the label feels like part of the same unit, and an auto-clamp keeps it from spilling across the Y-axis label column on tight timeframes. The aggregate *All Accounts* view intentionally skips the line — trailing drawdown is a per-account concept.
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
- **Animated Candlestick Logo** — Sidebar and titlebar logos use a 5-candle uptrend SVG with a smooth looping animation: the live candle retraces 50% at random intervals then continues upward. The sidebar replaces the static app name with a **live monospaced clock** (12-hour HH:MM:SS AM/PM) that ticks every second, pulled from the system's local time.
- **Splash Screen Moon Candle** — The final candle in the splash chart rally blasts dramatically off-screen with a fast scale-up animation and intensifying neon glow.
- **Welcome Tour** — Interactive 13-step animated walkthrough with spotlight highlighting, progress dots, and Ctrl+G shortcut introduction. Runs on first launch; restartable from Settings > About.
- **Window State Persistence** — Remembers whether you last had the app fullscreen/maximized, along with window position and size.
- **System Tray** — Enabled by default. When enabled, closing Bags minimizes it to the system tray instead of quitting. Double-click the tray icon to restore. Tray context menu with Open Bags, Quick Trade, and Quit.
- **Run on Startup** — Enabled by default. Launches automatically when you log into your computer. On macOS, opens hidden in the tray if system tray is also enabled.
- **Global Quick Trade Hotkey (`Ctrl+Shift+T`)** — When system tray is enabled, press `Ctrl+Shift+T` from anywhere on your system to pop up a Quick Trade window. Log a trade without opening the full app. Every field from the main trade modal: date, time in/out, symbol, direction, quantity, entry/exit price, commissions, setup, star rating, tags, mistakes, and notes — plus a live P&L preview. Supports Calculate P&L and Manual P&L entry modes. Auto-calc is futures-aware — type `MNQ`, `ES`, `MCLH6`, `ZBZ5`, etc. and Bags applies the correct contract multiplier (MNQ=$2/pt, ES=$50/pt, MCL=$100/pt, …) so quantity × point-diff produces the real broker P&L instead of a stock-style tick sum.
- **Widget Screensaver** — When the app sits idle for a configurable number of minutes (default 5, 1–120 range), Bags fades in a fullscreen ambient widget overlay. Choose between four widgets: an **Animated Equity Curve** that redraws your cumulative P&L with breathing neon glow, a **Minimal Starfield** with parallax drift and twinkle, a **Live Clock + Next Session Countdown** showing time, date, and a live ticker counting down to the next U.S. market session (pre-market, open, close) in America/New_York time with green/red delta from today's P&L, or a **Trade Stats Ticker** scrolling today/weekly/monthly/all-time P&L, win rate, trade count, best/worst trades, and current streak. Configure everything from **Settings → Preferences → Screensaver**: enable/disable, pick a widget, set idle minutes, and fire a **Preview** button to see it immediately. Any mouse, keyboard, or touch activity dismisses the overlay, and the screensaver will never appear on top of an open modal.
- **Multi-Monitor Pop-Out Tabs** — Every page has a pop-out button in the top-right of its action bar. Click it to launch that tab in its own frameless window, ideal for spanning the app across multiple monitors. The pop-out window gets its own minimize/maximize/close controls, shares the same theme and accent color, and tears down cleanly on app quit. Pop out the Trade Log on one screen while keeping the Dashboard or 3D Simulations on another.

### Customization

- **Custom Theme Builder** — Pick any accent color with a color picker in Settings. HSL palette generation derives the full UI theme from a single color input.
- **10 Accent Color Presets** — 6 solid colors (Purple, Ocean Blue, Emerald, Rose, Gold, Cyan) and 4 gradient themes (Neon, Sunset, Ocean, Aurora). Changes apply instantly across the entire UI.
- **Account Color Coding** — Assign any of ~48 curated colors (organized by hue family) to each trading account for visual distinction throughout the app.
- **Account Status Badges** — Animated glowing toggle in account modals to set status as Eval, Funded, Live, or Practice. Status badge displayed throughout the app.
- **Account Number Tracking** — Optional account number field displayed in account cards, settings, and the aggregate view.
- **Per-Account Max Loss Limit + Liquidation Buffer** — The account create/edit modal now has a **Max Loss Limit** input (e.g. `2000` for a 50K eval) and a companion **Liquidation Buffer** input (default `100`) that captures the dollar cushion your prop firm allows past the stated drawdown before the account is auto-liquidated. New accounts default to `2000 / 100`; existing accounts are left blank so no limit is silently imposed. Negatives and non-numeric input are clamped to `0`. These values feed the [Trailing Liquidation Line](#visual--animation) overlay on both equity curves and use the Topstep trailing formula `threshold = min(HWM, maxLoss + buffer) - (maxLoss + buffer)` so the rail locks at your starting balance the moment your peak equity climbs past the cap. Set Max Loss to `0` (or leave blank) to turn the overlay off for that account.
- **Privacy Blur Mode** — Toggle in Settings > Preferences to blur all account numbers app-wide. Click any blurred number to temporarily reveal it.
- **Customizable Dashboard Layout** — Drag and reorder dashboard widgets (stats, equity, charts, recent trades, streaks) into your preferred layout. Order persists across sessions.
- **Blown Account Tracker** — "Blew" button on each account card in Settings. Pops a modal asking if it was a Trump tweet or self-inflicted, then closes the account and archives it to a blown accounts graveyard with final P&L, trade count, and date.

### Trade Management

- **Trade Screenshots / Image Attachments** — Attach chart screenshots to individual trades. Images stored as Base64 with thumbnail preview in the trade modal.
- **Clipboard Screenshot Paste** — Press `Ctrl+V` (or `Cmd+V`) anywhere inside the trade modal to auto-attach a screenshot from the clipboard. Pair it with your broker's chart tool or `Win+Shift+S` / `Cmd+Shift+4` for a one-shot capture → paste → save workflow. Supports PNG and JPEG; oversized images are automatically downscaled.
- **Trade Log Minimap** — Canvas-rendered minimap alongside the trade log showing green/red trade distribution. Click to jump to any trade. DPR-aware for retina displays.
- **Virtual Scrolling** — Spacer-based virtualization for trade logs with 100+ rows. Only visible rows are rendered, with buffered off-screen rows for smooth scrolling.
- **Multi-Select Trades** — Checkboxes on trade rows with select-all. Bulk delete, export selected as CSV.
- **Split-Pane Chart View** — Inline TradingView candlestick chart beside the trade log.
- **Right-Click Context Menu** — Quick actions: View Chart, Edit, Duplicate, Copy Symbol, Delete.
- **Universal CSV Column Mapper** — Interactive UI for mapping any broker's CSV columns to Bags fields. 3-pass auto-detection (exact, word-boundary, long substring).
- **External ID Dedup** — CSV import detects broker-unique trade IDs (TopstepX, Tradovate, etc.) for perfect duplicate detection of multi-contract order fills.
- **CSV Import Preview** — Summary showing new trades, duplicates, and parse errors before confirming import.
- **Topstep-aware commissions import** — When the active account's prop firm is **Topstep** (or Topstep Forex), the CSV's `Commissions` column is summed with the `Fees` column because Topstep's export reports commissions as *round-turn* (both sides of the trade combined, effective 04/12/2026). For **any other prop firm** the `Commissions` column is intentionally ignored on import to avoid double-counting, because non-Topstep brokers don't guarantee round-turn semantics — map your broker's total per-trade fees into the `Fees` column instead.
- **Manual vs Calculate P&L toggle** — The main Add Trade modal (and the `Ctrl+Shift+T` Quick Trade window) both have a toggle at the top of the form. **Calculate P&L** mode takes quantity + entry + exit + fees and computes gross/net live as you type. **Manual P&L** mode hides the quantity/entry/exit fields and lets you type the broker-reported net P&L directly — ideal for trades where the broker consolidates fills, applies weird fees, or reports fractional bond ticks that don't round-trip cleanly through decimal math. Reopening a manual-saved trade automatically puts the modal back into Manual mode with the net P&L prefilled.
- **Futures-aware auto-calculation** — When you're in Calculate mode and type a recognized futures symbol, Bags applies the correct CME point-value multiplier so `(exit - entry) × qty × mult` lands on the real dollar P&L instead of a stock-style raw tick sum. Recognized families include equity index minis/micros (ES/MES, NQ/MNQ, YM/MYM, RTY/M2K), energy (CL/MCL, NG/QG), metals (GC/MGC, SI/SIL), crypto (BTC/MBT, ETH/MET), ags (ZC, ZW, ZS), and treasuries (ZB, ZN, ZF). Symbols with CME month+year suffixes (e.g. `MESH5`, `MNQU26`, `CLZ25`) prefix-match to the base contract, so CSV-imported trades work without cleanup. Unknown symbols fall back to `mult=1`, preserving the original (`shares × price-diff`) formula for stocks and options.
- **Contract spec hint** — As you type a symbol in either trade modal, a small hint line appears underneath showing the matched contract: "Micro E-mini Nasdaq-100 · Micro · $2/pt", "WTI Crude Oil · Standard · $1000/pt", etc. Confirms at a glance that Bags resolved your ticker correctly.

### UI Chrome

- **Collapsible Sidebar** — Toggle to icon-only mode for more screen space. Remembers collapsed state across sessions.
- **Account Edit Modal** — Full modal dialog for creating and editing accounts with prop firm assignment from a searchable dropdown of 55+ firms, account number, animated status toggle, and color picker.
- **Searchable Prop Firm Dropdown** — Type-ahead dropdown (Topstep, Apex, FTMO, Lucid, etc.) for assigning accounts to their funding source.

### Discord Integration

Bags can post trades and equity snapshots to a Discord channel via an incoming webhook. Everything is opt-in, the webhook URL never leaves your machine except when Bags itself calls Discord, and the integration can be disabled at any time from **Settings → Discord**.

- **Webhook setup** — Paste your Discord webhook URL into **Settings → Discord**. Bags validates the URL against Discord's official hosts (`discord.com`, `discordapp.com`, `ptb.discord.com`, `canary.discord.com`) with a `/api/webhooks/...` path prefix, so unrelated URLs are rejected. A one-click **Test** button fires a sanity-check message so you can confirm the channel is live before wiring real trades into it.
- **Auto-send on trade save** — Optional toggle that DMs your webhook a formatted embed every time you add or edit a trade: symbol, direction, P&L, R-multiple, entry/exit, account, setup, tags, and attached screenshots. Images are uploaded as multipart attachments (Discord's 25 MB-per-file cap is respected, with downscaling on oversized files).
- **Auto-send on equity snapshot** — Optional toggle that posts a dashboard equity-curve screenshot with your daily/weekly/total P&L summary every time the dashboard refreshes after a new trade.
- **Batch send from the trade log** — Multi-select any subset of trades in the Trade Log and right-click → **Send to Discord** to queue them all. Sends are rate-limit-aware: Bags honors Discord's `429 Retry-After` headers and spaces out the queue so you won't get throttled.
- **Equity animation clips** — The **Send to Discord** option on the equity-curve recorder uploads the MP4/WebM clip directly to the channel with an optional caption.
- **Safety net** — All outgoing traffic goes through the main-process `_postDiscordWebhook` helper which re-validates the URL host on every call, strips non-HTTPS schemes, caps upload size, times out hung requests, and never surfaces raw error bodies into the UI (so a broken webhook can't leak your token into logs).

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

Backups are saved to the `backups/` subfolder. Up to 20 backups are retained automatically, with older ones pruned on each new backup. You can review backups and delete individual snapshots from **Settings → Data**. Data persists across app updates — updates only replace the executable, never your journal.

Your trade records (trades, accounts, strategies, notes, screenshots) are **never** sent over the network by Bags. They live only in the JSON file above. The only network traffic Bags generates is the auto-updater, which fetches `version.json` from the public GitHub repo and downloads new releases when available.

---

## Trade Fields

Date, Time In/Out, Symbol, Direction, Entry/Exit Price, Qty, Fees, Setup, Rating, Tags, Mistakes, Notes, Images (Base64 attachments).

### Trading-day rollover (4:30 PM Central Time)

Bags uses the prop-firm convention for "today." A new trading day starts at **4:30 PM Central Time** — the same convention used by Topstep, TopstepX, and most funded-account prop firms. Any trade with an entry time between **4:30 PM CT** and **4:30 PM CT the following calendar day** is counted toward that next trading day. Calendar tiles, daily P&L cards, and streak calculations all respect this rollover so your numbers line up with what your broker reports.

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+N` | Open the Add Trade modal |
| `Ctrl+V` | Paste a screenshot from the clipboard (inside the trade modal) |
| `Ctrl+I` | Import CSV |
| `Ctrl+E` | Export all trades as CSV |
| `Ctrl+G` | Launch 3D Equity Mountain |
| `Ctrl+F` | Focus the trade-log search box |
| `Ctrl+,` | Open Settings |
| `Ctrl+Shift+T` | Quick Trade popup (global — works even when Bags is minimized/hidden, requires System Tray enabled) |
| `Ctrl+Shift+D` | Toggle the sidebar collapsed / expanded |
| `Esc` | Close any modal / split pane / context menu / 3D view / Quick Trade popup |
| `Enter` | Submit the focused modal |
| `Ctrl+1` … `Ctrl+7` | Jump to Dashboard, Trade Log, Analytics, Calendar, Progress, Calculator, Playbook |

All shortcuts are handled in the renderer, so they only fire when a Bags window has focus — except `Ctrl+Shift+T`, which is registered as a global accelerator when System Tray is on.

---

## System Requirements

- **Windows:** Windows 10 (1809) or later, x64. ~200 MB free disk for the portable build + your data folder.
- **macOS:** macOS 11 Big Sur or later, Apple Silicon or Intel x64.
- **Linux:** 64-bit glibc-based distribution (Ubuntu 20.04+, Fedora 34+, Arch rolling). Tested against Electron 28's GTK 3 requirements.
- **GPU:** Any GPU that supports WebGL 2 / WebGPU for the 3D simulations. Bags auto-detects integrated/older GPUs and disables incompatible graphics settings (ray tracing, SSAO, bloom) by default — you can re-enable them from **Settings → Video/Sim** if you want to push your hardware.
- **RAM:** 4 GB minimum, 8 GB recommended for the 3D Simulations page with large trade histories.

---

## Uninstall

**Windows (portable):** Just delete `Bags.exe`. Your journal data (`bags_data.json`, backups, settings) stays in `%APPDATA%\bags\` until you delete it manually (so a reinstall picks up exactly where you left off). To wipe everything, delete `%APPDATA%\bags\` as well.

**macOS:** Drag `Bags.app` to the Trash. Your journal data lives in `~/Library/Application Support/bags/` and is not removed automatically; delete that folder if you want a clean slate.

**Linux:** Remove the AppImage / installed binary. Your journal data lives in `~/.config/bags/` and is not removed automatically; delete that folder if you want a clean slate.

---

## License

Bags is distributed as-is for personal trade-journaling use. No warranty expressed or implied — trade responsibly, and always double-check your own P&L.
