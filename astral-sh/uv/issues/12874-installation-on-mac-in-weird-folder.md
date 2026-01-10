---
number: 12874
title: Installation on mac in weird folder
type: issue
state: open
author: htran-ubed
labels:
  - question
assignees: []
created_at: 2025-04-14T08:37:12Z
updated_at: 2025-04-14T09:53:02Z
url: https://github.com/astral-sh/uv/issues/12874
synced_at: 2026-01-10T01:25:26Z
---

# Installation on mac in weird folder

---

_Issue opened by @htran-ubed on 2025-04-14 08:37_

### Question

I used the curl installation method on my mac. It installed to /Users/ubed/.local/bin. This does not seem to be a default folder. I got the "command not found" and had to add this location to PATH.

Why does the uv installation not choose a bin folder that is already in system or user path?

### Platform

macOS arm64

### Version

uv 0.6.14

---

_Label `question` added by @htran-ubed on 2025-04-14 08:37_

---

_Comment by @konstin on 2025-04-14 08:54_

uv follows the XDG specification for its installation, cache and data folders on all unix systems. The curl installer show add `~/.local/bin` to your `PATH` in your shell configuration, so uv should be in `PATH` after restarting the shell. What shell are you using?

---

_Comment by @htran-ubed on 2025-04-14 09:53_

I'm using macOS default terminal. It did not add the path automatically for me.

---
