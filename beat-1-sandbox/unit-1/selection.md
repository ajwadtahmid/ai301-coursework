# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/conda/conda/issues/16475

**Verdict output**

Repo is maintained (active, commits within 6 months) ✓
Maintainer is present (recent releases and commits) ✓
Not claimed (no assignee, no linked PRs) ✓
Scope is bounded (docs task with clear requirements) ✓
No AI policy barrier (conda welcomes AI-assisted contributions) ✓
Has first-issue label (type::documentation) ✓

```json
{
  "item": "conda/conda#16475",
  "checks": [
    {"name": "Repo is maintained", "grade": "pass", "evidence": "Last push 2026-08-04, latest release 2026-07-31"},
    {"name": "Maintainer is present", "grade": "pass", "evidence": "Multiple commits within past month"},
    {"name": "Not claimed", "grade": "pass", "evidence": "No assignee, no linked PRs"},
    {"name": "Scope is bounded", "grade": "pass", "evidence": "Documentation task with clear scope: add docs for PyPI packages with conda install"},
    {"name": "No AI policy barrier", "grade": "pass", "evidence": "CONTRIBUTING.md welcomes AI tools if contributor understands changes"},
    {"name": "Has first-issue label", "grade": "pass", "evidence": "Labeled type::documentation"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

14/15 (calibration run), 17/20, 18/20 (final submitted run). The last score of 18/20 matches the `eval-run.txt` agreement line.

**Issue analysis**

Issue-15 (zulip/zulip#19589). Gold label: reject. My rubric: accept. Reason: The issue is a "Separate `command` and `text` field for slack-compatible outgoing webhook" feature with 97 comments and 2 closed/abandoned PRs showing repeated implementation attempts. My "Scope is bounded" check failed to detect this complexity flag because the check wording focuses on "multiple abandoned/closed linked PRs" but doesn't catch the comment volume as a signal of unresolved design debate. The gold label correctly rejected it: "years of design debate and two abandoned PRs behind a friendly label."

**Check rationale**

From rubric.md: "Scope is bounded: Not labeled as megaissue/epic/tracking; has clear reproduction steps or stated requirements; does not have multiple abandoned/closed linked PRs showing scope creep"

This check catches megaissues and feature requests lacking clear scope, and detects when multiple implementation attempts have failed. The reasoning: a first contributor should get a bounded task with clear acceptance criteria, not one requiring design decisions or involving years of discussion.

**Trade-offs**

The check rejects issue-15 correctly in principle but missed that it should also flag issues with extensive unresolved discussion (97 comments). Issues-04, 11, 19 are simpler tasks (bug fixes with few comments, bounded scope) that passed correctly. The trade-off: the check lets straightforward bugs through but doesn't distinguish between "10 comments of discussion" and "97 comments of design debate." Could improve by checking comment count or explicit "design discussion needed" keywords.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to interests and time**: This issue is adding documentation for a stable conda feature (installing PyPI packages with `conda install`). It's pure documentation work without code changes, giving scope to learn the docs structure and workflow. The task has clear requirements listed, so implementation is straightforward and time-bounded.

2. **What the verdict got right and what I weighed differently**: The rubric correctly identified this as a good first issue—active repo, responsive maintainers, clear scope, no claims. I weighted the fact that it's purely documentation (lower risk than code bugs), and the contributor notes show exactly what files to change and what content to add. A newcomer can make meaningful progress without deep domain knowledge.

3. **Anticipated difficulty in claiming it**: Low-to-medium. The task is well-scoped and documented. Main challenges: learning the conda docs structure (Sphinx/RST format), understanding the exact guidance needed for PyPI package installation, and getting the PR reviewed. The maintainers seem responsive, so the review should be constructive.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
