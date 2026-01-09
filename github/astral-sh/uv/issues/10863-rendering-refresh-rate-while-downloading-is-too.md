---
number: 10863
title: Rendering refresh rate while downloading is too high
type: issue
state: open
author: Marco-Sulla
labels:
  - bug
  - cli
assignees: []
created_at: 2025-01-22T16:04:24Z
updated_at: 2025-09-16T02:07:27Z
url: https://github.com/astral-sh/uv/issues/10863
synced_at: 2026-01-07T13:12:18-06:00
---

# Rendering refresh rate while downloading is too high

---

_Issue opened by @Marco-Sulla on 2025-01-22 16:04_

### Summary

While `uv` downloads packages, the refresh rate is so high that the text blinks in a very unpleasant way for the eyes.

I'm using `uv` on Windows, in a `cmder` command line. I reproduced it also on Linux, bash.

### Platform

Windows 11 x86_64

### Version

uv 0.3.2

### Python version

Python 3.9

---

_Label `bug` added by @Marco-Sulla on 2025-01-22 16:04_

---

_Label `cli` added by @zanieb on 2025-01-22 16:16_

---

_Comment by @zanieb on 2025-01-22 16:16_

Can you please confirm this reproduces on the latest version of uv?

---

_Comment by @Marco-Sulla on 2025-01-22 17:13_

I confirm I can reproduce this on latest version (0.5.22)

---

_Comment by @charliermarsh on 2025-01-22 21:04_

@konstin -- weren't you looking at this?

---

_Comment by @zanieb on 2025-01-22 21:44_

I think @konstin was looking at it from a perf perspective https://github.com/astral-sh/uv/issues/10384

---

_Comment by @konstin on 2025-01-23 10:02_

In the rustrover terminal i see the flickering too, due to the many multiline changes of the progress bars, though not in the system terminal.

It would make sense (though not required) to combine reducing the overall refresh together with fixing the performance.

---

_Comment by @mkrieger1 on 2025-09-15 08:46_

I believe I also have this issue with the Ghostty terminal when using `uv sync` (in uv version 0.7.20).

According to the Ghostty developers, this is usually caused by the applications emitting terminal control codes too frequently which may not be visible in "slow" terminals but can cause flickering or tearing in "fast" terminals:

- https://github.com/ghostty-org/ghostty/discussions/4048
- https://github.com/ghostty-org/ghostty/discussions/4247
- https://github.com/ghostty-org/ghostty/discussions/8162
- https://github.com/ghostty-org/ghostty/discussions/8285

Supposedly, the solution is that applications implement "synchronized output": https://gist.github.com/christianparpart/d8a62cc1ab659194337d73e399004036

I'm not sure if uv, or any library it uses for creating the terminal output, is doing that.

---

_Comment by @zanieb on 2025-09-16 02:07_

Interesting. Yeah, we don't handle something like that ourselves, we'd need to go down the stack to the libraries we're using for the progress bars.

---
