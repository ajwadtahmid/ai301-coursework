# Eval package: pkg-09

- source: sharkdp/fd#2033
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: sharkdp/fd (44113 stars, archived: no)
- description: A simple, fast and user-friendly alternative to 'find'.
- latest release: v10.4.2 (2026-03-10)
- bug reports: template asks reporters to confirm they read the troubleshooting section, describe the bug and the expected behavior, and give their fd version and operating system
- contribution policy (CONTRIBUTING.md, pull-request guidelines): AI-assisted code contributions are accepted from contributors who understand the code and must state the tool and the extent of its use in the pull request; comments to maintainers are expected to be in the contributor's own words and voice (AI help with grammar, spelling, and proofreading is fine); the policy states no disclosure ask for issue comments

## Issue

### [BUG] `--exec-batch` doesn't always run commands in order (#2033)

opened by tmccombs (COLLABORATOR) on 2026-06-23, state open, labels: bug

The man documentation for `--exec-batch` states: "Like --exec, this can
be used multiple times, in which case each command will be run in the
order given." However, there are a couple of scenarios where this does
not happen:

1. If `--batch-size` is used, then all commands will be run in order on
   each batch. This is potentially expected behavior, but if so, should
   be better documented.
2. If the command line size limit is hit for one command before
   another, say the second command hits the limit before the first
   does, then the command that hits the limit first will be run before
   the other. This is certainly unexpected and surprising behavior, and
   we should probably handle that better.

Version: 10.4.2. Operating system: applies to all.

## Thread highlights (1 comment total)

- 2026-06-30 hiSandog (NONE): the second case seems like the more surprising contract violation; suggests an acceptance test with two batch commands appending markers to a file, one command forced to flush earlier by a lower argument-size threshold, with the marker order required to match the CLI order

## Candidate claim comment

Hi, I'd like to look into the ordering guarantee here as a first
contribution. I spent an evening trying to trigger scenario 2 (the
argument-size flush reordering) and could not provoke it on my machine;
my full attempt is below, including what I think differed. If the
reordering needs a specific length distribution to trigger, I'd like to
work toward the acceptance test suggested in the thread.

## Candidate repro report

Result: I could NOT reproduce scenario 2 (a later `--exec-batch`
command running before an earlier one when the argument-size limit is
hit). I did not test scenario 1 (`--batch-size` interleaving); this
report is about scenario 2 only. Details of the failed attempt and
what differed from the report's conditions below.

Environment: fd 10.4.2 (pacman), Arch Linux (x86_64), kernel 6.15,
`getconf ARG_MAX` = 2097152.

Steps:

```
$ mkdir /tmp/fdtest && cd /tmp/fdtest
$ for i in $(seq 1 120000); do : > "$(printf 'file_%06d_%s' "$i" \
    "$(head -c 180 /dev/zero | tr '\0' 'x')")"; done
$ fd -t f . \
    --exec-batch sh -c 'echo ONE >> /tmp/order.log; true -- "$@"' _ \
    --exec-batch sh -c 'echo TWO >> /tmp/order.log; true -- "$@"' _
$ cat /tmp/order.log
ONE
ONE
ONE
TWO
TWO
TWO
```

With 120000 files of ~190-character names, each command's argument list
exceeds ARG_MAX and flushes in three batches. I ran this 5 times and
also re-ran with the second command's arguments padded to be ~40%
longer than the first's (a longer wrapper string), trying to make
command TWO hit the limit first.

Expected (per the report): with the second command hitting the size
limit first, a TWO flush should appear in the log before the final ONE
flush.

Actual: in every run, all ONE batches were written before any TWO
batch; I never observed a TWO marker overtaking a ONE marker.

What differed from the report's conditions: the report states the
reordering needs one command to hit the limit before the other, and my
padding approach may not achieve that, since fd appears to flush both
command buffers at the same file-count boundary on this input (uniform
name lengths). A distribution where argument lengths differ per file,
or a much lower forced limit than my 2 MiB ARG_MAX, may be required. I
did not find a knob to force a smaller limit from the CLI.
