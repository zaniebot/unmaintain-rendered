```yaml
number: 4417
title: Indented-block checks fail to respect inline comment
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-05-13T16:07:43Z
updated_at: 2023-05-16T14:51:25Z
url: https://github.com/astral-sh/ruff/issues/4417
synced_at: 2026-01-10T11:09:47Z
```

# Indented-block checks fail to respect inline comment

---

_Issue opened by @charliermarsh on 2023-05-13 16:07_

Given:

```py
def main() -> None:  # Comment
    print("Retrieving data...")


def main() -> None: 
# Comment
    print("Retrieving data...")


def main() -> None: 
    # Comment
    print("Retrieving data...")


def main() -> None: 
    print("Retrieving data...")
```

We report:

```
foo.py:1:1: E112 Expected an indented block
foo.py:2:1: E113 Unexpected indentation
foo.py:6:1: E115 Expected an indented block (comment)
```

Meanwhile, pycodestyle reports:

```
foo.py:6:1: E115 expected an indented block (comment)
```

---

_Label `bug` added by @charliermarsh on 2023-05-13 16:07_

---

_Comment by @charliermarsh on 2023-05-13 16:07_

\cc @MichaReiser in case there's an obvious fix.

---

_Comment by @charliermarsh on 2023-05-13 16:09_

I can probably fix this, good for me to make sure I understand the updated code.

---

_Closed by @charliermarsh on 2023-05-16 14:51_

---
