# agentfenster MCP server

The agentfenster MCP server lets Claude Code and other MCP clients drive real
Windows applications on a hidden second desktop inside your own session, with
the same logins, cookies and VPN, while you keep your screen, mouse and
keyboard. It is a local stdio server with 11 tools, from a one call task loop
down to single verified clicks.

Every action is verified against the window state afterwards. A click that
lands nowhere is reported as a failure, never as a silent success.

Full documentation: <https://agentfenster.com/mcp>

> The MCP server ships with the agentfenster app, a commercial closed-source
> Windows product. This repository is its public documentation and issue
> tracker. It contains no product source code.

## Install

1. Install agentfenster on Windows 10 or 11 from <https://agentfenster.com>.
   The MCP server is part of the app. Every plan starts with a 3-day trial, no
   card and no account required.
2. Register the server with Claude Code:

```bash
claude mcp add agentfenster -- agentfenster-mcp
```

Any MCP client that can launch a local stdio server works the same way. Then
just ask:

```
> open the character map on the agent desktop and turn on the advanced view
```

## The 11 tools

| Tool | What it does |
| --- | --- |
| `list_windows` | Lists the windows this machine can drive, with title, class, minimized state and whether a picture can be captured. The starting point when you do not know the exact window title. |
| `desktop_task` | Runs a full GUI task in one window and returns a single paragraph: what happened, whether it worked, and if not, which step failed and why. The click loop runs inside the server, so its retries never enter your context. |
| `inspect_window` | Returns the capped, filtered element tree of one window as a compact list, for planning clicks yourself. Capped at 200 elements and never walked from the desktop root. |
| `replay` | Lists recent runs, or returns one run's step-by-step log: every plan, every action, whether it changed anything, and a link to the visual replay in the app. |
| `open_app` | Launches a program on the shared agent desktop and reports its first titled window. The agent desktop starts empty, so this is how anything gets onto it. |
| `read_desktop` | Reads the compact element tree of a window on the agent desktop, one line per interactive element, no screenshot. Plan your clicks from this. |
| `click` | Clicks one element, chosen by id or name from `read_desktop`, then verifies against the desktop state and states explicitly whether anything changed. |
| `type_text` | Types text into one element on the agent desktop, then verifies whether anything changed. |
| `press_key` | Presses a key or key combination such as `ctrl+s`, `enter`, `tab` or `alt+f4`, optionally focusing an element first, with verification afterwards. |
| `scroll` | Scrolls a window or a specific pane and reports whether the content actually moved. A list that is already at the bottom reports no effect. |
| `agent_windows` | Lists the windows currently open on the shared agent desktop, frontmost first, so you can pick one by a title substring. |

## How it works

- The agent works on a second Win32 desktop inside your session. It never takes
  your foreground, mouse or keyboard, and you can watch it live in the app.
- Tools read the UI Automation element tree instead of screenshots: one line per
  interactive element, capped at 200 elements per window.
- The click loop of `desktop_task` runs inside the server on a model endpoint
  you configure, so its retries and intermediate steps never enter your context.
- Risky actions such as delete, send, pay or install are denied by default on
  MCP calls and reported, instead of blocking on a confirmation nobody can
  answer.
- agentfenster sends no telemetry. The only network traffic is the model
  endpoint you configure for the click loop.

## Requirements

| | |
| --- | --- |
| Platform | Windows 10 or 11 |
| Transport | stdio, local |
| Runtime | Python 3.11 or newer, installed with the app |
| Prerequisite | the agentfenster app (commercial, 3-day trial) |

## Links

- Product and download: <https://agentfenster.com>
- MCP documentation: <https://agentfenster.com/mcp>
- Pricing: <https://agentfenster.com/#pricing>
- Discord: <https://discord.gg/J9MfKP255Y>
- Support: <hello@agentfenster.com>
- Privacy: <https://agentfenster.com/privacy>
- Terms: <https://agentfenster.com/terms>

## Issues

Use the issue tracker of this repository for bug reports and questions about
the MCP server: tool behaviour, install problems, MCP client compatibility. For
anything about the app itself, Discord and <hello@agentfenster.com> are faster.

## Contributing

Corrections to the text in this repository are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

The documentation text in this repository is licensed under
[CC BY 4.0](LICENSE). You may share and adapt it, including commercially, as
long as you credit agentfenster and link to the license.

This does not cover the agentfenster app or the MCP server it ships with. They
are commercial closed-source software, no open-source license is granted for
them, and the name and logo are not licensed. Use of the app is governed by the
terms at <https://agentfenster.com/terms>.
