# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis grounded | Plan's stated cause (Diagnosis section) read against Repro evidence's shown behavior | The stated cause directly explains the behavior the repro evidence pins down, without contradicting or ignoring the evidence; diagnosis cites specific evidence (e.g., "line 25 crashes when..."), not generic descriptions. | required |
| Scope is bounded | Plan's "In scope" and "Not in scope" statements read against the Files list and Approach | The change is clearly bounded to specific files and areas; scope statement rejects related-but-separate work (e.g., "not argparse" when the bug is in request parsing); drive-by rewrites or "while we're here" additions are not present. | required |
| Plan is executable | Plan's Files, Approach, and Test plan sections | A stranger could start executing this plan without asking the author anything: files are named exactly, approach lists concrete steps in order, test plan names what will be observed (not just "run tests"). | required |
| Test plan is decisive | Plan's Test plan section read against Repro evidence's steps | The test plan directly exercises the scenario the repro evidence showed was failing; the plan says what success looks like observable (output changes, exit code changes, test passes) and ties it to the repro evidence. | required |
| Honesty about unknowns | Plan's "Unknowns and risks" section (or absence of one) | Risks and unknowns are named plainly (not glossed as "minor"); false confidence (stating certainty without evidence) fails; genuine "don't know" earns points; deviation section in plan.md is filled if build teaches something. | required |
| Plan comment is thread-aware | Plan comment read against thread highlights and repo-facts block | The comment references the issue's specific symptoms (not generic "+1"); acknowledges relevant thread comments or maintainer signals if present; respects AI-disclosure policy if stated; avoids boilerplate that could apply to any issue. | required |

## Verdict rule

Accept if every required check passes; unclear on any required check counts as fail (evidence to verify the check is absent).
