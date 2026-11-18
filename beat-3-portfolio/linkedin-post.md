I fixed a bug that stopped an open source video tool from rendering its own sample film. The maintainer merged my fix on November 6, 2026.

The tool, html-anime, turns web pages into video. It only worked on scenes with one exact name, so its sample film failed before drawing a frame. I reproduced the problem, changed the code to read each scene's real name, and added a check that the sample film follows the naming rule.

I also learned something about how I work. The tool I built to find beginner-friendly issues kept suggesting ones that someone had already fixed. Once I added a filter for that, I found this one. I used Claude Code, an AI coding assistant, to help trace the cause and write the check, and I wrote the fix myself.

It matters because the project's own example now works for anyone who tries it. It also taught me to keep my fixes small and my proof clear: my fix was merged the day I opened it.

https://github.com/blixvip/html-anime/pull/21

#opensource #softwaredevelopment #ai
