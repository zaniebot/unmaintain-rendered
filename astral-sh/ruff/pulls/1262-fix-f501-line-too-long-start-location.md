```yaml
number: 1262
title: Fix F501 (line-too-long) start location
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-F501-range
created_at: 2022-12-16T08:44:17Z
updated_at: 2022-12-17T01:46:46Z
url: https://github.com/astral-sh/ruff/pull/1262
synced_at: 2026-01-12T05:36:31Z
```

# Fix F501 (line-too-long) start location

---

_Pull request opened by @harupy on 2022-12-16 08:44_

This PR fixes the start location of F501 (line-too-long).

### Before

```
resources/test/fixtures/pycodestyle/E501.py:5:1: E501 Line too long (123 > 88 characters)
  |
5 | Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E501
  |
```

- Covers the entire line.
- Hard to tell where we can wrap the line.
- Easily overlaps with other violations.

### After

```
resources/test/fixtures/pycodestyle/E501.py:5:89: E501 Line too long (123 > 88 characters)
  |
5 | Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
  |                                                                                         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E501
  |
```

- Only covers the characters that exceed the limit.
- Easier to tell where we can wrap the line.
- Can avoid overlapping with other violations.
- flake8 use this approach.


---

_Renamed from "Fix F501 start location" to "Fix F501 (line-too-long) start location" by @harupy on 2022-12-16 08:45_

---

_Merged by @charliermarsh on 2022-12-16 16:29_

---

_Closed by @charliermarsh on 2022-12-16 16:29_

---

_Branch deleted on 2022-12-17 01:46_

---
