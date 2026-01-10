---
number: 7534
title: Add pip shortcut to venv script
type: issue
state: closed
author: PaleNeutron
labels:
  - question
assignees: []
created_at: 2024-09-19T07:28:34Z
updated_at: 2024-09-20T01:54:20Z
url: https://github.com/astral-sh/uv/issues/7534
synced_at: 2026-01-10T01:24:16Z
---

# Add pip shortcut to venv script

---

_Issue opened by @PaleNeutron on 2024-09-19 07:28_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I'm using uv 0.2.15 at ubuntu 22.04 and I found that if I create virtual env with uv venv, and active it with ` source .venv/bin/activate`, the pip command is still global pip.

This will cause a lot of problem if I forget which project's venv is managed by uv. The most common story is installed a new package to venv and not work.

A shortcut of `uv pip` named `pip` to venv is a really vital feature for me.


---

_Comment by @xqm32 on 2024-09-19 07:51_

I just add an alias `pip` to `uv pip` globally in my `.zshrc`, and use `pip3` as the "real" pip. This makes me never incorrectly install some packages outside my venv :-).

Also I use `python` as an alias to `.venv/bin/python3`, for same reason above, and now I don't even need to activate the venv ;-D.

---

_Comment by @weihenglim on 2024-09-19 09:55_

There were some discussion around adding a `pip` shim over on #1331 (TL;DR: probably not happening, `uv pip` is recommended but you can use `uv venv --seed` to seed `pip`)

---

_Label `question` added by @zanieb on 2024-09-19 11:09_

---

_Comment by @zanieb on 2024-09-19 11:09_

@weihenglim's response matches our official recommendation.

---

_Closed by @zanieb on 2024-09-19 11:09_

---

_Referenced in [astral-sh/uv#8354](../../astral-sh/uv/issues/8354.md) on 2024-10-19 02:19_

---

_Referenced in [astral-sh/uv#11550](../../astral-sh/uv/issues/11550.md) on 2025-02-16 05:31_

---

_Referenced in [astral-sh/uv#12604](../../astral-sh/uv/issues/12604.md) on 2025-04-01 17:56_

---
