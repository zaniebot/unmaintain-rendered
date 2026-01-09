---
number: 12322
title: "`uv pip autoupdate`?"
type: issue
state: open
author: gtkacz
labels:
  - question
assignees: []
created_at: 2025-03-19T19:21:10Z
updated_at: 2025-04-06T23:07:11Z
url: https://github.com/astral-sh/uv/issues/12322
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip autoupdate`?

---

_Issue opened by @gtkacz on 2025-03-19 19:21_

### Question

Is there a way to run a command through `uv` to "autoupdate" (bump to the latest version conforming to the constraints, like what `dependabot` does) in a `pip` environment?

### Platform

Linux 5.15.167.4-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.6.8 (c1ef48276 2025-03-18)

---

_Label `question` added by @gtkacz on 2025-03-19 19:21_

---

_Comment by @FishAlchemist on 2025-03-20 11:29_

I believe this request is associated with this issue
* https://github.com/astral-sh/uv/issues/1419

---

_Comment by @notatallshaw on 2025-03-20 16:11_

FWIW, if your using a pip workflow, this is the purpose of the `uv pip compile`: 

1. Define your unpinned requirements in a `requirements.in`
2. Compile them to a `requirements.txt`, e.g. `uv pip compile --upgrade requirements.in -o requirements.txt`
3. Sync your environment with the upgraded requirements `uv pip sync requirements.txt`

You can write a small script to turn this into a single command, with various options you may prefer. I beleive `uv pip compile` also supports implicitly extracting the requirements out of a `pyproject.toml`.

---

_Comment by @gtkacz on 2025-03-24 16:28_

@notatallshaw thing is, I need _some_ of the dependencies pinned due to vulnerabilities detected by Snyk

---

_Comment by @notatallshaw on 2025-03-24 17:03_

> [@notatallshaw](https://github.com/notatallshaw) thing is, I need _some_ of the dependencies pinned due to vulnerabilities detected by Snyk

In the workflow I describe above all dependencies and transitive dependencies  are pinned in the `requirements.txt`, if you need to constrain what versions a particular package can be pinned to then you define those constraints in the `requirements.in`.




---

_Comment by @konstin on 2025-03-28 18:02_

> [@notatallshaw](https://github.com/notatallshaw?rgh-link-date=2025-03-24T16%3A28%3A49.000Z) thing is, I need _some_ of the dependencies pinned due to vulnerabilities detected by Snyk

How do you currently define what you install in the venv, and how does snyk interact with that?

---

_Comment by @gtkacz on 2025-03-28 18:32_

@konstin Snyk itself will create PRs pinning versions to avoid vulnerabilites, e.g.: https://github.com/gtkacz/temporal_adjusters_py/pull/34/files

---

_Comment by @konstin on 2025-03-28 18:36_

You can use `uv pip install --upgrade requirements_dev.txt`, this will update all direct dependencies listed and their transitive dependencies, too.

---

_Comment by @gtkacz on 2025-03-28 21:02_

@konstin that will do it only on the env, no? I was looking for a command that would update the requirements file itself

---

_Comment by @PaintOnBrush on 2025-04-06 22:25_

what about something like `uv pip install --upgrade -r <(uv pip freeze)`

---

_Comment by @gtkacz on 2025-04-06 22:34_

@PaintOnBrush is that writing the updated to the requirement file? 🤔 

---

_Comment by @PaintOnBrush on 2025-04-06 23:07_

It uses the stdout from the process directly as the file.

---
