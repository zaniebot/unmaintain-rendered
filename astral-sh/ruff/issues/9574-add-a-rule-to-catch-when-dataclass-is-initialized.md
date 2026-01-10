```yaml
number: 9574
title: Add a rule to catch when dataclass is initialized as default argument using field
type: issue
state: closed
author: bvsk35
labels: []
assignees: []
created_at: 2024-01-18T22:06:39Z
updated_at: 2024-01-18T22:32:46Z
url: https://github.com/astral-sh/ruff/issues/9574
synced_at: 2026-01-10T11:09:51Z
```

# Add a rule to catch when dataclass is initialized as default argument using field

---

_Issue opened by @bvsk35 on 2024-01-18 22:06_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

# A minimal code snippet that reproduces the bug
```python
from dataclasses import dataclass, field

@dataclass
class Alice:
    var: int = 2

@dataclass
class Bob:
    val_1: Alice = field(default=Alice())
    val_2: Alice = Alice()  # Same as above but will be caught by RUF009


obj_1 = Bob()
obj_2 = Bob()

assert id(obj_1.val_1) == id(obj_2.val_1)  # This will yield True
```
Right now ruff will not catch `val_1` default initialization as an error. But `RUF009` error code will be raised for `val_2`. Maybe `RUF009` can be extended to incorporate `field(default=...)`. 

# Command to run
`ruff check /path/to/file.py`

# Ruff settings 
I am using generic ruff error codes such as `B, E, F, I, N, PL, RUF`.

# Ruff version
`ruff 0.1.13`  


Please let me know if there is any other required information that is missing. 

---

_Renamed from "Add a RUFXXX rule to catch function calls in default arguments inside dataclass when using field" to "Add a rule to catch when dataclass is initialized in default arguments" by @bvsk35 on 2024-01-18 22:17_

---

_Renamed from "Add a rule to catch when dataclass is initialized in default arguments" to "Add a rule to catch when dataclass is initialized in default arguments when using field" by @bvsk35 on 2024-01-18 22:20_

---

_Renamed from "Add a rule to catch when dataclass is initialized in default arguments when using field" to "Add a rule to catch when dataclass is initialized as default argument using field" by @bvsk35 on 2024-01-18 22:24_

---

_Closed by @bvsk35 on 2024-01-18 22:32_

---
