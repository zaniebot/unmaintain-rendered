---
number: 13360
title: "macOS: `uv python list` trips mac's \"library validation\" (?) even after offending Python has been removed"
type: issue
state: closed
author: dimaqq
labels:
  - bug
assignees: []
created_at: 2025-05-09T06:38:49Z
updated_at: 2025-05-09T06:42:50Z
url: https://github.com/astral-sh/uv/issues/13360
synced_at: 2026-01-10T01:25:32Z
---

# macOS: `uv python list` trips mac's "library validation" (?) even after offending Python has been removed

---

_Issue opened by @dimaqq on 2025-05-09 06:38_

### Summary

I had a lot of Pythons `uv python install`'ed, among them pypy.

Today, I wanted to check of 3.14t is available, and tried to run `uv python list`, a subcommand that I rarely, if ever, used.

And that triggered the macOS pop-up, that this or that shared object is not trusted:

<img width="533" alt="Image" src="https://github.com/user-attachments/assets/caf6d27b-aa11-466d-83fb-bd815daf814f" />

After I went through `uv python uninstall pypy` (requires dismissing the modal a dozen times), `uv python list` still trips mac protection

<img width="660" alt="Image" src="https://github.com/user-attachments/assets/73f464ab-31b1-4274-8776-80cb8cbbfc59" />

### Platform

Darwin kot.local 24.4.0 Darwin Kernel Version 24.4.0: Fri Apr 11 18:33:46 PDT 2025; root:xnu-11417.101.15~117/RELEASE_ARM64_T8112 arm64

### Version

uv 0.7.3 (Homebrew 2025-05-07)

### Python version

pypy3.10

---

_Label `bug` added by @dimaqq on 2025-05-09 06:38_

---

_Comment by @dimaqq on 2025-05-09 06:42_

It is possible that the pypy that `uv python list` tripped on was what I have installed myself, and not via `uv`.

After `uv cache clean`, `rm -rf /opt/pypy` and other cleanup, the error is gone.

Noting this in case someone else runs into this.

---

_Closed by @dimaqq on 2025-05-09 06:42_

---
