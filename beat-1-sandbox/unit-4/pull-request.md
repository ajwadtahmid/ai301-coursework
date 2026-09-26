# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/69

**Branch**

fix/68-empty-index-guard

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

20/20 (first run, submitted with --save-run)

**Package analysis**

Perfect agreement on all 20 packages. No disagreements to analyze. The rubric correctly categorized:
- All 7 clear-accept packages (pkg-02, 05, 08, 11, 13, 16, 19): Diff matched plan scope, observable before/after evidence, template sections filled
- All 4 not-tested packages (pkg-04, 07, 10, 14): Evidence lacked observable before/after or exercised unchanged paths only
- All 4 silent-drift packages (pkg-03, 06, 09, 17): Diff silently delivered scope not in plan, or description claims contradicted diff
- All 2 standards-wall packages (pkg-01, 20): Required template sections or AI disclosure absent
- All 3 unreviewable packages (pkg-12, 15, 18): Debug prints, commented-out code, or formatting churn buried the fix

The perfect calibration indicates the checks correctly capture what makes PRs ready vs not ready.

**Check rationale**

From rubric.md, the "Plan fidelity" check:

"Plan fidelity | Diff against plan's Files and Approach sections; description claims against diff's actual behavior | Diff stays within plan boundary: all changed files appear in plan's stated Files section, OR plan explicitly notes a deviation for each file outside scope. When diff contains less than plan (fewer files, incomplete steps), the description and plan both acknowledge it. Description's claims of what the PR does match diff's actual content (no claiming more or less than delivered, no silent scope creep). | required"

This check targets the silent-drift failure family directly. I chose it because silent-drift — the diff doing more or less than the plan, or the description misrepresenting what the diff does — is the most common failure mode in the scored packages (4 of 20). The check operationalizes "staying in scope" as: (1) all files in the diff appear in the plan's Files list, or (2) each outside file has an explicit deviation note. This makes scope drift observable and repeatable. The check also requires description claims to match diff content, catching cases like pkg-06 where "no functional changes" contradicts the diff's new config option. The rule does not require the diff to be minimal or beautiful, only that it stays within stated bounds and is honestly described.

**Trade-offs**

The rubric trades off strict scope enforcement against honest-outcome allowance. An alternate rubric could fail any PR with deferred work (stricter), or accept any partial delivery with any note (looser). The current form requires deferrals to be explicit in the plan AND restated in the description, matching the plan's own language. This earned 20/20 because the scored packages were designed to test exactly this trade: packages like pkg-13 and pkg-16 have honestly disclosed deferrals and pass, while packages like pkg-17 silently deliver less and fail. The rubric succeeded because it judges the thing itself (does scope match? is work honestly scoped?) not the prose (how long is the description?). The 20/20 agreement with no re-runs needed means the rubric is appropriately calibrated for this problem domain.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
