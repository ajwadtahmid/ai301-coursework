# Eval package: calib-02

- source: laurent22/joplin#16196
- captured: 2026-08-17
- calibration: true

## Repo facts (captured 2026-08-17)

- repo: laurent22/joplin (55975 stars, archived: no)
- description: Joplin, the privacy-focused note taking app with sync capabilities for Windows, macOS, Linux, Android and iOS.
- latest release: v3.6.15 (2026-06-20)
- bug reports: template asks for the operating system, the Joplin version, desktop version info, current behavior, expected behavior, and logs
- contribution policy (CONTRIBUTING.md): standard contribution guide; no stated AI policy

## Issue

### editor window disappears if search doesn't find the search string (#16196)

opened by sphynxnz256 (NONE) on 2026-08-12, state open, labels: bug

Operating system: Windows. Joplin version: 3.6.15.

When you type something into the search bar, if the thing you type is
not found, the editor window closes until you clear the search bar or
search for text that does exist. The reporter believes the search is
returning a null result Joplin does not know how to handle, points at
laurent22/joplin#15147 as an earlier report of the same behavior that
was closed unfixed, and offers to attempt a fix.

## Thread highlights (0 comments total)

(no comments)

## Candidate claim comment

+1!! this happens to me constantly and it drives me nuts 😩 search is
basically broken, can't believe this has been around since the other
issue and never got fixed. claiming this one, someone has to do it 🙏

## Candidate repro report

Same here!! The editor totally disappears whenever my search comes up
empty, exactly like the issue says. It happens all the time, every
single day, on all my machines. Everyone I know who uses Joplin has
this problem too, so it is definitely not just me. It is obviously the
null result thing the reporter mentioned, that part of the code has
always been flaky. This is a huge usability problem and honestly
should be the top priority for the next release. Please fix it soon,
the app is unusable for heavy search users like me!
