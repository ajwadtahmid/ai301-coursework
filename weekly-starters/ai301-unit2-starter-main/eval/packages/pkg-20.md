# Eval package: pkg-20

- source: ghostty-org/ghostty#13604
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: ghostty-org/ghostty (59782 stars, archived: no)
- description: Ghostty is a fast, feature-rich, and cross-platform terminal emulator that uses platform-native UI and GPU acceleration.
- latest release: v1.3.1
- bug reports: template asks for the ghostty version, configuration, and platform details; first-time contributors go through a vouch flow before PRs are accepted
- contribution policy (CONTRIBUTING.md + AI_POLICY.md): strict AI rules. All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance; the human in the loop must fully understand the work; AI-assisted issues and comments must be reviewed and edited by a human before submission

## Issue

### Mode 2031 reports do not work unless both a dark and a light theme are configured (#13604)

opened by jcollie (MEMBER) on 2026-08-04, state open, labels: vt, gtk

With an isolated Ghostty instance launched with
`--config-default-files=false --window-theme=dark`, Ghostty's GTK log
correctly reports `style manager changed scheme=.dark`, but querying
the current color scheme with `CSI ? 996 n` returns `CSI ? 997 ; 2 n`,
which indicates light mode. With an actual conditional theme pair
configured (`theme = light:Rose Pine Dawn,dark:Rose Pine`) the same
query correctly returns `CSI ? 997 ; 1 n`.

The reporter narrows it to `Config.changeConditionalState`: the
function only rebuilds the configuration when the changed state key is
in `_conditional_set`, and a single non-conditional theme
(`theme = Kitty Default`) does not add `.theme` to that set, so the
scheme change is treated as irrelevant. The surface knows the scheme
is dark, but the config copied into Termio keeps its default light
state, and the mode-2031 response reads from there. The issue includes
a minimal draft patch to `src/config/Config.zig` treating the theme
state as relevant even when the theme is not conditional.

## Thread highlights (0 comments total)

(no comments)

## Candidate claim comment

Hi! I'd like to take this one as a first Ghostty contribution. I
reproduced the wrong mode-2031 report on the 1.3.1 release with a
single non-conditional theme (report below), so it is not specific to
debug builds. Next I want to test the draft patch in the issue against
both the single-theme and conditional-pair configurations and report
back before opening a PR.

## Candidate repro report

Environment: ghostty 1.3.1 (release build, Fedora 42 RPM), GTK
backend, GNOME 48 (Wayland), dark system scheme.

Steps:

1. Config file containing only `theme = Kitty Default`, then launched:
   `ghostty --config-default-files=false --config-file=./test-config
   --window-theme=dark`
2. In the new window, queried the color scheme:

```
$ printf '\033[?996n' ; sleep 0.2 ; cat -v
^[[?997;2n
```

3. Control: same launch with
   `theme = light:Rose Pine Dawn,dark:Rose Pine`, same query:

```
$ printf '\033[?996n' ; sleep 0.2 ; cat -v
^[[?997;1n
```

Expected: `CSI ? 997 ; 1 n` (dark) in both runs, since the window is
launched with `--window-theme=dark` and the GTK style manager reports
a dark scheme either way.

Actual: the single-theme run answers `997;2` (light, first capture),
the conditional-pair run answers `997;1` (dark, second capture),
matching the issue on a release build.
