```yaml
number: 1508
title: Fix E722 and F707 ranges
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-except-range
created_at: 2022-12-31T10:33:08Z
updated_at: 2022-12-31T12:58:46Z
url: https://github.com/astral-sh/ruff/pull/1508
synced_at: 2026-01-12T05:36:31Z
```

# Fix E722 and F707 ranges

---

_Pull request opened by @harupy on 2022-12-31 10:33_

This PR fixes E722 and F707 ranges.

### Current

```
resources/test/fixtures/pycodestyle/E722.py:16:1: E722 Do not use bare `except`
   |
16 | / except:
17 | |     pass
   | |________^ E722
   |
```

### Fixed

```
resources/test/fixtures/pycodestyle/E722.py:16:1: E722 Do not use bare `except`
   |
16 | except:
   | ^^^^^^ E722
   |
```

---

_Merged by @charliermarsh on 2022-12-31 12:58_

---

_Closed by @charliermarsh on 2022-12-31 12:58_

---
