---
number: 16551
title: uv cache prune not pruning?
type: issue
state: closed
author: sergiocallegari
labels:
  - question
assignees: []
created_at: 2025-11-02T09:38:04Z
updated_at: 2025-11-03T20:32:57Z
url: https://github.com/astral-sh/uv/issues/16551
synced_at: 2026-01-07T13:12:19-06:00
---

# uv cache prune not pruning?

---

_Issue opened by @sergiocallegari on 2025-11-02 09:38_

### Summary

Tried to install a package that brings in tons of dependencies with `uv tool`. I did `uv tool install openai-whisper` that brings in several GBs of CUDA even on non-nvidia hardware. Once done with it, I uninstalled the tool. However there seems to be no way to get rid of the nvidia stuff from the cache with `uv cache prune`. The detection logic says that there is nothing "unused".

### Platform

Manjaro linux X86-64

### Version

uv 0.9.7

### Python version

python 3.13.9 (managed)

---

_Label `bug` added by @sergiocallegari on 2025-11-02 09:38_

---

_Comment by @charliermarsh on 2025-11-02 17:56_

`uv cache prune` does _not_ remove dependencies that are no longer used in your project (we don't track usages like that). It removes other ephemeral resources from the cache.

---

_Label `bug` removed by @samypr100 on 2025-11-03 03:35_

---

_Label `question` added by @samypr100 on 2025-11-03 03:35_

---

_Closed by @charliermarsh on 2025-11-03 16:53_

---

_Comment by @sergiocallegari on 2025-11-03 20:32_

The manual says "uv cache prune removes all *unused* cache entries", from which I probably came to wrong conclusions. Sorry for the noise. It then talks about "unreachable objects". My understanding was that files in the cache get hard-linked into the virtual environments created by the projects and by the tools and that when the link count goes back to 1 the items are in the cache only and can be removed. Is this understanding wrong? Is there some doc where "unreachable" is better clarified?



---
