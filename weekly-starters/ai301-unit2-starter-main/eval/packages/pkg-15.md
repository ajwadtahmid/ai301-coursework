# Eval package: pkg-15

- source: laurent22/joplin#16205
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: laurent22/joplin (55975 stars, archived: no)
- description: Joplin, the privacy-focused note taking app with sync capabilities for Windows, macOS, Linux, Android and iOS.
- latest release: v3.6.15 (2026-06-20)
- bug reports: template asks for the operating system, the Joplin version, desktop version info, current behavior, expected behavior, and logs
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### Desktop: Switching note immediately after changing the text will lose the text, when using the Rich Text editor (#16205)

opened by mrjo118 (COLLABORATOR) on 2026-08-13, state open, labels: bug, high, Regression, v3.7

Operating system: Windows. Joplin version: 3.7.10.

Reproduction steps: open Joplin and select the Rich Text editor; type
a character, or a continuous stream of characters very quickly without
stopping, then immediately switch to another note in the note list
(before the toggle between RTE and MDE button is re-enabled); wait a
second or two; switch back to the original note. The last change is
missing, but an update is still uploaded to the server. If the note is
instead opened in a new window and that window is closed immediately
after making a change this quickly, no changes are lost.

The issue does not occur in Joplin 3.6. A screen recording is attached
to the issue. Expected: all changes are saved when switching notes
while using the RTE, the same way closing a note window saves them.

## Thread highlights (0 comments total)

(no comments)

## Candidate claim comment

Hi, I'd like to work on this data-loss regression. I've already
reproduced it and tracked down the root cause (see my report below),
so I should be able to have a fix approach to discuss soon.

## Candidate repro report

I reproduced this bug and identified the exact root cause.

Environment: Joplin 3.7.10, Windows 11.

The problem is a race condition between the Rich Text editor's
debounced save and the note-switch handler. The RTE schedules its save
on a debounce timer, and when you switch notes quickly, `loadNote`
cancels the pending save before it fires, which is why the last
keystrokes vanish. The new-window path is unaffected because closing a
window flushes the editor synchronously. I verified this race
condition is the cause; the fix is to flush the pending save in the
switch handler before the new note loads, the same flush the window
close path already does.

Expected: the last change survives a fast note switch.

Actual: the change is lost, exactly as the issue describes, because
the debounced save is cancelled by the switch. I am confident this is
the correct diagnosis.
