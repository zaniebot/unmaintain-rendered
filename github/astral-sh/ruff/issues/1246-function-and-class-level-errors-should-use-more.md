---
number: 1246
title: Function- and class-level errors should use more specific spans
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-12-15T02:25:14Z
updated_at: 2022-12-15T03:40:02Z
url: https://github.com/astral-sh/ruff/issues/1246
synced_at: 2026-01-07T13:12:14-06:00
---

# Function- and class-level errors should use more specific spans

---

_Issue opened by @charliermarsh on 2022-12-15 02:25_

Right now, they use the _entire_ function (or class). They should use the `def` or `class`, or maybe the function or class _name_? Should look at what some other tools do.

---

_Label `enhancement` added by @charliermarsh on 2022-12-15 02:25_

---

_Comment by @charliermarsh on 2022-12-15 02:38_

Yeah, I think these should use the name.

---

_Comment by @charliermarsh on 2022-12-15 02:38_

See: https://github.com/charliermarsh/vscode-ruff/issues/56.

---

_Referenced in [astral-sh/ruff-vscode#56](../../astral-sh/ruff-vscode/issues/56.md) on 2022-12-15 02:38_

---

_Comment by @charliermarsh on 2022-12-15 02:40_

(We probably need a method to take a `FunctionDef` or `ClassDef` and extract the span of its name, which will require tokenizing.)

---

_Referenced in [astral-sh/ruff#1247](../../astral-sh/ruff/pulls/1247.md) on 2022-12-15 03:39_

---

_Closed by @charliermarsh on 2022-12-15 03:40_

---
