```yaml
number: 754
title: Erroneous F821 on nested class
type: issue
state: closed
author: torarvid
labels:
  - bug
assignees: []
created_at: 2022-11-15T15:45:02Z
updated_at: 2022-11-15T17:19:43Z
url: https://github.com/astral-sh/ruff/issues/754
synced_at: 2026-01-12T15:54:40Z
```

# Erroneous F821 on nested class

---

_@torarvid_

First off: Amazing looking tool. Love the work! 🏆 

Ruff version 0.0.120

Steps to reproduce:

1. Create test.py with the following contents

```python
class RandomClass:
    """This class needs to be present to trigger the bug."""

    def random_func(self):
        """The class also needs a function"""
        pass


class OuterClass:
    class InnerClass:
        pass

    def failing_func(self) -> "InnerClass":
        return self.InnerClass()
```

Then run `ruff test.py`:
```
Found 1 error(s).
test.py:13:31: F821 Undefined name `InnerClass`
```

But if, say, you remove all of `random_func`, the execution seems to succeed 😁 

---

_Comment by @charliermarsh on 2022-11-15 15:59_

Thank you! Weird bug, will try to fix today! :)

---

_Label `bug` added by @charliermarsh on 2022-11-15 16:03_

---

_Closed by @charliermarsh on 2022-11-15 17:19_

---

_Comment by @charliermarsh on 2022-11-15 17:19_

Figured this one out, hopefully fixed in next release but let me know if you hit it again!

---

_Comment by @charliermarsh on 2022-11-15 17:19_

Used your example as a new test case.

---
