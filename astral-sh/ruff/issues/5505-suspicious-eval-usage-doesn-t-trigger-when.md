---
number: 5505
title: "`suspicious-eval-usage` doesn't trigger when expected"
type: issue
state: closed
author: tjkuson
labels:
  - bug
assignees: []
created_at: 2023-07-04T15:57:23Z
updated_at: 2023-07-04T18:01:31Z
url: https://github.com/astral-sh/ruff/issues/5505
synced_at: 2026-01-10T01:22:44Z
---

# `suspicious-eval-usage` doesn't trigger when expected

---

_Issue opened by @tjkuson on 2023-07-04 15:57_

Using ruff `0.0.276`, running `ruff check --select S307 scratch.py` where `scratch.py` is

```python
import os

print(eval("1+1"))
print(eval("os.getcwd()"))
print(eval("os.chmod('%s', 0777)" % 'test.txt'))


# A user-defined method named "eval" should not get flagged.
class Test(object):
    def eval(self):
        print("hi")
    def foo(self):
        self.eval()

Test().eval()
```

flags zero violations. The above Python code is from the [Bandit source](https://github.com/PyCQA/bandit/blob/main/examples/eval.py).

Running bandit flags three violations (as expected).


---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-04 17:49_

---

_Comment by @charliermarsh on 2023-07-04 17:50_

I'll take a look.

---

_Label `bug` added by @charliermarsh on 2023-07-04 17:52_

---

_Referenced in [astral-sh/ruff#5506](../../astral-sh/ruff/pulls/5506.md) on 2023-07-04 17:53_

---

_Closed by @charliermarsh on 2023-07-04 18:01_

---
