---
number: 17599
title: PT019 should not recommend usefixtures for parametrize values
type: issue
state: closed
author: Hawk777
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-04-24T05:14:25Z
updated_at: 2025-05-14T14:31:44Z
url: https://github.com/astral-sh/ruff/issues/17599
synced_at: 2026-01-07T13:12:16-06:00
---

# PT019 should not recommend usefixtures for parametrize values

---

_Issue opened by @Hawk777 on 2025-04-24 05:14_

### Summary

Consider the following code:
```python
import pytest

@pytest.mark.parametrize("_foo", [1, 2, 3])
def test_thingy(_foo):
    print("hello")
```

When I run `ruff check --select=PT019` on it, it fails. However, according to a pytest dev replying to my question [here](https://github.com/pytest-dev/pytest/issues/13349), a value injected via `parametrize` is a fixture only due to an implementation detail that should not be relied upon. Therefore, it is inappropriate to use `@pytest.mark.usefixture` on it.

### Version

ruff 0.11.6

---

_Comment by @MichaReiser on 2025-04-24 06:33_

The PT019 rule assumes that every parameter starting with a `_` is a fixture. It seems reasonable to exclude parameters for which a named `parametrize` exists.

---

_Label `rule` added by @MichaReiser on 2025-04-24 06:33_

---

_Label `help wanted` added by @MichaReiser on 2025-04-24 06:33_

---

_Referenced in [astral-sh/ruff#17650](../../astral-sh/ruff/pulls/17650.md) on 2025-04-26 22:27_

---

_Closed by @ntBre on 2025-05-14 14:31_

---

_Closed by @ntBre on 2025-05-14 14:31_

---
