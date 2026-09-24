# Eval package: calib-01

- source: jesseduffield/lazygit#5883
- captured: 2026-08-17
- calibration: true

## Repo facts (captured 2026-08-17)

- repo: jesseduffield/lazygit (81407 stars, archived: no)
- description: Simple terminal UI for git commands.
- latest release: v0.64.1 (2026-08-12)
- bug reports: template asks for the lazygit version, git version, operating system, and a description with steps
- contribution policy (CONTRIBUTING.md): the maintainer reviews outside pull requests only selectively (maintaining is a hobby and review time is scarce, especially with AI-generated PRs hard to assess); issue reports remain welcome; no stated AI disclosure requirement

## Issue

### [UX] Pressing 's' to stash untracked files accepts a name but silently does nothing (#5883)

opened by ThatOn3Gu7 (NONE) on 2026-07-31, state open, labels: bug

With an untracked file focused in the Files panel, pressing `s` opens
the "Stash name" popup as if everything is good to go. After typing a
name and hitting Enter, the popup closes and nothing happens: the file
remains untracked, no stash is created, and no toast or error explains
why. Standard `git stash` ignores untracked files by default (that is
what `Shift+S`, stash including untracked, is for), so the failure is
understandable under the hood, but the name prompt appearing makes the
silent no-op confusing.

Steps: 1. create a brand-new untracked file in a repo with no other
tracked changes; 2. focus it in the Files panel and press `s`; 3. type
a stash name, hit Enter; 4. the prompt disappears, no stash, no
notification.

Environment: lazygit v0.63.1 (Termux, Android arm64), git 2.55.0.

## Thread highlights (2 comments total)

- 2026-07-31 ThatOn3Gu7 (NONE): hope you guys fix it soon
- 2026-08-08 bhallashivam1997 (NONE): pushed a fix for this

## Candidate claim comment

Hi, I'd like to look at this stash UX papercut as a first
contribution. Reproduced on current 0.64.1 on Linux (report below), so
it is not Termux-specific. Plan: find where the stash-name prompt
decides to appear and make the untracked-only case either warn or
stash with `--include-untracked`.

## Candidate repro report

Environment: lazygit 0.64.1 (release binary), git 2.55.0, Ubuntu 24.04.
The issue was filed from v0.63.1 on Termux; same behavior here.

Steps and observed:

```
$ git init -q t && cd t && touch new.txt
$ lazygit   # focus new.txt in Files, press 's', name it "test", Enter
$ git stash list
$ git status --short
?? new.txt
```

Expected: a stash named "test" exists (or an error says untracked
files cannot be stashed this way).

Actual: the popup closed with no message; `git stash list` prints
nothing and the file is still untracked, as shown.
