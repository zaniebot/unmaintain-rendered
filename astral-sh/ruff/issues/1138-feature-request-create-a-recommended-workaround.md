```yaml
number: 1138
title: "feature-request: Create a recommended workaround for files with match statements"
type: issue
state: closed
author: j-carson
labels:
  - documentation
assignees: []
created_at: 2022-12-08T04:53:32Z
updated_at: 2023-02-22T00:06:45Z
url: https://github.com/astral-sh/ruff/issues/1138
synced_at: 2026-01-12T15:54:41Z
```

# feature-request: Create a recommended workaround for files with match statements

---

_@j-carson_

Right now, if a file uses the match statement, you have to manually exclude the file from ruff in your environment. It would be better to have an official workaround, such as:

1. Ruff automatically falls back to running flake8 when the match statement is encountered 
2. Providing a way to mark one function as noqa, but still lint the rest of the file
3. Some other workaround I haven't thought of?

The goal is not to have to manually create an appropriate special treatment for each file that is not compatible and to still have some code-quality checks on the offending file

---

_Label `enhancement` added by @charliermarsh on 2022-12-09 04:43_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:15_

---

_Label `documentation` added by @charliermarsh on 2022-12-31 18:15_

---

_Comment by @charliermarsh on 2023-02-22 00:06_

We support `match` statements as of v0.0.250 -- so hopefully no longer an issue!

---

_Closed by @charliermarsh on 2023-02-22 00:06_

---
