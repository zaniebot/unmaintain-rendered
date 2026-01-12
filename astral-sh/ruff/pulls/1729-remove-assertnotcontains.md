```yaml
number: 1729
title: "Remove `assertNotContains`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: remove-assertNotContains
created_at: 2023-01-08T01:11:26Z
updated_at: 2023-01-08T05:21:19Z
url: https://github.com/astral-sh/ruff/pull/1729
synced_at: 2026-01-12T15:55:07Z
```

# Remove `assertNotContains`

---

_@harupy_

`unittest.TestCase` doens't have a method named `assertNotContains`.

---

_Comment by @charliermarsh on 2023-01-08 01:32_

Any idea why this was included initially?

---

_Comment by @harupy on 2023-01-08 01:36_

It was included in https://github.com/charliermarsh/ruff/pull/1506/files#diff-63acef36182d791074bf61c44d3b9715f8783e11f6314869597ea761cdd48d0aR82. cc @edgarrmondragon 

---

_Comment by @harupy on 2023-01-08 01:38_

To confirm, the following code prints out False

```python
import unittest

print("assertNotContains" in dir(unittest.TestCase))
```

---

_Comment by @andersk on 2023-01-08 01:48_

Might be a Django thing? https://docs.djangoproject.com/en/4.1/topics/testing/tools/#django.test.SimpleTestCase.assertNotContains

---

_Comment by @harupy on 2023-01-08 01:57_

@andersk Thanks for the comment!

---

_Comment by @harupy on 2023-01-08 02:16_

If `assertNotContains` was Django's `assertNotContains`, would it make sense to emit PT009? I guess not.

---

_Comment by @charliermarsh on 2023-01-08 03:13_

Alright I did some digging here. It looks like `flake8-pytest-style` ported this from `flake8-pytest`. `flake8-pytest` originally included Django's `SimpleTestCase` methods, then removed them [here](https://github.com/vikingco/flake8-pytest/commit/8f44e4f474bcf9f2979a2684cec750d19a17504a), but left `assertNotContains`. I'm guessing that was an oversight? So, yes, we should remove it.

---

_Comment by @harupy on 2023-01-08 03:14_

@charliermarsh Thanks for the investigation! Agreed.

---

_Merged by @charliermarsh on 2023-01-08 03:15_

---

_Closed by @charliermarsh on 2023-01-08 03:15_

---

_Branch deleted on 2023-01-08 03:16_

---

_Comment by @charliermarsh on 2023-01-08 03:16_

It's maybe worth auditing the methods here. I think we're missing [`assertCountEqual`](https://docs.python.org/3/library/unittest.html#unittest.TestCase.assertCountEqual)? Although maybe that's it.

---

_Comment by @harupy on 2023-01-08 05:21_

@charliermarsh Agreed, filed #1736 

---
