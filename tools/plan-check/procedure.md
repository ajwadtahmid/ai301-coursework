# Procedure: how this skill grades a plan package

## Read order

1. **Read the Repro evidence first** - note what behavior it demonstrates (the failing symptom, the environment, the steps that trigger it, what's actually happening)
2. **Read the Issue context** - understand what the maintainers are asking for and any relevant thread signals
3. **Read the Candidate plan** - understand the diagnosis, scope, files, approach, test plan
4. **Read the Candidate plan comment** - understand what the author is committing to publicly

This order matters: you must know what the repro evidence actually shows before you can judge whether the plan's diagnosis matches it.

## Evidence gathering

**For Diagnosis grounded check**:
- From Repro evidence: extract the exact behavior shown (e.g., "ZeroDivisionError raised at line 52 in BM25Okapi._initialize")
- From Plan Diagnosis section: extract the stated root cause (e.g., "BM25Okapi divides by corpus_size which is 0")
- Compare: does the stated cause directly explain the shown behavior?

**For Scope is bounded check**:
- From Plan: extract the "In scope" statement, "Not in scope" statement, and Files list
- Record: what specific changes are included? what related work is explicitly excluded?

**For Plan is executable check**:
- From Plan Files section: are file paths exact (e.g., `rag/retriever/keyword_search.py` not `keyword search`)? Are paths unambiguous?
- From Plan Approach section: are steps ordered and concrete (e.g., "add guard at line 17-26" not "fix the method")?
- From Plan Test plan: are success conditions observable (e.g., "test passes" or "error no longer raised" not "verify it works")?

**For Test plan is decisive check**:
- From Repro evidence: what specific scenario failed (steps, environment)?
- From Plan Test plan: does it directly re-run that scenario? Does it show what changed?

**For Honesty about unknowns check**:
- From Plan "Unknowns and risks" section (or note if absent): are risks named plainly or minimized? Is certainty false or grounded?
- If deviations section: is it filled in (honest mid-build change) or empty?

**For Plan comment is thread-aware check**:
- From Candidate plan comment: does it reference specific issue symptoms (not generic)?
- From Thread highlights: are relevant signals acknowledged?
- From Repo facts AI policy: if required, is disclosure present?

## Check execution

Execute checks in this order:
1. Diagnosis grounded (must match repro evidence)
2. Scope is bounded (must be clear)
3. Plan is executable (must be followable)
4. Test plan is decisive (must verify the fix)
5. Honesty about unknowns (must be realistic)
6. Plan comment is thread-aware (must fit the conversation)

**If evidence for a check is genuinely absent** (e.g., no Unknowns section, no repo-facts policy statement):
- Record grade as "unclear" with evidence note "Unknowns section not present"
- Treat as fail per verdict rule

**Once gathered, each check can be graded without re-reading** the whole package (evidence is recorded).

## Verdict assembly

Apply the verdict rule to the per-check grades:
1. Count required checks: is every one "pass"?
2. If yes → verdict is "accept"
3. If any "fail" or "unclear" → verdict is "reject"
4. Quote the deciding check in the output (the first one that failed, or the most critical pass if all passed)

Output the result as JSON with the deciding check evidence quoted.
