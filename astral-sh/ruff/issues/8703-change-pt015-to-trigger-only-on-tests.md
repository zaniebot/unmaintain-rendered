```yaml
number: 8703
title: Change PT015 to trigger only on tests
type: issue
state: closed
author: NeilGirdhar
labels:
  - question
assignees: []
created_at: 2023-11-15T19:15:49Z
updated_at: 2023-11-20T20:02:25Z
url: https://github.com/astral-sh/ruff/issues/8703
synced_at: 2026-01-12T15:54:48Z
```

# Change PT015 to trigger only on tests

---

_@NeilGirdhar_

[PT015](https://docs.astral.sh/ruff/rules/pytest-assert-always-false/) recommends using `pytest.fail`, which makes perfect sense inside a test.  But why is it triggering in ordinary code?

---

_Comment by @charliermarsh on 2023-11-15 20:16_

This may be an ignorant question, but how can we determine whether something is a test or not in the context of pytest? Aren't pytest tests just ordinary, arbitrary functions?

---

_Label `question` added by @charliermarsh on 2023-11-15 20:16_

---

_Comment by @NeilGirdhar on 2023-11-15 21:46_

> Aren't pytest tests just ordinary, arbitrary functions?

Here's the [test discovery from the docs](https://docs.pytest.org/en/7.2.x/explanation/goodpractices.html#conventions-for-python-test-discovery).

I'm not sure how careful you want this test to be, but this test could be made to only trigger on:
* `test` prefixed functions outside of a class, and
* `test` prefixed methods inside `Test` prefixed test classes (without an `__init__` method)

that are within files named `test_*.py` or `*_test.py`.   Although, this is technically [changeable](https://docs.pytest.org/en/7.2.x/example/pythoncollection.html#changing-naming-conventions) by the user.

What do you think?

---

_Renamed from "Why does PT015 trigger outside tests?" to "Change PT015 to trigger only on tests" by @NeilGirdhar on 2023-11-16 17:08_

---

_Comment by @dhruvmanila on 2023-11-20 20:02_

Closing this in favor of #8794

---

_Closed by @dhruvmanila on 2023-11-20 20:02_

---
