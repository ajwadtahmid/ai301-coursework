---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

This tool answers: **Is this pull request ready to submit?**

A PR package contains five artifacts: a plan the PR claims to implement (excerpted from plan-context), the repository's stated template and policy (repo-facts), the candidate PR itself (title, description, commits, diff), and evidence that tests pass (test-evidence section). This tool reads all five against each other to decide whether the PR honors the plan, shows decisive evidence, runs clean code, and meets the repository's standards.

## Inputs and modes

**Eval mode:** The bundle provided is the complete world; every part is read from the package and nothing is fetched. Every check runs with the full verdict rule (all required checks must pass).

**Live mode (grading your own PR before opening):** 
- Read your plan.md from the current working directory (its Files, Scope, Approach, test-plan sections and any deviation notes)
- Read the diff with `git diff main...HEAD` (three dots) run from the working copy; this produces everything your branch changes relative to the repo's default branch
- Read your draft PR title and description (from your local editor or the GitHub PR preview)
- Read your test evidence from captured before/after output (console transcripts, screenshots) you've saved
- Read the repository's PR template from the repo's GitHub settings or CONTRIBUTING.md
- Read the repository's stated AI-use policy from CONTRIBUTING.md or the repo's documentation
- For a house student (working on an instructor-assigned issue): the instructor's plan stands in for plan.md; all other inputs apply

## The scope seam (live mode only)

When you run this tool in live mode, before grading, read scope.md to learn which repositories are in scope for this exercise (Path Review classroom repo only this week). If the scope line reads as `<ORG>/<PATH-REVIEW-REPO>` (a placeholder), stop and ask your instructor for the finalized Path Review repo link before continuing; eval mode ignores scope.md, so the harness works either way. Once you confirm the repo is in scope, proceed to grading.

## The voice seam (live mode only)

After grading is complete, read voice-guide.md to learn communication rules for the PR title and description (how to write clearly, how to engage with maintainers). Voice-guide violations (e.g., unclear language, ignoring a maintainer's request) are reported to you as guidance, not as verdict changes: the verdict depends on the rubric alone, and voice violations are notes for revision.

## Component reads

This tool executes in three stages: **read** (following procedure.md's read order), **gather** (following procedure.md's evidence gathering), and **grade** (executing procedure.md's check execution and verdict assembly). The rubric.md defines what each check measures. The references/evidence-guide.md tells you where each kind of evidence lives and what good looks like. When procedure.md is silent on a step, report the gap; never fill it yourself. When rubric.md or procedure.md is empty or incomplete, refuse: a blank rubric means you cannot grade.

## Verdict and output

**Verdict space:** Binary — **accept** (the PR is ready to submit) or **reject** (hold for revision).

**Verdict rule:** Apply the rule in rubric.md's Verdict rule section. Unclear on a required check means fail. When the first required check fails, the verdict is reject; if all required checks pass, the verdict is accept.

End your reply with the JSON block below, valid and last, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

1. **Evidence first:** Every grade is justified by evidence from the package or live repo (diff, description, test output, template). Absent or ambiguous evidence counts as fail, not unclear.
2. **Grade the thing, not the polish:** Judge whether the diff implements the plan, whether evidence is decisive, whether code is clean, whether the description is accurate — not whether the prose is eloquent or the commits are many or few.
3. **The rubric decides:** Each check's pass condition determines the grade; apply the condition as written, not as you might wish it read.
4. **The procedure decides how:** Each check is executed per procedure.md's steps. Missing or ambiguous procedure counts as a procedural gap; report it and do not improvise.
5. **Unclear is fail:** When a check's evidence is absent or the rubric's rule is too vague to apply with certainty, grade it fail, not unclear (per the verdict rule: unclear on required checks means fail).
