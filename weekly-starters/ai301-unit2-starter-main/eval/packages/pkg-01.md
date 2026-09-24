# Eval package: pkg-01

- source: httpie/cli#1640
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: httpie/cli (38427 stars, archived: no)
- description: Modern, user-friendly command-line HTTP client for the API era. JSON support, colors, sessions, downloads.
- latest release: 3.2.4 (2024-11-01)
- bug reports: template asks reporters to confirm they searched for similar issues and are on the latest version, and to provide minimal reproduction steps
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### JSON content type not being set when a single header is present (#1640)

opened by gargantuanprism (NONE) on 2025-06-25, state open, labels: bug, new

Minimal reproduction code and steps:

1. `https post pie.dev/post -v 'header1: xyz' x=1`
2. `https post pie.dev/post -v 'header1: xyz' 'header2: abc' x=1`

Current result, output from step 1 (request headers):

```
POST /post HTTP/1.1
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: pie.dev
header1: xyz

{"x": "1"}
```

(Neither HTTPie's verbose output nor pie.dev's response reports `Content-Type: application/json`.)

Expected result, as seen in the output from step 2, where the request carries two custom headers:

```
POST /post HTTP/1.1
Accept-Encoding: gzip, deflate
Connection: keep-alive
Content-Type: application/json
header1: xyz
header2: abc
Host: pie.dev
```

## Thread highlights (6 comments total)

- 2025-06-26 rjw57 (NONE): also hitting this; possibly related to httpie/cli#1637, may be fixed by upgrading the `multidict` dependency; pinning httpie to 3.2.3 as a workaround
- 2025-07-05 Oluwasetemi (NONE): experiencing this as well
- 2026-02-17 worksbyfriday (NONE): same root cause as httpie/cli#1637: a regression in multidict 6.5.0 broke `CIMultiDict.popone()`, which httpie's `apply_missing_repeated_headers()` relies on; the multidict fix has since shipped upstream

## Candidate claim comment

Hi, I'm working through this as a first contribution. I can reproduce the missing `Content-Type: application/json` on 3.2.4 with exactly one custom header (repro below, no network needed with `--offline`). Next I want to check the `apply_missing_repeated_headers()` path against the multidict versions mentioned above and report back what I find.

## Candidate repro report

Environment: HTTPie 3.2.4 (pip), Python 3.12.4, multidict 6.6.0, macOS 14.5 (arm64).

Steps (offline, prints the request without sending):

```
$ http --offline post pie.dev/post 'header1: xyz' x=1
POST /post HTTP/1.1
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: pie.dev
header1: xyz

{"x": "1"}
```

Control run, no custom header:

```
$ http --offline post pie.dev/post x=1
POST /post HTTP/1.1
Accept-Encoding: gzip, deflate
Connection: keep-alive
Content-Type: application/json
Host: pie.dev

{"x": "1"}
```

Expected: both requests carry `Content-Type: application/json`, since the body is a JSON object either way.

Actual: with exactly one custom header present, the `Content-Type` header is absent from the request, matching the report. With zero custom headers it is set correctly.
