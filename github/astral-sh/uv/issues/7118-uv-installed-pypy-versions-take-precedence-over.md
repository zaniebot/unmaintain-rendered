---
number: 7118
title: uv-installed PyPy versions take precedence over non-uv CPython versions
type: issue
state: closed
author: hynek
labels:
  - bug
  - breaking
  - uv python
assignees: []
created_at: 2024-09-06T06:18:23Z
updated_at: 2024-09-24T17:52:16Z
url: https://github.com/astral-sh/uv/issues/7118
synced_at: 2026-01-07T13:12:17-06:00
---

# uv-installed PyPy versions take precedence over non-uv CPython versions

---

_Issue opened by @hynek on 2024-09-06 06:18_

`uv 0.4.6 (84f25e8cf 2024-09-05)`

I've been playing a bit with `uv python` and installed pypy3.9 with it and forgot about it.

Now, whenever I run `uv venv --python python3.9` I get pypy3.9 instead. It just says `Using Python 3.9.19`.

My guess is that uv-installed Pythons take hard precedence, because uninstalling it starts giving me my proper Python 3.9 from homebrew, despite the fact I still have a pyp3.9 installed, too.

---

_Renamed from "uv-installed PyPy versions take precedence over Python versions" to "uv-installed PyPy versions take precedence over non-uv CPython versions" by @hynek on 2024-09-06 06:29_

---

_Comment by @zanieb on 2024-09-06 13:21_

Hm yeah we'll use a managed Python over an unmanaged one. You think we should prefer the system CPython in this case? You can do `--python cpython3.9` or `--python cp39` to avoid getting PyPy. You can also use `--python-preference system`.

---

_Label `uv python` added by @zanieb on 2024-09-06 13:21_

---

_Comment by @hynek on 2024-09-06 13:25_

Hmmm, fair enough for me, but kinda surprising. I would've never expected to get a PyPy when asking for "Python 3.9". IDK, maybe I'm just set in my ways, by in my mind python3.9 is CPython and pypy3.9 is PyPy.

---

_Comment by @zanieb on 2024-09-06 13:38_

Yeah that makes sense. We should probably change the ordering, though it'll be a bit of a pain.

---

_Label `bug` added by @zanieb on 2024-09-06 13:39_

---

_Label `breaking` added by @zanieb on 2024-09-06 13:39_

---

_Comment by @henryiii on 2024-09-06 20:45_

> I would've never expected to get a PyPy when asking for "Python 3.9". IDK, maybe I'm just set in my ways, by in my mind python3.9 is CPython and pypy3.9 is PyPy.

I've argued this before too. :)

---

_Referenced in [astral-sh/uv#7286](../../astral-sh/uv/issues/7286.md) on 2024-09-12 15:11_

---

_Comment by @zanieb on 2024-09-12 15:17_

Oh I might be fixing this by accident while working on #7286 haha

---

_Assigned to @zanieb by @zanieb on 2024-09-15 23:28_

---

_Referenced in [astral-sh/uv#7508](../../astral-sh/uv/pulls/7508.md) on 2024-09-18 16:39_

---

_Comment by @saucoide on 2024-09-23 00:23_

To add a datapoint, i'm just running into this and also found the behavior unexpected, especially because the output of a command depends on what other uv-managed pythons were previously installed

```sh
uv python install python3.8 pypy3.8
uv venv --python python3.8 .venv  # venv created with cpython 3.8
```

vs

```sh
uv python install pypy3.8
uv venv --python python3.8 .venv  # venv created with pypy 3.8
```

Also this produces different results whether run together or separately:

```sh
uv python install pypy3.8 python3.8  # installs both cpython & pypy

uv python install pypy3.8
uv python install python3.8  # wont install anything -> already found a 3.8 python pypy

```


---

_Referenced in [wntrblm/nox#842](../../wntrblm/nox/pulls/842.md) on 2024-09-23 23:07_

---

_Referenced in [astral-sh/uv#7650](../../astral-sh/uv/pulls/7650.md) on 2024-09-23 23:40_

---

_Closed by @zanieb on 2024-09-24 17:52_

---
