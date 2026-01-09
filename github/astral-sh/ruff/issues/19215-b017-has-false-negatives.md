---
number: 19215
title: "`B017` has false negatives"
type: issue
state: closed
author: ntBre
labels:
  - bug
  - rule
assignees: []
created_at: 2025-07-08T19:07:21Z
updated_at: 2025-07-11T16:43:10Z
url: https://github.com/astral-sh/ruff/issues/19215
synced_at: 2026-01-07T13:12:16-06:00
---

# `B017` has false negatives

---

_Issue opened by @ntBre on 2025-07-08 19:07_

[assert-raises-exception (B017)](https://docs.astral.sh/ruff/rules/assert-raises-exception/#assert-raises-exception-b017) has false negatives when the exceptions are provided as keyword arguments:

```python
import pytest
import unittest

with pytest.raises(Exception):  # true positive
    pass

with pytest.raises(expected_exception=Exception):  # false negative
    pass

class TestClass(unittest.TestCase):
    def test_okay(self):
        with self.assertRaises(Exception):  # true positive
            pass

    def test_false_negative(self):
        with self.assertRaises(exception=Exception):  # false negative
            pass
```

I think these also apply to the non-context-manager versions added in #19063. I have a feeling that there are other edge cases around argument validation here, but these are the examples I came up with as a start.

[playground](https://play.ruff.rs/e1ad5948-95f6-43bd-b687-b3dd95e6429f)
            

---

_Label `bug` added by @ntBre on 2025-07-08 19:07_

---

_Label `rule` added by @ntBre on 2025-07-08 19:07_

---

_Comment by @danparizher on 2025-07-08 19:25_

I can grab this one since I worked on the previous issue

---

_Assigned to @danparizher by @ntBre on 2025-07-08 20:06_

---

_Referenced in [astral-sh/ruff#19217](../../astral-sh/ruff/pulls/19217.md) on 2025-07-08 20:35_

---

_Closed by @MichaReiser on 2025-07-11 16:43_

---
