# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm contributing to open source as a student in an AI course, working through reproduction and diagnosis. I'm learning the repo's patterns and will ask questions if I need help. Readers should expect specific claims backed by artifacts, honest assessment of what I can and cannot reproduce, and follow-through on what I promise.

## Rules I write by

### Rule: Name the issue and next steps concretely

Each claim comment must identify the specific issue it addresses and state what you will do next. Generic reassurances that could apply to any issue are not specific enough.

- Wrong: "I will help fix this issue."
- Right: "I can reproduce the missing Content-Type header with a single custom header on 3.2.4. Next I want to check the multidict 6.5.0 regression mentioned in the thread."

### Rule: Never assert what the artifacts don't show

If you're showing an error artifact, don't claim it proves a crash. If you're showing a successful run, don't say it failed. State only what the output demonstrates.

- Wrong: "This confirms the race condition: the request completed successfully."
- Right: "The request completed successfully; the race condition hypothesis would predict a timeout here, so this rules it out."

### Rule: Environment comes first in the report

Always lead the repro report with a one-line environment statement (tool version, OS, key dependencies). This is the strangest thing to leave out, and it matters for reproducibility.

- Wrong: "Steps: 1. run `http post pie.dev/post x=1`..."
- Right: "Environment: HTTPie 3.2.4, Python 3.12.4, macOS 14.5. Steps: 1. run `http post pie.dev/post x=1`..."

### Rule: Acknowledge version mismatches

If you're testing a different version than the issue names, say so explicitly instead of glossing over it. This helps readers decide if your result applies.

- Wrong: "Tested on 1.5.3 [no mention of version]"
- Right: "Tested on 1.5.3 (issue reports 2.0); no failure observed, but version delta might matter."

## Things I never post

- Promises I can't keep: "guaranteed fix in 2 days", "I will take ownership of this", timestamps I'm not sure about
- Generic +1 or me-too comments with no new information
- Artifact-free claims about root cause ("I'm 99% sure this is a race condition") without supporting output
- Boilerplate that doesn't name the issue or my specific plan
