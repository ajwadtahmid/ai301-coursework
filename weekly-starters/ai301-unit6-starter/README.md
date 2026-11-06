# AI301 Unit 6 materials: the voice guard and repo memory

Materials for Unit 6 of AI301 (working with maintainers). The steps
are on the course portal's Unit 6 Assignment tab. This repo holds the
files those steps install.

## What's here

- `skill/INSTALL.md`: the voice guard install, steps 1 to 6. The
  Assignment tab has you do steps 1 to 5. Step 6 explains the guard's
  log.
- `skill/check_voice.py` and `skill/hooks-settings.json`: the voice
  guard. You write no code. The hook checks what you post through
  Claude Code against your voice guide, and
  `check_voice.py --draft FILE` runs the same check on a draft you
  post from the browser.
- `skill/voice-guide-upstream.md`: the Upstream section for your voice
  guide (claims, questions, pings). You write the rules. If you're
  starting a voice guide from scratch, its closing comment lists the
  unit 2 sections too.
- `skill/repro-check-scope-wild.md` and `skill/plan-check-scope-wild.md`:
  the real-repo scopes that replace repro-check's and plan-check's
  sandbox scopes.
- `skill/rubric-scope-patch.md`: the scope patch for your scout's
  rubric. The scope check rates how hard an issue looks instead of
  rejecting every open question. Issues that aren't one piece of work
  still fail.
- `skill/claude-local-template.md`: the repo memory template. You copy
  it into your clone of the repo you're contributing to as
  `CLAUDE.local.md` and follow it top to bottom.

## Get the materials

```
git clone https://github.com/codepath/ai301-unit6-starter.git
```

Or click **Code**, then **Download ZIP** on this page. Then follow the
Assignment tab.

The install adds to what you already have. It adds the guard, replaces
repro-check's and plan-check's scopes, and moves your voice guide to
one home. It doesn't touch your scout (you patch its rubric yourself,
with `rubric-scope-patch.md`) or the Beat 1 skill folders in your
course repo's `tools/`.

## No eval/ here

Unit 6 ships no eval set. To check that the scope patch didn't break
your rubric's agreement with the gold labels, run Unit 1's harness
from the `eval/` folder of your
[Unit 1 materials](https://github.com/codepath/ai301-unit1-starter).
