```yaml
number: 6448
title: "Support `uv tool upgrade --python <PYTHON>`"
type: pull_request
state: closed
author: j178
labels:
  - uv tool
assignees: []
draft: true
base: main
head: upgrade-python
created_at: 2024-08-22T16:57:43Z
updated_at: 2024-09-26T01:28:32Z
url: https://github.com/astral-sh/uv/pull/6448
synced_at: 2026-01-12T16:07:22Z
```

# Support `uv tool upgrade --python <PYTHON>`

---

_@j178_

Closes #6297

---

_Comment by @zanieb on 2024-08-22 17:10_

We'll need to document this at https://docs.astral.sh/uv/concepts/tools/

Can you write some details here about the strategy for this?

I wonder if we should be upgrading Python versions by default during upgrade (unless the user requested a specific version during install).

---

_Label `uv tool` added by @zanieb on 2024-08-22 17:12_

---

_Comment by @j178 on 2024-08-23 05:34_

> need to document this at https://docs.astral.sh/uv/concepts/tools/

sure, will do

> upgrading Python versions by default during upgrade

I'm not very familiar with this codebase, could you provide some guidance?
I wonder is there a chicken-egg problem, how do we decide which Python version to upgrade to?

---

_Comment by @zanieb on 2024-08-23 13:09_

For example, if there's a Python version that's _newer_ than the one the tool is installed with, we reinstall the tool with the latest Python version we can find. We'd probably need to be careful to keep a tool on a managed or system Python e.g. do not switch from one to the other automatically. Does that make sense? One problem is we might not be able to solve in environment, in which case we'd have to bail on it? And we'd have to do this on every attempted upgrade? Eek. ~Definitely can be considered separately from this change.~ Actually, this might be important for this change because we might require `uv tool install` again if you want to change Python versions?

---

_Comment by @charliermarsh on 2024-09-26 01:28_

I believe this was covered by #7605.

---

_Closed by @charliermarsh on 2024-09-26 01:28_

---
