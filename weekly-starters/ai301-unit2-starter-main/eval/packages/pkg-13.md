# Eval package: pkg-13

- source: microsoft/terminal#20443
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: microsoft/terminal (104622 stars, archived: no)
- description: The new Windows Terminal and the original Windows console host, all in the same place!
- latest release: v1.24.11911.0 (2026-07-16)
- bug reports: template asks for the Windows Terminal version, the Windows build number, other relevant software, steps to reproduce, expected behavior, and actual behavior
- contribution policy (CONTRIBUTING.md): standard contribution guide with a CLA; no stated AI policy

## Issue

### Desktop-wide lag after long-running TUI sessions persists after Terminal restart; DWM restart required (#20443)

opened by Polymistis (NONE) on 2026-07-18, state open, labels: Issue-Bug, Needs-Triage, Needs-Attention

Windows Terminal 1.24.11911.0, 3840x2160 display at 120 Hz. Steps:
run prolonged TUI sessions (Codex CLI, Claude Code) that produce
frequent spinner, status, title, and streamed-text redraws; after
prolonged activity, window dragging, application switching, and desktop
selection become severely choppy. Stopping the TUI workloads and
reopening Terminal with one fresh window does not restore desktop
responsiveness; restarting `dwm.exe` restores it immediately. Exact
time-to-trigger is not deterministic. Default rendering settings (no
acrylic, transparency, background image, or custom shader).

Resource checks from the reporter: RAM above 50 GB free, near-zero
paging and disk, DWM handle counts comparable before and after, DWM CPU
about 104.6% on a one-logical-core scale before the restart and 7.7%
after. A related issue, microsoft/terminal#18422, reports similar lag
but recovers when Terminal closes; here only a DWM restart recovers.

## Thread highlights (14 comments total)

- 2026-07-19 JC-S (NONE): running into the exact same issue, also triggered by Codex CLI and Claude Code
- 2026-07-19 Polymistis (NONE): killing the DWM process (auto-restarts) restores performance completely
- 2026-07-20 Polymistis (NONE): DWM stays stuck regardless of TUI activity; closing all terminal windows has no effect, which is why the report notes Terminal restart does not recover

## Candidate claim comment

I want to help with this one, it has been driving me crazy for months!
I use Claude Code all day every day and my whole desktop turns into a
slideshow just like this issue says. Assigning myself to this, I know
exactly what the reporter is talking about.

## Candidate repro report

Can 100% confirm this bug. I run long Claude Code sessions on Windows
11 and after a few hours the entire desktop lags exactly as described:
dragging windows is choppy, alt-tab is choppy, everything is choppy. I
restarted Terminal and it was still lagging, so this is definitely the
DWM bug from this issue and not my machine. It happens every single
time I have a long session going, guaranteed reproducible on my end.
My machine is high end (RTX 4080, 64 GB RAM) so it is not a hardware
problem. This desperately needs a fix, the terminal is basically
unusable for serious TUI work right now. Happy to test any patch!
