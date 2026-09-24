# Eval package: pkg-14

- source: zellij-org/zellij#5451
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: zellij-org/zellij (34963 stars, archived: no)
- description: A terminal workspace with batteries included.
- latest release: v0.44.3 (2026-05-13)
- bug reports: template asks for an issue description, a minimal reproduction, and other relevant information (zellij version, terminal, OS)
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### Multi-tab startup layout: one pane never renders and permanently ignores input (Windows) (#5451)

opened by amir-arad (CONTRIBUTOR) on 2026-08-07, state open, labels: none

Creating a session from a layout with 3 tabs leaves one terminal pane
permanently blank: no prompt, and it ignores all input. The spawned
shell process is alive and idle, so this is a pane setup/render failure
rather than a shell one. It is a race: the victim is never the first
tab, and which tab dies varies between runs, so a single passing run
does not disprove it.

The debug log for the affected tab stops dead after
`ApplyLayout: sending QueryTerminalSize to client 1`, with no further
log activity for the life of the session. Minimal reproduction: a KDL
layout with 3 tabs (tab-bar, a terminal pane with a distinct `cwd`,
status-bar each), `zellij -n <layout>`, wait, then
`action dump-screen -p terminal_N` for each pane; one comes back empty.
Measured: 3/3 failed via startup layout, 4/4 clean via `action new-tab`
after startup, 3/3 clean with `zsh -f` (no rc files). No recovery from
keystrokes, tab switch, window resize, or client reconnect.

Environment: zellij 0.44.3, Windows 10.0.26200.8973, Alacritty.

## Thread highlights (4 comments total)

- 2026-08-07 amir-arad (CONTRIBUTOR): full debug log from a fresh reproduction; terminal_2 blank and unresponsive, the log is 23 meaningful lines then 86 identical unhandled-event lines
- 2026-08-11 divens (CONTRIBUTOR): thanks for the report, will look into it
- 2026-08-14 s-annam (NONE): what looks like the same bug on macOS + Ghostty, on interactive `new-tab` in a long-running session, so probably not Windows-specific

## Candidate claim comment

Hello! I'd like to take this startup-layout race as my first zellij
contribution. I have reproduced the dead pane on my machine (evidence
below) and I'm ready to start bisecting the layout-apply path.

## Candidate repro report

I set up the reproduction environment and confirmed everything is in
place. Evidence of my setup:

```
$ zellij --version
zellij 0.44.3
$ zellij ls
layout-test [Created 2m ago]
```

I created the 3-tab layout file from the issue and started a session
with it, and the session launches with all three tabs showing in the
tab bar, so the layout is definitely being applied the way the issue
describes. The screenshot I took shows the zellij session running with
the three tabs visible.

Environment: zellij 0.44.3 (cargo install), Windows 11 (10.0.26100),
Windows Terminal 1.24.

Expected: one of the three panes comes up blank and ignores input.

Actual: the session starts and the tabs are all present, consistent
with the issue's setup. This confirms the bug report's scenario is
reproducible on my machine and I am ready to investigate the race.
