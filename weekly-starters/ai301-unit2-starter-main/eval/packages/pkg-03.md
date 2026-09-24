# Eval package: pkg-03

- source: BurntSushi/ripgrep#2779
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: BurntSushi/ripgrep (67344 stars, archived: no)
- description: ripgrep recursively searches directories for a regex pattern while respecting your gitignore.
- latest release: 15.2.0 (2026-07-15)
- bug reports: template asks for the ripgrep version, how it was installed, the operating system, a description of the bug, steps to reproduce, actual behavior, and expected behavior
- contribution policy (CONTRIBUTING.md section "Use of AI", AI_POLICY.md): AI-assisted coding is welcome with a human in the loop who understands the work; comments to maintainers must be written by humans in their own words, and AI-generated comments may be hidden

## Issue

### Adjacent replaced multiline matches result in wrong line numbers (#2779)

opened by meedstrom (NONE) on 2024-04-11, state open, labels: bug

Version: ripgrep 13.0.0. OS: Kubuntu 23.10, installed via APT.

Using a multiline regexp with `--replace`, when the regexp matches strings that come immediately one after another, it bungles the line numbers of all of them.

Steps to reproduce: save a file `test.txt` containing:

```
:properties:
:id: fnord
:end:
:properties:
:id: boccob
:end:
:properties:
:id: d321fdddffff
:end:
:properties:
:id: clowns
:end:
```

Then run:

```
rg -nU '^:properties:\n:id: (.*)\n:end:' -r '$1' test.txt
```

Actual behavior:

```
1:fnord
2:boccob
3:d321fdddffff
4:clowns
```

Only the first hit is correct. Expected:

```
1:fnord
4:boccob
7:d321fdddffff
10:clowns
```

## Thread highlights (6 comments total)

- 2024-04-11 BurntSushi (OWNER): the incomplete part of the description: it is not just adjacent matches, the `--replace` flag is also required to trigger it
- 2024-04-26 meedstrom (NONE): offered to look for the problem, asked for pointers
- 2024-04-26 BurntSushi (OWNER): likely in `grep-printer`; look at the "standard" printer

## Candidate claim comment

I'd like to take a run at this one as a first contribution. Reproduced on current 15.2.0 (report below), and per the pointer above I'll start reading the standard printer in grep-printer to see where line numbers are computed for adjacent replaced matches.

## Candidate repro report

Environment: ripgrep 15.2.0 (cargo install), Arch Linux (x86_64). The issue was filed against 13.0.0; behavior is unchanged on 15.2.0.

Steps: created `test.txt` with the exact 12 lines from the issue (four `:properties:`/`:id:`/`:end:` blocks, no blank lines between them), then:

```
$ rg -nU '^:properties:\n:id: (.*)\n:end:' -r '$1' test.txt
1:fnord
2:boccob
3:d321fdddffff
4:clowns
```

Expected: line numbers of each match's starting line, so 1, 4, 7, 10.

Actual: 1, 2, 3, 4 as shown above; every match after the first reports the wrong line. Dropping `-r '$1'` from the same command reports 1, 4, 7, 10 correctly, which matches the owner's note that `--replace` is required to trigger it.
