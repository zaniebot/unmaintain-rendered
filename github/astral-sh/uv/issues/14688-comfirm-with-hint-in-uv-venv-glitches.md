---
number: 14688
title: "Comfirm with hint in `uv venv` glitches"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2025-07-17T19:30:19Z
updated_at: 2025-07-17T20:52:56Z
url: https://github.com/astral-sh/uv/issues/14688
synced_at: 2026-01-07T13:12:18-06:00
---

# Comfirm with hint in `uv venv` glitches

---

_Issue opened by @konstin on 2025-07-17 19:30_

### Summary

When I run `uv venv` and press enter, the output glitches: There is still content in the current line, even though the command is over and I should have a clear new prompt.

<img width="841" height="168" alt="Image" src="https://github.com/user-attachments/assets/493a02c1-036a-49e1-93ce-395c89e72522" />

```
home ~/projects/uv ⊤  ../uv/target/release/uv venv 
Using CPython 3.13.2
Creating virtual environment at: .venv
✔ A virtual environment already exists at `.venv`. Do you want to replace it? · yes
Activate with: source .venv/bin/activate
home ~/projects/uv ⊤  ` flag or set `UV_VENV_CLEAR=1` to skip this prompt
```

### Platform

Ubuntu 24.04, stock terminal

### Version

0.8.0

### Python version

n/a

---

_Assigned to @jtfmumm by @konstin on 2025-07-17 19:30_

---

_Label `bug` added by @konstin on 2025-07-17 19:30_

---

_Comment by @zanieb on 2025-07-17 19:34_

Interesting, I tested this quite a bit. Is `clear_lines` not working on your system? Are you doing anything weird?

---

_Comment by @zanieb on 2025-07-17 19:35_

(I believe this is "my" change, in https://github.com/astral-sh/uv/pull/14667 I added a new "hint" to the prompt)

---

_Unassigned @jtfmumm by @konstin on 2025-07-17 19:36_

---

_Comment by @zanieb on 2025-07-17 19:38_

I only tested it on macOS though (I use Kitty)

---

_Comment by @konstin on 2025-07-17 19:40_

It's a mostly stock terminal with a custom prompt style. It happens both in Gnome Terminal and in the RustRover terminal (old and new). It happens with bash, but it doesn't happen with zsh or fish. It does also happen in a bash without any configuration (`bash --noprofile --norc`):

<img width="841" height="194" alt="Image" src="https://github.com/user-attachments/assets/382ef748-2910-4343-a2f2-3ae1264b559f" />

---

_Comment by @zanieb on 2025-07-17 20:39_

Okay, I can reproduce in `bash`.

---

_Referenced in [astral-sh/uv#14691](../../astral-sh/uv/pulls/14691.md) on 2025-07-17 20:44_

---

_Comment by @MeGaGiGaGon on 2025-07-17 20:50_

I can also reproduce on git bash/powershell/cmd, but only if I set the prompt to something without a newline (`export PS1="$"` in git bash, `function prompt() {}` in powershell, `prompt -` in cmd). Using the binary from #14691 fixes it for me on all three.

---

_Comment by @zanieb on 2025-07-17 20:52_

<3 thank you for testing!

---

_Closed by @konstin on 2025-07-17 20:52_

---

_Referenced in [astral-sh/uv#14950](../../astral-sh/uv/issues/14950.md) on 2025-07-29 09:33_

---
