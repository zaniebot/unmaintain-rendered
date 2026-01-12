```yaml
number: 1610
title: "Autofix for `PIE790` leaves whitespace in the affected line"
type: issue
state: closed
author: edgarrmondragon
labels: []
assignees: []
created_at: 2023-01-04T01:05:59Z
updated_at: 2023-01-04T01:45:00Z
url: https://github.com/astral-sh/ruff/issues/1610
synced_at: 2026-01-12T15:54:41Z
```

# Autofix for `PIE790` leaves whitespace in the affected line

---

_@edgarrmondragon_

Test file:

```python
"""Test module."""


class MyClass:
    """This is a class."""

    def __init__(self) -> None:
        """Initialize my class."""
        pass
```

Output:

```diff
$ ruff --select ALL --diff pie790.py
--- pie790.py
+++ pie790.py
@@ -6,4 +6,4 @@
 
     def __init__(self) -> None:
         """Initialize my class."""
-        pass
+        
```

---

_Renamed from "Autofix for `PIE790` leaves trailing whitespaces in the affected line" to "Autofix for `PIE790` leaves whitespaces in the affected line" by @edgarrmondragon on 2023-01-04 01:06_

---

_Comment by @edgarrmondragon on 2023-01-04 01:12_

I think I have the fix for this. Will send a PR.

---

_Comment by @charliermarsh on 2023-01-04 01:16_

Thanks. I feel like we have some code somewhere that deletes the entire line if it's the only statement on the line...

---

_Comment by @charliermarsh on 2023-01-04 01:16_

Or maybe we don't.

---

_Comment by @charliermarsh on 2023-01-04 01:17_

I think if that code went through `autofix/helpers.rs#delete_stmt`, it would do the right thing. But that requires a bit more wiring, I think I was lazy.

---

_Renamed from "Autofix for `PIE790` leaves whitespaces in the affected line" to "Autofix for `PIE790` leaves whitespace in the affected line" by @edgarrmondragon on 2023-01-04 01:32_

---

_Comment by @edgarrmondragon on 2023-01-04 01:41_

@charliermarsh thanks for tip, `delete_stmt` made it super easy :D!

---

_Closed by @charliermarsh on 2023-01-04 01:45_

---
