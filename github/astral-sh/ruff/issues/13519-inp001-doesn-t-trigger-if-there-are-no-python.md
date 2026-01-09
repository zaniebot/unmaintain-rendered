---
number: 13519
title: "`INP001` doesn't trigger if there are no Python files in the immediate directory"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-09-26T01:14:08Z
updated_at: 2024-11-10T03:03:35Z
url: https://github.com/astral-sh/ruff/issues/13519
synced_at: 2026-01-07T13:12:15-06:00
---

# `INP001` doesn't trigger if there are no Python files in the immediate directory

---

_Issue opened by @charliermarsh on 2024-09-26 01:14_

If you have a directory within your project that's missing an `__init__.py` file, but it only contains Python modules (directories with `__init__.py` files), and no Python _files_, we won't catch it in the [implicit namespace package](https://docs.astral.sh/ruff/rules/implicit-namespace-package/) rule.

---

_Label `bug` added by @charliermarsh on 2024-09-26 01:14_

---

_Comment by @charliermarsh on 2024-09-26 01:27_

This is a pretty substantial limitation. It looks non-trivial to fix though, since we rely on `package` being `None` for this detection.

---

_Comment by @MichaReiser on 2024-09-26 06:22_

Hmm, I think there's a similar issue somewhere

---

_Comment by @MichaReiser on 2024-09-26 06:23_

Here, https://github.com/astral-sh/ruff/issues/13225

Folders without an `__init__.py` are considered namespace packages which Ruff doesn't support.

---

_Comment by @charliermarsh on 2024-09-28 12:40_

We do support them but you have to mark them explicitly.

---

_Comment by @charliermarsh on 2024-09-28 12:41_

(The whole point of the rule is to find implicit namespace packages though.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 22:10_

---

_Referenced in [astral-sh/ruff#14236](../../astral-sh/ruff/pulls/14236.md) on 2024-11-10 01:26_

---

_Closed by @charliermarsh on 2024-11-10 03:03_

---
