<!--
  Public-facing README. This file is the SOURCE for the public repo's README.md.

  MODEL (2026-09-19, binary-only): the public repo `nicglass-overlay` carries only
  this README + GitHub Releases (prebuilt installers); no source is published.
  Issues are ENABLED (owner, 2026-09-26) so players can report bugs publicly; the
  owner folds them into the private repo's tracker as needed. This file is
  version-controlled here in the private repo and
  carried to the public repo by the release automation (#538) — there is no
  publish/scrub source-mirror pipeline any more (Track B retired). The private repo's
  own dev readme is the repo-root README.md; edit that one for private/dev notes,
  this one for what the public sees.

  Security reports go through GitHub private vulnerability reporting or a Discord DM
  (testers are invited by hand) — no public invite link is published here by design.
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
data — journal, ledger, session history — always stays on your machine and is never
uploaded.

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
