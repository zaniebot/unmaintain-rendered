```yaml
number: 9811
title: linter is not checking for E302 rule
type: issue
state: closed
author: umair313
labels:
  - rule
assignees: []
created_at: 2024-02-04T12:13:48Z
updated_at: 2024-02-05T06:57:28Z
url: https://github.com/astral-sh/ruff/issues/9811
synced_at: 2026-01-12T15:54:49Z
```

# linter is not checking for E302 rule

---

_@umair313_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Ruff version `0.2.0`
or the bellow code following commands are not working, it should fix the space between two functions according to PEP E302 rule.
```
ruff check .
```
```
ruff check --fix .
```
## Code
```
class Node:
    data = None
    next_node = None

    def __init__(self, data):
        self.data = data
    def __repr__(self):
        return f"<Node: {self.data}>"
```

---

_Comment by @charliermarsh on 2024-02-05 00:33_

Thanks for filing! We haven't shipped support for the `E3` rules yet. You can see the Pycodestyle progress here: https://github.com/astral-sh/ruff/issues/2402. And there's an open PR for it here: https://github.com/astral-sh/ruff/pull/9266.

---

_Closed by @charliermarsh on 2024-02-05 00:33_

---

_Label `rule` added by @charliermarsh on 2024-02-05 00:33_

---

_Comment by @umair313 on 2024-02-05 06:57_

@charliermarsh thanks! I've been considering transitioning from using pre-commit with flake8 to pre-commit with ruff. However, upon reflection, it seems this change might not be feasible at the moment.

🚀 I'm eager to contribute to this. Could you advise on how I might be able to assist?

---
