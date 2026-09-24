# Eval package: calib-04

- source: sharkdp/hyperfine#920
- captured: 2026-08-17
- calibration: true

## Repo facts (captured 2026-08-17)

- repo: sharkdp/hyperfine (28666 stars, archived: no)
- description: A command-line benchmarking tool.
- latest release: v1.20.0 (2025-11-18)
- bug reports: no structured template; reports are expected to describe the problem and how to reproduce it
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### panic: integer overflow in `--parameter-scan` near i32::MAX (#920)

opened by nikolauspschuetz (NONE) on 2026-08-14, state open, labels: none

`--parameter-scan` panics with an integer overflow when the range ends
at (or one step below) `i32::MAX`. Minimal reproduction:

```
hyperfine --parameter-scan x 2147483646 2147483647 'true'
```

Debug/test builds panic with `attempt to add with overflow`; release
builds wrap from `i32::MAX` to `i32::MIN` instead of panicking, so the
iterator keeps going and hyperfine tries to generate about 4 billion
commands (effectively a hang / OOM).

Cause (from the reporter): min/max parse as `i32`, the `MAX_PARAMETERS`
cap is only enforced at construction via `size_hint`, and the range
iterator's `next()` does an unconditional `state += step`, which
overflows on the final element. Expected: the scan yields the in-range
values and stops, wherever the range sits in the `i32` domain. The
reporter says they have a small fix plus regression test ready.

## Thread highlights (0 comments total)

(no comments)

## Candidate claim comment

Hi, I'd like to verify and follow this overflow issue as a first
contribution here. I've reproduced the panic with the issue's exact
command (report below). Since the reporter already has a fix ready,
I'll offer independent testing of that PR across build profiles first.

## Candidate repro report

Ran the issue's minimal reproduction exactly as given:

```
$ hyperfine --parameter-scan x 2147483646 2147483647 'true'
Benchmark 1: true
thread 'main' panicked at src/parameter/range_step.rs:66:9:
attempt to add with overflow
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

A control run of the same scan shifted away from the boundary
completes normally:

```
$ hyperfine --parameter-scan x 1 2 'true'
Benchmark 1: true
  Time (mean +- sd):     1.1 ms +-   0.2 ms
Benchmark 2: true
  Time (mean +- sd):     1.1 ms +-   0.2 ms
```

Expected: the boundary scan behaves like the control, benchmarking the
two in-range values and exiting cleanly.

Actual: the panic shown above, on the final element of the range,
matching the issue's description of the debug-build behavior.
