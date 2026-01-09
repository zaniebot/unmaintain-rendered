---
number: 9295
title: Per-tool file exclusion/inclusion?
type: issue
state: closed
author: ktbarrett
labels:
  - question
assignees: []
created_at: 2023-12-27T23:21:44Z
updated_at: 2023-12-28T06:29:39Z
url: https://github.com/astral-sh/ruff/issues/9295
synced_at: 2026-01-07T13:12:15-06:00
---

# Per-tool file exclusion/inclusion?

---

_Issue opened by @ktbarrett on 2023-12-27 23:21_

I'm looking to add the `pydocstyle` tool to my ruff config, but it checks all Python sources in the repo with this rule. Not all Python code in the repo is public facing: automation, tests, etc. and does not warrant docstrings which generates a ton of errors.

I'd like to run the pydocstyle checks against only one directory, while the other checks should be applied to all Python files. Is there any way to accomplish this? For further reference, I'm using pre-commit to run ruff.

---

_Comment by @zanieb on 2023-12-28 01:48_

Hi!

There's [per-file-ignores](https://docs.astral.sh/ruff/settings/#per-file-ignores) which you can use to accomplish this.

There's been some debate about `per-file-selects` but I can't remember where — the closest I can find right now is https://github.com/astral-sh/ruff/issues/6156

---

_Label `question` added by @zanieb on 2023-12-28 01:48_

---

_Comment by @hauntsaninja on 2023-12-28 05:47_

You could put a ruff.toml in that directory that extends the base configuration

---

_Comment by @ktbarrett on 2023-12-28 06:29_

@hauntsaninja Ah yes, that works!

---

_Closed by @ktbarrett on 2023-12-28 06:29_

---
