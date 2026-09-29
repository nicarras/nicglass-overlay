<!--
  Public-facing README. This file is the SOURCE for the public repo's README.md.

  MODEL (2026-09-19, binary-only): the public repo `nicglass-overlay` carries only
  this README + GitHub Releases (prebuilt installers); no source is published.
  Issues are ENABLED (owner, 2026-09-26) so players can report bugs publicly; the
  owner folds them into the private repo's tracker as needed. This file is version-controlled here in the private repo and
  carried to the public repo by the release automation (#538) — there is no
  publish/scrub source-mirror pipeline any more (Track B retired). The private repo's
  own dev readme is the repo-root README.md; edit that one for private/dev notes,
  this one for what the public sees.

  Security reports go through GitHub private vulnerability reporting or a Discord DM
  (testers are invited by hand) — no public invite link is published here by design.
  The public repo's .github/ISSUE_TEMPLATE/ forms live only in that repo.
-->

# NicGlass Overlay

A transparent, always-on-top overlay for **Star Citizen** that reads the game's own
`Game.log` and turns it into at-a-glance, MobiGlass-styled widgets — missions,
cargo, trading, salvage, deliveries and more — laid over the game while you play.

It is **read-only by construction**: it only reads the log files the game already
writes to disk. It never injects code, hooks the game process, reads game memory, or
automates input. See **[License & compliance](#license--compliance)** below.

> A personal, fan-made project. Not affiliated with Cloud Imperium Games.

## What it shows

Widgets, grouped into panels you summon with hotkeys:

- **Missions** — every active mission, objective, reward and reputation change,
  visible without opening the MobiGlass.
- **Deliveries** — mission cargo legs grouped by destination, with SCU per leg.
- **Trading** — freeform buy-low / sell-high commodity runs with running P&L and a
  route advisor.
- **Salvage** — a beam coach: claim, wreck, actively-salvaging state and rate.
- **Session log** — session, travel, economy, mining and death events in one
  timeline.
- **Journal** — a persistent, timestamped scratchpad.
- **Blueprints** — crafting-pool completion with a manual owned-toggle.
- **Infographics** — hotkey-summoned reference images.
- **Settings** — hotkeys, appearance, and the `Game.log` path.

The widget set adapts to your ship's career (industrial → salvage, transport →
hauling). There is no login or profile setup for the overlay itself — it reads your
handle out of the log.

## Requirements

- **Windows** with **Star Citizen** installed — the overlay shell lays a window over
  the running game, so it is Windows-only.

## Install and run

1. Download the latest installer (`NicGlass-Overlay-Setup-<version>.exe`) from the
   [Releases](../../releases) page.
2. Run it. The builds are **unsigned**, so Windows SmartScreen may warn about an
   "unknown publisher" — choose *More info → Run anyway*.
3. Launch Star Citizen. The overlay follows the game's `Game.log` at the default
   install path automatically; if your install lives elsewhere, set the path in the
   overlay's **Settings** panel.
4. Use the hotkeys to summon and dismiss widget panels over the game. Hotkeys and
   appearance are configurable in Settings.

The window is transparent, always-on-top and click-through, so it sits over the game
without stealing input.

## How it works

Star Citizen continuously writes a `Game.log`. The overlay follows that file, parses
each record into typed events (missions, travel, economy, salvage, …), and streams
them to the widget pages — plain localhost web pages rendered inside a transparent
Electron window. Nothing is read from the game other than the log file it already
writes.

## Cloud sync (optional)

Every buy and sell the overlay sees becomes a commodity price datapoint. These are
kept locally and fed into the trade advisor whether or not you do anything else.
**Optionally**, you can link an account (Settings → Connections) to back up your
price observations and share them with a community trade database — gated by two
independent, opt-in consents (contribute yours / use others'). Your personal play
data — journal, ledger, session history — stays on your machine. Price sync never
uploads it; the only way your logs leave the machine is the separate **Share
diagnostic logs** setting below.

## Diagnostic logs (optional)

With an account linked, the overlay can send your log files to the developer so bugs
can be found without you having to hunt for files or describe a session. This is a
separate setting in Settings → Connections, independent of price sharing.

- **Default:** **off** in public releases — nothing is sent unless you turn on
  **Share diagnostic logs**. Beta builds (a `-beta` version) start with it on, and
  show a notice the first time you link an account that says what is sent and lets
  you turn it off with one click. Nothing is sent before that notice is dismissed,
  and your own choice always wins. Nothing is ever sent without a linked account.
- **When:** in the background shortly after the overlay starts, and periodically after
  that. Each file is sent once. **Send logs now** sends the same files plus the
  session in progress, and shows an upload ID you can paste into a bug report.
- **What is sent:**
  - your Star Citizen `Game.log` sessions from the last 14 days — your handle and RSI
    account ID, the handles of players near you, locations, play times, and your
    PC's hardware and Windows version;
  - the overlay's own log, including the readings it made from your screen (prices,
    mission titles, reputation).
- **Removed before sending:** your PC's name, its network addresses, your Windows
  user name and home folder, and any screen text the overlay read but could not
  recognise (it could be chat or other players' names). Screenshots and OCR
  captures are never sent. Your own copy of the log on your PC keeps everything.
- **Where it goes and for how long:** a private storage bucket (Amazon S3, US East)
  that only the developer can read. Uploads are deleted from it automatically **90
  days** after upload. The developer may keep a local copy of a session while
  working on the bug it shows, and deletes it when that bug is closed.
- **Why:** to reproduce and fix bugs in the overlay. The logs are not shared,
  published or used for anything else.

Turning the setting off stops future uploads. To have files that were already sent
deleted sooner, send the developer your upload ID — a Discord direct message if you
are a tester, or a GitHub issue that gives only the upload ID, not your handle.

## Configuring OCR (optional)

A handful of useful numbers live only on your screen, never in the log — your cargo
hold's SCU, a commodity kiosk's loading time and fee, live commodity prices, the
salvage depot buffer, and your reputation tier. The overlay can read these with
**optional OCR**: screenshot-style capture of the Star Citizen window, only while that
window is focused, **off by default**. Turn it on in **Settings → OCR**.

Because the on-screen position of each readout depends on your resolution and HUD,
**every OCR reading is calibrated once** — you show the overlay where on *your* screen
each value sits. Until a reading is calibrated **and validated**, it stays off and
shows nothing.

**To calibrate a reading:**

1. With the game running, press **`End`** to enter **calibration mode**. Labeled boxes
   appear over the game — one per reading — each with **Preview**, **Validate**,
   **Reset** and a status badge. (Calibration mode lets the overlay take mouse input;
   press `End` again to exit and restore click-through.)
2. Open the in-game screen that shows the reading (see the table), and drag/resize that
   reading's box over the value.
3. Click **Preview** to confirm the box is reading it, then **Validate** to lock it in.
   An unvalidated box produces nothing; moving a validated box returns it to "needs
   validation" until you re-validate.
4. Press `End` to exit. Box positions are saved and persist across restarts.

**Where each reading lives, and what the box should enclose.** Draw each box so it
**fully contains** the exact text named below, with a little margin — if you clip the
value it won't read. A slightly generous box is fine (nearby HUD text like fuel gauges
or status lines is ignored); just avoid making it so wide it also catches a *second*,
similar-looking readout.

| Reading | Open this screen | Draw the box to enclose |
|---|---|---|
| **Cargo hold** | Your ship's cargo-hold / vehicle-inventory **manifest**, with cargo aboard | the hold's `current / max SCU` total line (e.g. `120 / 696 SCU`) — the manifest's SCU figure, **not** the kiosk's local-storage item list |
| **Cargo kiosk** (loading time + fee) | At a commodity terminal, pick a commodity → **Buy** to open the **Confirmation** dialog (Cancel without buying) | both the `EST. LOADING TIME  hh:mm:ss` line **and** the `CARGO AUTO-LOAD: ¤N` line — enclose them together (either one alone is enough to validate) |
| **Commodity kiosk** (prices) | A commodity terminal's Buy / Local Market Value grid | the commodity **price rows** — enclose the name + price columns for the list of commodities (the grid body, not the header or filter controls) |
| **Salvage buffer** | In a salvage ship (e.g. a Vulture), the salvage HUD | the word `DEPOT` **and** its `x / y SCU` value **together** (including `FILLER STATION READY` just below is fine); an empty `0.0 / 13.0 SCU` reads fine. Reads best when that text sits over a darker background |
| **Reputation** | The MobiGlass **Reputation** panel, on an org with your current tier tile highlighted | the **row of rank tiles**, so your currently-**highlighted** rank tile is fully inside with a little margin — the reader picks the highlighted one out of the tiles in the box |

OCR only runs while Star Citizen is the focused window, and each reading refreshes on
its own short interval. If a reading stays blank after validating, re-check that the
box covers the value and that the right screen is open.

A couple of readouts — **cargo hold** and **salvage buffer** — sit in different places
on different ships, and each reading remembers a single box, not one per ship. If you
fly more than one cargo or salvage hull, re-check (and if needed re-validate) those
boxes after switching ships. A slightly generous box is fine: extra HUD text around
the value (fuel gauges, status lines) doesn't interfere with the reading.

## Performance impact

We measure the overlay's cost to the game with PresentMon, watching Star Citizen's own
frames. Each run is 90 seconds in the same hangar scene, and the three setups are
compared within a single run. Test PC: RTX 4080 Super at 3840×1600 (measured
2026-09-28).

| Setup | Avg FPS vs. no overlay | 1% low vs. no overlay | Frames over 100 ms |
|---|---|---|---|
| Overlay off | — (70.9 fps) | — (34.1 fps) | 0 |
| Overlay on, OCR off | **−4.5%** (67.7) | −1.5% (33.6) | 0 |
| Overlay on, OCR on | **−7.3%** (65.7) | −11.4% (30.2) | 0 |

- The game stays in hardware "independent flip" presentation with the overlay open, so
  the transparent window doesn't force slower desktop composition.
- OCR reads the screen every few seconds, one reading at a time, on a single background
  process. It adds about 2% of total CPU. OCR doesn't cause periodic hitches: the
  longest frame in the OCR run was 69 ms, against 60 ms with no overlay.
- These figures come from one machine. The 1% low varies by a few percent between runs
  on its own. A slower CPU will likely feel OCR more. If you notice stutter, turn OCR
  off in **Settings → OCR** (takes effect after an overlay restart) and please report it.

## Bug reports and questions

Found a bug or have an idea? **[Open an issue](../../issues/new/choose)** — there are
forms for bug reports and feature requests. Testers on the community Discord can keep
reporting there too.

Please **don't attach raw `Game.log` files** to an issue. Issues are public, and the
log carries your handle and session detail. Paste only the few lines around the
problem, and blank out your handle if you'd rather keep it private.

Suspected **security** issues should not go in a public issue — see
[SECURITY.md](SECURITY.md).

## License & compliance

**License: proprietary end-user license — see [LICENSE](LICENSE).** NicGlass
Overlay is free to use for your personal use. Redistribution, resale, and reverse-
engineering of the application are not permitted (with the standard carve-outs for the
bundled open-source components below and for rights the law does not let a license
exclude). This is a closed-collaboration project: the source is not published.

**Third-party components.** The build bundles third-party software under its own
licenses, with the required notices in **[THIRD-PARTY-NOTICES](THIRD-PARTY-NOTICES)**:
the low-level input hook uses `libuiohook` (LGPL-3.0-or-later) via `uiohook-napi`
(MIT), shipped as a separately-loadable native library per the LGPL's shared-library
terms; optional cloud sync uses the AWS SDK / Smithy packages (Apache-2.0); and the
UI typefaces (Chakra Petch, JetBrains Mono) are under the SIL Open Font License 1.1.

**EAC-safe by construction.** The overlay only *reads* the log files the game writes
to disk. It never injects code, hooks the process, or reads game memory, and it does
not automate input. Optional OCR (off by default) is screenshot-style capture of the
Star Citizen window only, while that window is focused — the same thing a person
reading their own screen does.

**Not affiliated with Cloud Imperium Games.** Star Citizen and all related marks are
the property of CIG / Roberts Space Industries. This is an unofficial, fan-made
personal tool with no endorsement or affiliation. Use it in accordance with CIG's
Terms of Service and EULA; you are responsible for how you use it.

**No warranty.** The software is provided "as is", without warranty of any kind and
without any guarantee that it is compatible with, or permitted by, any third-party
service including Star Citizen. You use it at your own risk. See the
[LICENSE](LICENSE) for the full disclaimer and limitation of liability.
