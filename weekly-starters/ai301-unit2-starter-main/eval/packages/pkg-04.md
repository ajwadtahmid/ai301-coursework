# Eval package: pkg-04

- source: junegunn/fzf#2021
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: junegunn/fzf (82552 stars, archived: no)
- description: A command-line fuzzy finder.
- latest release: v0.74.3 (2026-08-17)
- bug reports: template asks for OS, shell, confirmation the manual and existing issues were checked, and the problem with steps to reproduce
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### Elements in the list of choices overflow their lines (#2021)

opened by romkatv (CONTRIBUTOR) on 2020-05-07, state open, labels: bug

Steps to reproduce:

1. Set terminal width to 80 and terminal height to at least 4.
2. Feed fzf a multi-line payload containing tab characters (the report pipes the zsh `run-help` function source, twice, NUL-separated) and run:

```
... | fzf --read0 --query setcap
```

The matched elements render past the edge of their lines and overflow into the next lines, corrupting the list display. Removing all tabs from the payload makes the problem disappear.

## Thread highlights (11 comments total)

- 2020-05-07 junegunn (OWNER): clearly a bug in the way fzf processes tab stops; a workaround is `--tabstop 1`
- 2020-05-07 romkatv (CONTRIBUTOR): suspected tabs too; could not trigger it with a smaller synthetic payload
- 2020-07-26 pedromss (NONE): still happens with `--tabstop 1` on Ubuntu 18.04, fzf 0.21.1 (screenshot)

## Candidate claim comment

+1 also seeing this!! it's super annoying, any updates on a fix?? would love for this to get resolved soon 🙏

## Candidate repro report

Can confirm this happens for me too. The lines totally overflow just like the screenshots show, my whole list gets garbled whenever there are tabs in the input. It's exactly the same behavior as described above, so this is definitely still a bug. Really hoping someone can fix this soon because it makes fzf hard to use for this kind of input. Happy to help test a fix whenever one is up!
