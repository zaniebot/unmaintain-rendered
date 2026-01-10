---
number: 13774
title: "`[tool.uv.sources]` does not override transitive dependencies"
type: issue
state: closed
author: Avasam
labels:
  - bug
assignees: []
created_at: 2025-06-02T01:52:10Z
updated_at: 2025-06-04T03:14:50Z
url: https://github.com/astral-sh/uv/issues/13774
synced_at: 2026-01-10T01:25:38Z
---

# `[tool.uv.sources]` does not override transitive dependencies

---

_Issue opened by @Avasam on 2025-06-02 01:52_

### Summary

Using `uv.sources` to point a dependency to a local wheel file works. But not if that dependency is also another dep's dependency.

Example to replicate on Windows + Python 3.14:

`pyproject.toml`
```py
dependencies = [
  "PyWinCtl >=0.0.42", # Has pywin32 as a dependency
  "pywin32 >=307; sys_platform == 'win32' and python_version < '3.14'", # Python 3.13 support
]
[tool.uv.sources]
# wheel taken from https://github.com/mhammond/pywin32/actions/runs/15353580238
pywin32 = { path = "./scripts/pywin32-310.1-cp314-cp314-win_amd64.whl", marker = "python_version >= '3.14'" } # Python 3.14 support
```

Running `uv sync` results in:
```
Resolved 213 packages in 1ms
error: Distribution `pywin32==310 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.14 (`cp314`), but `pywin32` (v310) only has wheels with the following Python ABI tags: `cp311`, `cp312`, `cp313`
```

In the above, commenting out `PyWinCtl` will work.

### Platform

Windows 10.0.19045 Build 19045

### Version

uv 0.7.9 (13a86a23b 2025-05-30)

### Python version

Python 3.14.0a7

---

_Label `bug` added by @Avasam on 2025-06-02 01:52_

---

_Comment by @oconnor663 on 2025-06-02 17:59_

I'd like to try to reproduce this, but I need a little more hand-holding 😅 Could you tell me exactly how to get that artifact from https://github.com/mhammond/pywin32/actions/runs/15353580238 ?

---

_Comment by @oconnor663 on 2025-06-02 18:38_

Ah, @konstin helped me put together a Linux repro:

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "pandas",
  "numpy; python_version < '3.13'",
]

[tool.uv.sources]
numpy = { url = "https://files.pythonhosted.org/packages/19/49/4df9123aafa7b539317bf6d342cb6d227e49f7a35b99c287a6109b13dd93/numpy-2.2.6-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", marker = "python_version >= '3.13'" }
```

```                                                                                                                        
$ uv run -p 3.12 --with pip python -m pip download pandas
...
$ uv run -p 3.13 --with pip python -m pip download pandas
...
$ rm numpy*cp313*
$ mkdir wheels
$ mv *.whl wheels
$ uv sync -p 3.12 --no-index --find-links wheels
... [works]
$ uv sync -p 3.13 --no-index --find-links wheels
...
error: Distribution `numpy==2.2.6 @ registry+wheels` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.13 (`cp313`), but `numpy` (v2.2.6) only has wheels with the following Python ABI tag: `cp312`
```

Removing the dependency on `pandas` above fixes the last command.

---

_Comment by @zanieb on 2025-06-02 18:41_

This behavior is intentional, afaik. We require all sources to be direct dependencies.

edit: ah I see this is both direct and transitive, sorry I misunderstood

---

_Comment by @Avasam on 2025-06-02 19:57_

> I'd like to try to reproduce this, but I need a little more hand-holding 😅 Could you tell me exactly how to get that artifact from [mhammond/pywin32/actions/runs/15353580238](https://github.com/mhammond/pywin32/actions/runs/15353580238) ?

When you scroll down to the bottom of the page you can download artifacts

![Image](https://github.com/user-attachments/assets/47398e93-c5c1-44cc-9c67-aef83097a626)

But since you now have a linux based repro, that works too :) (and the source can probably be anything, like a url source)

---

_Assigned to @oconnor663 by @oconnor663 on 2025-06-03 16:21_

---

_Comment by @oconnor663 on 2025-06-03 17:07_

Does removing the `python_version` marker from your `[dependencies]` list fix the issue? For example, the NumPy repro above works if I do this (hat tip @konstin again):

```diff
diff --git i/pyproject.toml w/pyproject.toml
index dfc8679..ee7206b 100644
--- i/pyproject.toml
+++ w/pyproject.toml
@@ -4,7 +4,7 @@ version = "0.1.0"
 requires-python = ">=3.12"
 dependencies = [
   "pandas",
-  "numpy; python_version < '3.13'",
+  "numpy",
 ]
 
 [tool.uv.sources]
```

I don't understand all the machinery here yet, but I think the `sources` feature works by "patching" your dependencies, and if the markers/constraints don't match/overlap between `dependencies` and `sources`, that "patch" has no effect?

---

_Comment by @Avasam on 2025-06-03 18:01_

> Does removing the `python_version` marker from your `[dependencies]` list fix the issue? 

@oconnor663 Not only does it work, I didn't even realize that in my minimal example pywin32 isn't installed at all on 3.14

![Image](https://github.com/user-attachments/assets/98413a30-5f31-432f-a4fb-f5f28c3de295)

> I don't understand all the machinery here yet, but I think the `sources` feature works by "patching" your dependencies, and if the markers/constraints don't match/overlap between `dependencies` and `sources`, that "patch" has no effect?

Sounds like that would be it.

---

Idk if there's ways to improve user messaging. Maybe detect and warn if a source in `tool.uv.sources` goes unused (taking into account the marker)? that would probably have tipped me off.

Feel free to close !

---

_Comment by @oconnor663 on 2025-06-03 19:51_

Yes I think adding a warning for cases like this would be a good idea. I'll keep this ticket open and try that today or tomorrow.

---

_Comment by @zanieb on 2025-06-03 23:18_

Just to be clear, I don't think we can unconditionally warn if `tool.uv.sources` is unused at install time — it'd be too noisy. However, I do think it's reasonable to warn if `tool.uv.sources` would _never_ be used. It seems a bit tricky, but maybe it's not so bad. @oconnor663, I'd recommend opening a new issue with a simpler reproduction and dedicated title so we can close this one out.

---

_Referenced in [astral-sh/uv#13829](../../astral-sh/uv/issues/13829.md) on 2025-06-04 03:14_

---

_Comment by @oconnor663 on 2025-06-04 03:14_

Opened https://github.com/astral-sh/uv/issues/13829. Closing this one.

---

_Closed by @oconnor663 on 2025-06-04 03:14_

---
