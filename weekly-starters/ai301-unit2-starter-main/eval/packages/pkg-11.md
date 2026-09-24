# Eval package: pkg-11

- source: mikefarah/yq#2797
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: mikefarah/yq (15843 stars, archived: no)
- description: yq is a portable command-line YAML, JSON, XML, CSV, TOML, HCL and properties processor.
- latest release: v4.53.3 (2026-06-06)
- bug reports: template asks for the yq version, operating system, how yq was installed, a concise input document (10 lines or less), the command run, actual behavior, and expected behavior
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### Multi-line strings erroneously written as single line in the presence of some UTF8 chars (#2797)

opened by spockz (NONE) on 2026-08-05, state open, labels: bug, v4

When updating a yaml to add or update a field with a multi-line string,
the field is emitted as a single-line string with encoded newlines
instead of a multiline block, in the presence of some UTF8 characters.

With `some.yml` containing `foo: bar` and `broken.md` a multi-line file
containing the 🚧 character, the command
`yq eval '.longDescription |= (load_str("broken.md"))' some.yaml`
produces a single double-quoted line with `\n` escapes. Expected: a
`longDescription: |` block scalar preserving the lines. Interestingly,
the same command with a file containing ✅ instead emits the expected
block scalar.

Version: 4.53.3. Operating system: linux. Installed via pacman.

## Thread highlights (1 comment total)

- 2026-08-16 ccoVeille (NONE): a go-yaml maintainer; the underlying issue is tracked in yaml/go-yaml#354 with a fix PR in yaml/go-yaml#357

## Candidate claim comment

Hi, first contribution here. Reproduced this on 4.53.3 on macOS
(report below); the character class seems to be what decides the
emitter style. Given the go-yaml thread linked above, my plan is to
verify whether yq's vendored go-yaml carries the fix from
yaml/go-yaml#357 and report back before touching yq's own emitter
code.

## Candidate repro report

Environment: yq 4.53.3 (Homebrew), macOS 15.5 (arm64).

Steps:

```
$ printf 'foo: bar\n' > some.yml
$ printf 'Multiline file containing\n\n🚧\n\nA "weird" utf8 char?\n' > broken.md
$ yq eval '.longDescription |= (load_str("broken.md"))' some.yml
foo: bar
longDescription: "Multiline file containing\n\n\U0001F6A7\n\nA \"weird\" utf8 char?\n"
```

Control with ✅ instead of 🚧:

```
$ printf 'Multiline file containing\n\n✅\n\nA "less weird" utf8 char?\n' > works.md
$ yq eval '.longDescription |= (load_str("works.md"))' some.yml
foo: bar
longDescription: |
  Multiline file containing

  ✅

  A "less weird" utf8 char?
```

Expected: both runs emit a block scalar (`longDescription: |`) that
preserves the lines.

Actual: the 🚧 input collapses to a single double-quoted line with
escaped newlines (shown above, with the character itself escaped as
`\U0001F6A7`); the ✅ input emits the expected block scalar. Matches
the report on a different OS and install method.
