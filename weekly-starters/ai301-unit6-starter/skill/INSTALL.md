# Installing the voice-guard (the overlay, stated loudly)

This install runs your own voice check before anything you post
through Claude Code. It is
an **overlay onto what you already own**, not a new tool replacing an
old one:

- **Added:** the guard: `check_voice.py` (the checker, with its
  manual check for browser posts) and one hook entry in your Claude Code settings
  (`hooks-settings.json` is the block you merge). Both are
  staff-complete; you write no code.
- **Replaced:** repro-check's and plan-check's `scope.md`, the unit-2
  and unit-3 sandbox scopes for the wild ones
  (`repro-check-scope-wild.md` and `plan-check-scope-wild.md`,
  staff-complete; the Assignment tab has both copy commands). The
  classroom house rules do not travel, and each wild scope states the
  flipped norms for its tool.
- **Promoted:** your voice guide, to one canonical home at
  `~/.claude/voice-guard/voice-guide.md`. The guard reads it there,
  and from today that copy is the guide of record: it is the one you
  edit.
- **Not touched by this install:** the scout (`issue-scout/` and
  everything in it: your rubric, your strategy, your evidence guide;
  you patch the rubric yourself with `rubric-scope-patch.md`), the
  unit-1 harness, and every Beat 1 skill folder in your course
  repo's `tools/` exactly as you submitted it.

The guard never runs in eval mode. Nothing here touches how the
unit-1 regression measures your rubric.

## 1. One guide again

Your voice guide started in unit 2 (`repro-check`) and units 3-4 had
you paste it forward into carry-over slots (`plan-check`,
`pr-precheck`). If you added rules in those copies, fold the newest
text back into `~/.claude/skills/repro-check/voice-guide.md` now, so
one file holds your whole guide. Newest means the later week's copy:
keep every rule any copy added, and where the same rule was reworded
across copies, the later week's wording wins. A guide with empty
unit-2 sections gets them finished now too; the frame at the bottom
of `skill/voice-guide-upstream.md` lists what each section holds.

**No unit-2 guide at all?** Legitimate, and the frame carries you:
copy `skill/voice-guide-upstream.md` to
`~/.claude/skills/repro-check/voice-guide.md` and write every
section fresh (its closing comment restates the unit-2 frame). You
are writing all your rules today instead of extending them; leave
time for it, and ask a TF if you want a second pair of eyes.

## 2. Extend it upstream

Append the template in `skill/voice-guide-upstream.md` (everything
below its opening comment) to your guide, and fill the three
subsections: claims, questions, pings, at least one rule each,
wrong/right pair each. This is the judgment part of the install; the
lecture's drafting moment gave you the first 2-3 rules, and the rest
you write now. Write each rule as plain text under its heading, outside the
comment blocks, in the format the Claims comment shows (`#### Rule:`,
then a Wrong line and a Right line). The guard skips comments when it
checks that your guide has rules, so a rule left inside one may not
count. Once your
rules are in, delete the template's instruction comments: the guide of record carries rules, not
scaffolding. (Started a fresh guide in step 1? It already has the
Upstream section, so skip the append and fill it.)

## 3. The canonical home

From the folder where you unpacked the Unit 6 materials:

```
mkdir -p ~/.claude/voice-guard
cp skill/check_voice.py skill/hooks-settings.json ~/.claude/voice-guard/
[ -L ~/.claude/skills/repro-check/voice-guide.md ] || mv ~/.claude/skills/repro-check/voice-guide.md ~/.claude/voice-guard/voice-guide.md
ln -sf ~/.claude/voice-guard/voice-guide.md ~/.claude/skills/repro-check/voice-guide.md
ln -sf ~/.claude/voice-guard/voice-guide.md ~/.claude/skills/plan-check/voice-guide.md
ln -sf ~/.claude/voice-guard/voice-guide.md ~/.claude/skills/pr-precheck/voice-guide.md
```

The `mv` is the promotion (the `[ -L ... ] ||` in front skips it if
you already ran this block, so running it twice is safe); the `ln` lines leave repro-check,
plan-check, and pr-precheck all reading the same single file (each
skill's live mode reads `voice-guide.md` from its own folder), so
the skills you draft with and the guard can never disagree about your rules. The `-f` lets them
overwrite your frozen carry-over copies, which step 1 already folded
back. If the plan-check or pr-precheck line says "No such file or
directory", that skill isn't installed, so skip that line. `hooks-settings.json` rides along so the canonical home holds
every guard file in one place. (If `ln -s` errors on your
system, instead create each skill's `voice-guide.md` containing the
single line: `My voice guide now lives at ~/.claude/voice-guard/voice-guide.md; read it there.`)

## 4. The hook

Merge the block from `skill/hooks-settings.json` into
`~/.claude/settings.json`:

- No settings file yet: run
  `cp ~/.claude/voice-guard/hooks-settings.json ~/.claude/settings.json`.
  If the file exists but holds only `{}`, replace its contents with
  the whole block.
- Settings but no `"hooks"` key yet: copy the `"hooks": { ... }`
  entry from the file into your settings, with a comma between it
  and the entry before it.
- Already have hooks: add the `PreToolUse` entry from the file into
  your existing `"hooks"` object (or its matcher entry into your
  existing `"PreToolUse"` list), with a comma between it and the
  entry before it.

Save, then check the file. It should print "voice guard hook found"
(an error means the JSON is broken, often a missing comma):

```
python3 -c 'import json, os; s = json.load(open(os.path.expanduser("~/.claude/settings.json"))); print("voice guard hook found" if any("check_voice" in h.get("command", "") for e in s.get("hooks", {}).get("PreToolUse", []) for h in e.get("hooks", [])) else "hook NOT found, check step 4 again")'
```

Then start a fresh `claude` session (settings load at
startup).

One thing said out loud: the hook is user-global. From now on the
guard reads every outgoing comment, issue, or PR body you post
through Claude Code, in every repo, course or not.

## 5. Test that the guard fires

The wiring test should be STOPPED:

```
claude "run this command: gh issue comment --help"
```

Expect the guard to block it with "no body the guard can read"; that
stop IS the pass, proof the hook fires on comment commands before
anything goes out. (Body-less forms are stopped on purpose; real
posts always carry `--body-file`, and a plain terminal still runs
`gh issue comment --help` fine.) If the command runs unblocked
instead: run step 4's check, then replace
`$HOME` in the hook's command with your absolute home directory
path; those two cover nearly every miss.

Once you have a draft (your claim comes later), you can check it
the way the browser path always will:

```
python3 ~/.claude/voice-guard/check_voice.py --draft <your claim draft file>
```

You get the same verdict shape either way: pass, or the exact rule
you broke, quoted.

## 6. The evidence trail

Every check, hook or manual, appends one dated line to
`~/.claude/voice-guard/guard-log.md`. That file is your guard
transcript. It stays on your machine with the guard files and your
canonical guide: the unit-6 Toolkit entry in your contribution
story, in your own words, is what Assignment 5 reads, and the guard
itself is unit-9 portfolio material.
