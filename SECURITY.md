# Security Policy

## Supported versions

This is a personal, fan-made project. Only the most recent release receives fixes;
there are no long-term support branches.

## Reporting a vulnerability

Please report security issues **privately** — not in a public issue.

Use GitHub's **[private vulnerability reporting](../../security/advisories/new)**
(the repository's *Security* tab → *Report a vulnerability*). Testers on the
community Discord can also send the maintainer a **direct message** instead (there
is no public invite link). Include what you found, how to reproduce it, and the
release tag affected. Please do not attach raw `Game.log` files — share only the
specific lines needed.

You'll get an acknowledgement as soon as the maintainer sees it. As a personal
project maintained in spare time, response times are best-effort.

## Scope

The overlay is a local desktop application. A few things worth knowing when assessing
its attack surface:

- It **reads** the log files Star Citizen writes to disk. It does not inject code,
  hook the game process, read game memory, or automate input.
- It serves the widget pages over a **localhost** HTTP/SSE server bound to the local
  machine.
- Optional cloud sync of market price observations authenticates via UCI OIDC →
  Amazon Cognito → short-lived STS credentials → DynamoDB. There are no long-lived
  cloud keys in the client; the one persistent secret (a UCI refresh token) is stored
  encrypted at rest, outside the state directory.

Personal play data (journal, ledger, session history) stays on the local machine by
design and is not uploaded.
