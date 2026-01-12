```yaml
number: 1502
title: "Improve `T20X` ranges"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-T20x-range
created_at: 2022-12-31T08:02:27Z
updated_at: 2022-12-31T12:41:53Z
url: https://github.com/astral-sh/ruff/pull/1502
synced_at: 2026-01-12T05:36:31Z
```

# Improve `T20X` ranges

---

_Pull request opened by @harupy on 2022-12-31 08:02_

This PR improves `T20X`'s ranges to make it easier to find violations in arguments.

### Current:

```
resources/test/fixtures/flake8_print/T201.py:7:1: T201 `print` found
  |
7 | print("Hello, world!", file=sys.stderr)  # T201
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ T201
  |
```

### Improved

```
resources/test/fixtures/flake8_print/T201.py:7:1: T201 `print` found
  |
7 | print("Hello, world!", file=sys.stderr)  # T201
  | ^^^^^ T201
  |
```

---

_Merged by @charliermarsh on 2022-12-31 12:41_

---

_Closed by @charliermarsh on 2022-12-31 12:41_

---
