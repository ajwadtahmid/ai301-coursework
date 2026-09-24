# Eval package: pkg-18

- source: golangci/golangci-lint#4242
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: golangci/golangci-lint (19261 stars, archived: no)
- description: Fast linters runner for Go.
- latest release: v2.12.2 (2026-05-06)
- bug reports: template asks reporters to confirm they use a binary release within the 2 latest major releases, searched existing issues, read the typecheck FAQ, and tried the standalone linter, then to provide a problem description, the golangci-lint version, the configuration, the Go environment, and a code example
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### gomodguard: Panic stack trace: goroutine 1 [running] / can't run linter goanalysis_metalinter: panic occurred (#4242)

opened by honey-yogurt (NONE) on 2023-12-08, state open, labels: bug, dependencies

First golangci-lint use, adding pre-commit to an existing project. The
project's go.mod file is not in the root directory; after renaming the
directory containing go.mod, golangci-lint fails with a gomodguard
panic (`can't run linter goanalysis_metalinter: panic occurred`) and a
runtime stack trace. Reported with v1.46.2 via pre-commit, then
confirmed unchanged after updating to v1.55.2.

## Thread highlights (8 comments total)

- 2023-12-08 honey-yogurt (NONE): updated to v1.55.2, same error
- 2023-12-08 Antonboom (CONTRIBUTOR): the panic is in gomodguard itself (processor.go line 146), not golangci-lint; suggests filing there
- 2023-12-08 honey-yogurt (NONE): filed ryancurrah/gomodguard#40 upstream

## Candidate claim comment

Hi, I'd like to work on this gomodguard panic. We hit this exact crash
at my job this week, so I have a live environment where it reproduces
(report below) and a strong motivation to see it fixed. I'll
coordinate with the upstream gomodguard issue as needed.

## Candidate repro report

Reproduced the panic in our work environment.

Environment: golangci-lint v1.55.2 (binary release), Go 1.21, macOS 14.

Steps:

1. Checked out our company monorepo (private; I cannot share it or its
   layout, but the relevant part matches the issue: go.mod lives in a
   subdirectory, not the repo root).
2. Used our internal `.golangci.yml`, which configures gomodguard with
   our internal blocked-modules list (also not shareable, but it is a
   normal gomodguard block list).
3. Ran our pre-commit hook, which invokes golangci-lint the same way
   as the issue reporter's setup.

Output (trimmed to the relevant lines):

```
ERRO [linters_context] gomodguard: parse module file: ...
panic: runtime error: invalid memory address or nil pointer dereference
goroutine 1 [running]:
runtime/debug.Stack()
github.com/ryancurrah/gomodguard.(*Processor).processFiles(...)
ERRO Running error: can't run linter goanalysis_metalinter: panic occurred
```

Expected: golangci-lint reports lint findings, or a readable
configuration error if the go.mod location is a problem.

Actual: the gomodguard panic above, matching the issue. It reproduces
every time in our repo; when I copied the same config into a fresh
scratch module with go.mod at the root, it did not reproduce, so the
subdirectory layout looks essential, as the issue says.
