# Eval package: pkg-05

- source: conda/conda#16543
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: conda/conda (7485 stars, archived: no)
- description: A system-level, binary package and environment manager running on all major operating systems and platforms.
- latest release: 26.7.0 (2026-07-31)
- bug reports: template asks for a descriptive title, a duplicate search, what happened, and the output of `conda info` and `conda list`
- contribution policy (CONTRIBUTING.md, section "Generative AI"): generative AI tools welcome; you are responsible for all contributions and must review and understand AI-generated content before including it in a pull request

## Issue

### EnvironmentSectionNotValid message breaking json output (#16543)

opened by beenje (CONTRIBUTOR) on 2026-08-14, state open, labels: type::bug

Running `conda env update --quiet --json -f <conda-lock python-3.14 environment yaml URL> 2>/dev/null` produces:

```
EnvironmentSectionNotValid: The following section on '<the yaml URL>' is invalid and will be ignored:
 - category

{
  "success": true,
  "message": "All requested packages already installed."
}
```

which breaks the json output.

The `EnvironmentSectionNotValid` error message should be printed to stderr and not stdout.

Reporter's `conda info` shows conda 26.7.0, Python 3.12.7, libmamba solver, osx-arm64 (miniforge3).

## Thread highlights (0 comments total)

(no comments)

## Candidate claim comment

First contribution attempt: I reproduced this on 26.7.0 (report below) and want to trace where `EnvironmentSectionNotValid` gets emitted, since with `--json` every human-readable message should be routed to stderr. I'll report back what I find in the exceptions path.

## Candidate repro report

Environment: conda 26.7.0 (miniforge3), Python 3.12.7, macOS 15.5 (osx-arm64), libmamba solver.

Steps: wrote a minimal `env.yml` containing a valid `dependencies:` list plus a `category:` section (the section conda does not recognize), then:

```
$ conda env update --quiet --json -f env.yml 2>/dev/null
EnvironmentSectionNotValid: The following section on 'env.yml' is invalid and will be ignored:
 - category

{
  "success": true,
  "message": "All requested packages already installed."
}
```

Piping the same stdout into a JSON parser fails on the leading error text:

```
$ conda env update --quiet --json -f env.yml 2>/dev/null | python3 -m json.tool
Expecting value: line 1 column 1 (char 1)
```

Expected: with `--json` and stderr discarded, stdout contains exactly one parseable JSON document.

Actual: the `EnvironmentSectionNotValid` warning is printed to stdout above the JSON (shown above), so the stream does not parse. The warning belongs on stderr.
