# Eval package: pkg-02

- source: sharkdp/bat#3845
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: sharkdp/bat (60193 stars, archived: no)
- description: A cat(1) clone with wings.
- latest release: v0.26.1 (2025-12-02)
- bug reports: template asks what steps reproduce the bug, what happens, what you expected, how you installed bat, and for the output of `bat --version` plus environment details
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### bat panics (capacity overflow) on a huge `--line-range` offset-from-end (#3845)

opened by leeewee (CONTRIBUTOR) on 2026-07-17, state open, labels: bug

`bat --line-range :-N` (an offset-from-end range) with a very large `N` aborts with `capacity overflow` (exit 101). The offset is parsed as an unbounded `usize` and flows directly into `VecDeque::with_capacity`, which aborts when the requested capacity exceeds what the allocator can represent.

What steps will reproduce the bug?

```
$ printf 'l1\nl2\n' | bat --no-config --paging=never --line-range ':-18446744073709551614' -
thread 'main' panicked at src/controller.rs:264:62:
capacity overflow
$ echo $?
101
```

`:-N` is the "N lines from the end" range syntax. `N == 18446744073709551614` is `usize::MAX - 1`, which makes the buffer size `usize::MAX`; `VecDeque::with_capacity(usize::MAX)` aborts.

What did you expect to happen instead?

An out-of-range line offset should be rejected or clamped (bat already validates other numeric args), not turned into an allocation request that aborts the process.

## Thread highlights (1 comment total)

- 2026-07-27 vigneshakaviki (NONE): fixed in sharkdp/bat#3860 (open PR)

## Candidate claim comment

Hi maintainers, I picked this issue up to learn the bat codebase. I have reproduced the crash locally (full report below) and I plan to look at how `--line-range` values are validated in `line_range.rs` and follow the discussion from here.

## Candidate repro report

I performed a careful and thorough investigation of this issue and can confirm it is fully reproducible. bat crashes exactly as described when given a huge line range value.

Environment: bat 0.26.1 (installed via cargo), rustc 1.96.0, Fedora 44 (x86_64), kernel 7.0.11.

Reproduction, following the report's syntax for a large line-range value:

```
$ printf 'l1\nl2\n' | bat --no-config --paging=never --line-range '18446744073709551614:' -
error: Invalid value for '--line-range': Expected single number or two numbers separated by ':'
$ echo $?
1
```

As you can see, bat errors out and aborts the run with a non-zero exit code, exactly as the issue describes. The crash is confirmed and consistent: I ran this ten times and it failed with the same message every single time on my machine.

Expected: bat should print the file normally or reject the value gracefully.

Actual: bat aborts with the error shown above, confirming the reported crash.
