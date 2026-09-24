# Eval package: pkg-12

- source: prettier/prettier#19795
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: prettier/prettier (52204 stars, archived: no)
- description: Prettier is an opinionated code formatter.
- latest release: 3.9.6 (2026-07-21)
- bug reports: template asks for the prettier version, a playground or runnable reproduction, the input, the actual output, and the expected output
- contribution policy (CONTRIBUTING.md, section "AI usage policy"): only submit code you fully understand and have tested; be prepared to explain your changes; do not ignore the issue and PR templates; low-quality AI content is closed immediately

## Issue

### Range formatting appends a stray token outside the range when the range starts with a comment in an arrow-function body (#19795)

opened by erraitgrand-del (NONE) on 2026-08-06, state open, labels: none

Prettier 3.8.4, Node 22.23.1, `parser: "babel"`, default options. Range
formatting appends a stray token outside the requested range when the
range starts with a comment inside an arrow-function body. Two shapes:

Shape A, arrow passed as a call argument, input
`beforeEach(() => {\n  // c\n  a();\n});\n` with rangeStart 19,
rangeEnd 32: the closing `});` becomes `};);`, which no longer parses.

Shape B, arrow assigned to a const, input
`const f = () => {\n  // c\n  a();\n};\n` with rangeStart 18,
rangeEnd 31: the closing `};` becomes `};;`, an extra empty statement,
which parses and is therefore the quieter corruption.

In both cases the range covers the comment line plus the statement
after it. Expected: input unchanged, since the code inside the range is
already formatted and nothing outside the range should be touched. The
reporter notes this is the same symptom as prettier/prettier#12964
(closed, fixed for 2.8), whose own repro no longer reproduces on 3.8.4,
so these look like variants the old fix did not cover.

## Thread highlights (0 comments total)

(no comments)

## Candidate claim comment

Hi, I'd like to investigate this range-formatting bug as a first
contribution. Both shapes reproduce for me on the current 3.9.6
release (report below), so it is not specific to 3.8.4. My next step
is tracing how range formatting decides the range's closing boundary
when the first thing in range is a comment, starting from the fix that
closed prettier/prettier#12964.

## Candidate repro report

Environment: prettier 3.9.6 (npm, fresh `npm install prettier@3.9.6`),
Node 22.23.1, macOS 14.6 (arm64). The issue was filed against 3.8.4;
both shapes still reproduce on 3.9.6.

Steps: ran the issue's script verbatim in an empty directory:

```
$ node repro.mjs
```

with `repro.mjs` containing the issue's two `prettier.format` calls
(shape A: rangeStart 19, rangeEnd 32; shape B: rangeStart 18,
rangeEnd 31; both `parser: "babel"`).

Output, shape A:

```
beforeEach(() => {
  // c
  a();
};);
```

Re-parsing that output fails: `SyntaxError: Unexpected token, expected
"," (4:2)`.

Output, shape B:

```
const f = () => {
  // c
  a();
};;
```

Expected: both inputs come back unchanged; nothing outside the range
should be touched.

Actual: shape A's `});` became `};);` (not parseable), shape B's `};`
became `};;` (extra empty statement), matching the report exactly on
3.9.6.
