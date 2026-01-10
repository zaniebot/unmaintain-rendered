---
number: 17132
title: "PT019 `pytest-fixture-param-without-value` misses public fixture"
type: issue
state: open
author: mschoettle
labels:
  - question
assignees: []
created_at: 2025-04-01T19:55:10Z
updated_at: 2025-04-02T07:41:54Z
url: https://github.com/astral-sh/ruff/issues/17132
synced_at: 2026-01-10T01:22:58Z
---

# PT019 `pytest-fixture-param-without-value` misses public fixture

---

_Issue opened by @mschoettle on 2025-04-01 19:55_

### Summary

I defined a fixture in `conftest.py` and I was expecting a violation since it satisfies the same logic as described in the [rule description.](https://docs.astral.sh/ruff/rules/pytest-fixture-param-without-value/).

```python
# in conftest.py
def my_fixture() -> None:
    pass
```

```python
def test_something(my_fixture: None):
    pass
```

### Version

ruff 0.11.2 (4773878ee 2025-03-21)

---

_Comment by @dhruvmanila on 2025-04-01 20:24_

Looking at the code, it seems that you'd need to prefix the fixture with an underscore, so `_my_fixture` works, which I think would be to match the upstream rule (?).

So, the following would work:
```py
def test_some(_my_fixture: None):
    pass
```

You'd also need to use `test_` prefix and not suffix.

---

_Comment by @mschoettle on 2025-04-01 21:23_

It does work when prefixing with an underscore. When reading the rationale ([upstream](https://github.com/m-burst/flake8-pytest-style/blob/master/docs/rules/PT019.md) and `ruff`, i.e., unused fixture in the test function) it seems to me that it should apply to any fixture that is not used in the test function, so also those not prefixed. Unless fixtures should be prefixed all the time? 🤔 

> You'd also need to use test_ prefix and not suffix.

Good catch. It was just in this minimal example.

---

_Comment by @MichaReiser on 2025-04-02 07:41_

The rule uses heuristics to detect fixtures, and we need to balance false positives (where Ruff detects something as a fixture where it isn't) with false negatives. That's why it requires the underscore.

---

_Label `question` added by @MichaReiser on 2025-04-02 07:41_

---
