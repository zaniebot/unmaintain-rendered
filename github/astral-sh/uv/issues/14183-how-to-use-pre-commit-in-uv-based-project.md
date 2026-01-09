---
number: 14183
title: How to use pre-commit in uv-based project?
type: issue
state: open
author: dantetemplar
labels:
  - question
assignees: []
created_at: 2025-06-21T12:07:12Z
updated_at: 2025-06-21T21:54:26Z
url: https://github.com/astral-sh/uv/issues/14183
synced_at: 2026-01-07T13:12:18-06:00
---

# How to use pre-commit in uv-based project?

---

_Issue opened by @dantetemplar on 2025-06-21 12:07_

### Question

Just show me how...

I continuously have problems with it... For example, when I hit commit button in Cursor (VsCode):
```
> git -c user.useConfigOnly=true commit --quiet --allow-empty-message --file -
/home/dante/.cache/uv/archive-v0/E42P0RZofT8YmAsd9Qkve/bin/python: No module named pre_commit
```

How to install the pre-commit so python will have access to it? `python -m pre-commit` will be enough, I guess.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @dantetemplar on 2025-06-21 12:07_

---

_Comment by @zanieb on 2025-06-21 13:00_

Perhaps you want to add it as a dev dependency?

Can you share more about your setup? Are you using a `pyproject.toml`, `uv sync`, etc? Are you using `uv pip`? How were you managing `pre-commit` before? How did you install your pre-commit hooks?

---

_Comment by @skprasadu on 2025-06-21 21:54_

I did test this with dev dependency by running this command 

```
uv add pre-commit --dev

uv run pre-commit install

```

And you are done, once installed, as long as you have a .pre-commit-config.yaml at the root of the repository it works, when you pus the code in VSCode.

Let me know if you have any questions.

---
