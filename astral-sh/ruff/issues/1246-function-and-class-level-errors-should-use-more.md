```yaml
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
synced_at: 2026-01-12T15:54:41Z
```

# Function- and class-level errors should use more specific spans

---

_@charliermarsh_

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

_Comment by @charliermarsh on 2022-12-15 02:40_

(We probably need a method to take a `FunctionDef` or `ClassDef` and extract the span of its name, which will require tokenizing.)

---

_Closed by @charliermarsh on 2022-12-15 03:40_

---
