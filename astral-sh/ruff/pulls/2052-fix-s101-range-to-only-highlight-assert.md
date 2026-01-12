```yaml
number: 2052
title: "Fix S101 range to only highlight `assert`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-S101-range
created_at: 2023-01-21T08:15:43Z
updated_at: 2023-01-21T12:15:00Z
url: https://github.com/astral-sh/ruff/pull/2052
synced_at: 2026-01-12T15:55:07Z
```

# Fix S101 range to only highlight `assert`

---

_@harupy_

Fix:

```
resources/test/fixtures/flake8_bandit/S101.py:2:1: S101 Use of `assert` detected
  |
2 | assert True
  | ^^^^^^^^^^^ S101
  |

resources/test/fixtures/flake8_bandit/S101.py:8:5: S101 Use of `assert` detected
  |
8 |     assert x == 1
  |     ^^^^^^^^^^^^^ S101
  |

resources/test/fixtures/flake8_bandit/S101.py:11:5: S101 Use of `assert` detected
   |
11 |     assert x == 2
   |     ^^^^^^^^^^^^^ S101
   |

Found 3 error(s).
```

to:

```
resources/test/fixtures/flake8_bandit/S101.py:2:1: S101 Use of `assert` detected
  |
2 | assert True
  | ^^^^^^ S101
  |

resources/test/fixtures/flake8_bandit/S101.py:8:5: S101 Use of `assert` detected
  |
8 |     assert x == 1
  |     ^^^^^^ S101
  |

resources/test/fixtures/flake8_bandit/S101.py:11:5: S101 Use of `assert` detected
   |
11 |     assert x == 2
   |     ^^^^^^ S101
   |
```


---

_Merged by @charliermarsh on 2023-01-21 12:15_

---

_Closed by @charliermarsh on 2023-01-21 12:15_

---
