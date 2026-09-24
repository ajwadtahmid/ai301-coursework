# Eval package: pkg-16

- source: pandas-dev/pandas#66656
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: pandas-dev/pandas (49509 stars, archived: no)
- description: Flexible and powerful data analysis / manipulation library for Python, providing labeled data structures similar to R data.frame objects, statistical functions, and much more.
- latest release: v3.0.5 (2026-07-22)
- bug reports: template asks reporters to confirm the bug exists on the latest version and on the main branch, and to provide a reproducible example, an issue description, the expected behavior, and the output of `pd.show_versions()`
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### BUG: Passing a tuple at creation for 1-d index in df is fine but rename_axis with tuple fails (#66656)

opened by loicdiridollou (MEMBER) on 2026-08-08, state open, labels: Bug, Indexing

Reproducible example:

```python
import pandas as pd

df = pd.DataFrame([1], index=pd.Index([1], name=(1, 2, 3)))  # fine
df.reset_index().rename_axis((1, 2, 3))  # crashes at runtime
```

It is unclear why creating an index with a tuple name is allowed while
renaming the axis with that same tuple fails at runtime. Surfaced while
typing `index.name` and `df.rename_axis` in the pandas-stubs package.
Expected behavior: creation and renaming should accept the same types.

The reporter confirmed the bug on the latest version and on the main
branch (template checks ticked); the installed-versions block shows
python 3.14.6 and a main-branch pandas build on macOS arm64.

## Thread highlights (1 comment total)

- 2026-08-08 aaron-seq (NONE): confirms on pandas 2.3.3 and current main; the failure is in pandas/core/indexes/base.py inside Index.set_names, which wraps names in a list only for scalars, so the tuple takes the sequence path and fails the length check

## Candidate claim comment

Hi, I'd like to pick this up as a first pandas contribution. I have
reproduced the crash (report below) and the thread's pointer at
Index.set_names gives me a concrete place to start; I'll investigate
whether tuple names should be wrapped as scalars there and report
back.

## Candidate repro report

Environment: pandas 1.5.3 (pip), Python 3.10.12, Ubuntu 22.04 (x86_64).

Steps:

```python
>>> import pandas as pd
>>> df = pd.DataFrame([1], index=pd.Index([1], name=(1, 2, 3)))
>>> df.reset_index().rename_axis((1, 2, 3))
Traceback (most recent call last):
  ...
ValueError: Length of new names must be 1, got 3
```

Expected: `rename_axis` accepts the tuple, the same way `pd.Index`
accepted it as a name at creation.

Actual: the call raises the ValueError shown above. The crash the
issue describes is confirmed: index creation with a tuple name
succeeds and rename_axis with the same tuple fails at runtime.
