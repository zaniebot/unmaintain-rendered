```yaml
number: 2649
title: "[`pylint`]: duplicate-argument-name"
type: pull_request
state: closed
author: colin99d
labels: []
assignees: []
draft: true
base: main
head: duplicate-argument-name
created_at: 2023-02-08T00:58:44Z
updated_at: 2023-02-08T12:41:15Z
url: https://github.com/astral-sh/ruff/pull/2649
synced_at: 2026-01-12T04:52:00Z
```

# [`pylint`]: duplicate-argument-name

---

_Pull request opened by @colin99d on 2023-02-08 00:58_

@charliermarsh, I have a quick question for you. This lint is supposed to detect duplicate arguments in a function. This is a SyntaxError in python:
```
>>> def a(x, y, x):
...     pass
...
  File "<stdin>", line 1
SyntaxError: duplicate argument 'x' in function definition
```

The syntax error makes Ruff return this: 
`error: Failed to parse crates/ruff/resources/test/fixtures/pylint/duplicate_argument_name.py: duplicate argument 'apple' in function definition at line 1 column 30`

Is there any way to get around this, so we can still deliver feedback to the user?

---

_Comment by @charliermarsh on 2023-02-08 02:30_

Ah yeah, we should just skip this rule IMO.

---

_Closed by @colin99d on 2023-02-08 12:41_

---
