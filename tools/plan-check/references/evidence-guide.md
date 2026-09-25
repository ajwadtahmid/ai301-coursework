# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives**: 
- Plan's Diagnosis section states the root cause
- Repro evidence section shows what behavior was actually observed (the failing symptom, the error, the output)

**What good looks like**: 
The stated cause directly explains the observed behavior without contradiction. For example: if the repro shows "ZeroDivisionError at line 52 during corpus_size division," the diagnosis should identify that calculation and explain why it fails on empty input. A diagnosis that says "tokenizer is too strict" when the repro shows "division by zero" does not ground.

## Scope

**Where it lives**: 
- Plan's "In scope" statement names what will be changed
- Plan's "Not in scope" statement names related work that is explicitly excluded
- Plan's Files list names specific files to modify

**What good looks like**: 
Scope is clear and bounded. "In scope: fix line 25 guard, remove xfail test" is bounded. "In scope: improve the keyword search" is not (vague). "Not in scope: argparse" shows awareness of related-but-separate problems. Scope creep (adding unrelated tasks like "while we're here, refactor the tokenizer") is absent.

## Executability

**Where it lives**: 
- Plan's Files section lists specific file paths
- Plan's Approach section lists concrete steps in order (not stages or phases)
- Plan's Test plan section names specific test commands or observable verification steps

**What good looks like**: 
A stranger could follow the plan. "Add guard at line 17-26 that checks if chunks is empty" is followable. "Fix the method" is not. "Run pytest on test_keyword_search.py" is followable. "Run tests" is not.

## Test plan

**Where it lives**: 
- Plan's Test plan section
- Repro evidence's steps section (what was failing)

**What good looks like**: 
The test plan directly re-runs the scenario the repro evidence showed was failing, or runs a test that captures that scenario. The plan says what will change (output, exit code, test result) in observable terms. "Before: ZeroDivisionError raised, exit 2. After: index([]) succeeds, search returns [], exit 0" is decisive. "Verify tests pass" is vague.

## Honesty

**Where it lives**: 
- Plan's "Unknowns and risks" section (or note if absent)
- Plan.md's deviations section (filled only if build discovered something unexpected)

**What good looks like**: 
Risks and unknowns are named plainly. "Risk: None identified" is honest. "Risk: This is straightforward" is false confidence (not a risk). "Unknown: whether the pattern works on all Python versions" is honest. Deviation section shows honest mid-build change: "Discovered X during testing, updated plan to Y, here's why."

## Comms

**Where it lives**: 
- Plan comment (candidate comment in the package)
- Thread highlights (what maintainers or other contributors said)
- Repo facts contribution policy (AI-use disclosure requirement if stated)

**What good looks like**: 
The plan comment references the specific issue symptoms, not generic language. "The ZeroDivisionError at line 52 when passing empty chunks" is specific. "+1, will fix" is not. If a maintainer suggested a direction in the thread, the comment acknowledges it. If the repo requires AI-use disclosure, the comment discloses. The comment avoids boilerplate that could apply to any issue.
