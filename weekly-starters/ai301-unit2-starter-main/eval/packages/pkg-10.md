# Eval package: pkg-10

- source: starship/starship#7648
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: starship/starship (59450 stars, archived: no)
- description: The minimal, blazing-fast, and infinitely customizable prompt for any shell!
- latest release: v1.26.0 (2026-06-28)
- bug reports: template asks reporters to file via `starship bug-report` (pre-populates the system configuration) and to give current behavior, expected behavior, environment (starship version, shell type and version, OS), and the relevant starship configuration
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### BUG: Prompt disappears when path is a symlink to a subdirectory inside a Git repository (#7648)

opened by ccqpein (CONTRIBUTOR) on 2026-08-01, state open, labels: bug

Current behavior: when `[directory]` has `repo_root_style` enabled,
visiting a directory that is a symlink pointing to a subdirectory
inside a Git repo, the `directory` module outputs nothing (the current
working directory vanishes from the prompt).

For example: logical path `~/projects/app-dir` symlinked to
`/Users/user/code/monorepo/packages/app-dir`, Git root directory
`/Users/user/code/monorepo`. Navigating to `~/projects/app-dir`
renders a blank directory prompt, and `starship explain` omits the
`directory` module completely.

Expected behavior: starship should still render the current directory
path instead of hiding the directory module.

Possible solution (from the reporter): in `src/modules/directory.rs`,
`contract_repo_path(display_dir, repo_root)?` returns `None` for such
paths and the `?` operator drops the whole module.

Environment: starship 1.26.0, fish 4.7.1, no plugin manager, macOS.
Configuration: `[directory]` with `repo_root_style = "bold red"`.

## Thread highlights (0 comments total)

(no comments)

## Candidate claim comment

Hi! I picked this directory-module issue as a first contribution. I
tried to reproduce it before claiming and so far cannot (details
below): on my Linux + zsh setup the exact symlink layout renders fine.
I'd still like to take it: my next step is a macOS + fish environment
matching the report, and reading `contract_repo_path` for the `None`
path the reporter points at, which looks plausible independent of my
failed attempt.

## Candidate repro report

Result: cannot reproduce on Linux + zsh with the report's exact layout
and config. What I ran and what differed from the report's environment
is below.

Environment: starship 1.26.0 (cargo install), zsh 5.9, Ubuntu 24.04
(x86_64). The report is macOS + fish 4.7.1; shell and OS both differ,
starship version matches.

Steps:

```
$ mkdir -p ~/code/monorepo/packages/app-dir && cd ~/code/monorepo && git init -q
$ mkdir -p ~/projects && ln -s ~/code/monorepo/packages/app-dir ~/projects/app-dir
$ cat ~/.config/starship.toml
[directory]
repo_root_style = "bold red"
$ cd ~/projects/app-dir
```

Prompt after the last `cd` (colors stripped):

```
monorepo/packages/app-dir on  master
```

`starship explain` in that directory lists the `directory` module with
the contracted repo path; it is not omitted.

Expected (per the report): the directory module vanishes and
`starship explain` omits it.

Actual: the module renders on every attempt (fresh shell, repeated
`cd`, absolute vs `~` symlink targets). What differed from the report:
OS (Linux vs macOS), shell (zsh vs fish), and my prompt shows the
resolved physical path, which suggests my shell reports `PWD`
differently for symlinked directories than fish does on macOS. A fish
shell resolving `PWD` logically looks necessary to hit the
`contract_repo_path` failure; I did not have one available for this
attempt.
