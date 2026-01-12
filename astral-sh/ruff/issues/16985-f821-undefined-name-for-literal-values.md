```yaml
number: 16985
title: "F821 \"Undefined name\" for Literal values"
type: issue
state: closed
author: gavindsouza
labels: []
assignees: []
created_at: 2025-03-26T17:28:44Z
updated_at: 2025-03-26T18:26:59Z
url: https://github.com/astral-sh/ruff/issues/16985
synced_at: 2026-01-12T15:54:55Z
```

# F821 "Undefined name" for Literal values

---

_@gavindsouza_

### Summary

Simplified version of the source code:
```python
from typing import Literal

Data = str
Select = Literal
Check = bool

class Guest:
    nickname: Data
    croissant_preference: Select["Croissant", "Pain au chocolat", "Pain aux raisins"]
    likes_snacks: Check
```

Output of `ruff check`
```bash
% ruff check backoffice/__init__.py                                                                                                                       25-03-26 - 18:42:28
backoffice/__init__.py:13:35: F821 Undefined name `Croissant`
   |
11 | class Guest:
12 |     nickname: Data
13 |     croissant_preference: Select["Croissant", "Pain au chocolat", "Pain aux raisins"]
   |                                   ^^^^^^^^^ F821
14 |     likes_snacks: Check
   |

backoffice/__init__.py:13:47: F722 Syntax error in forward annotation: Unexpected token at the end of an expression
   |
11 | class Guest:
12 |     nickname: Data
13 |     croissant_preference: Select["Croissant", "Pain au chocolat", "Pain aux raisins"]
   |                                               ^^^^^^^^^^^^^^^^^^ F722
14 |     likes_snacks: Check
   |

backoffice/__init__.py:13:67: F722 Syntax error in forward annotation: Unexpected token at the end of an expression
   |
11 | class Guest:
12 |     nickname: Data
13 |     croissant_preference: Select["Croissant", "Pain au chocolat", "Pain aux raisins"]
   |                                                                   ^^^^^^^^^^^^^^^^^^ F722
14 |     likes_snacks: Check
   |

Found 3 errors.
```

### Version

0.11.2, also latest main

---

_Comment by @InSyncWithFoo on 2025-03-26 18:08_

This seems to be a duplicate of #11378.

---

_Comment by @ntBre on 2025-03-26 18:26_

Thanks, I think it is a duplicate. Replacing `Select` with `Literal` directly suppresses the diagnostics, so the issue is related to aliasing again. As the other issue is labeled, this will require better type inference to detect this kind of aliasing robustly.

https://play.ruff.rs/903e0a21-6e61-4210-8469-0520206ebc96

---

_Closed by @ntBre on 2025-03-26 18:26_

---
