---
number: 10472
title: Pytest errors outside of test folder
type: issue
state: closed
author: kleinicke
labels: []
assignees: []
created_at: 2024-03-19T12:53:43Z
updated_at: 2024-03-19T13:00:23Z
url: https://github.com/astral-sh/ruff/issues/10472
synced_at: 2026-01-10T01:22:50Z
---

# Pytest errors outside of test folder

---

_Issue opened by @kleinicke on 2024-03-19 12:53_

The ruff pytest rules are currently applied in the complete repo and not only inside the tests. 
Adding somewhere in the code an assert 0 or assert False, will result in PT015 and the suggestion to replace it with pytest.fail().

---

_Comment by @AlexWaygood on 2024-03-19 13:00_

Thanks for opening the issue! Closing as a duplicate of #8794

---

_Closed by @AlexWaygood on 2024-03-19 13:00_

---
