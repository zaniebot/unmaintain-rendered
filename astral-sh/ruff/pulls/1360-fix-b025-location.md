```yaml
number: 1360
title: Fix B025 location
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-B025-location
created_at: 2022-12-24T16:25:16Z
updated_at: 2022-12-24T17:22:12Z
url: https://github.com/astral-sh/ruff/pull/1360
synced_at: 2026-01-12T05:36:31Z
```

# Fix B025 location

---

_Pull request opened by @harupy on 2022-12-24 16:25_

Closes #1359 

## Before

```
   |
15 | / try:
16 | |     a = 1
17 | | except ValueError:
18 | |     a = 2
19 | | except ValueError:
20 | |     a = 2
   | |_________^ B025
   |
resources/test/fixtures/flake8_bugbear/B025.py:22:1: B025 try-except block with duplicate exception `pickle.PickleError`
   |
22 | / try:
23 | |     a = 1
24 | | except pickle.PickleError:
25 | |     a = 2
26 | | except ValueError:
27 | |     a = 2
28 | | except pickle.PickleError:
29 | |     a = 2
   | |_________^ B025
   |
resources/test/fixtures/flake8_bugbear/B025.py:31:1: B025 try-except block with duplicate exception `TypeError`
   |
31 | / try:
32 | |     a = 1
33 | | except (ValueError, TypeError):
34 | |     a = 2
35 | | except ValueError:
36 | |     a = 2
37 | | except (OSError, TypeError):
38 | |     a = 2
   | |_________^ B025
   |
resources/test/fixtures/flake8_bugbear/B025.py:31:1: B025 try-except block with duplicate exception `ValueError`
   |
31 | / try:
32 | |     a = 1
33 | | except (ValueError, TypeError):
34 | |     a = 2
35 | | except ValueError:
36 | |     a = 2
37 | | except (OSError, TypeError):
38 | |     a = 2
   | |_________^ B025
   |
Found 4 error(s).
```

## After

```
   |
19 | except ValueError:
   |        ^^^^^^^^^^ B025
   |
resources/test/fixtures/flake8_bugbear/B025.py:28:8: B025 try-except block with duplicate exception `pickle.PickleError`
   |
28 | except pickle.PickleError:
   |        ^^^^^^^^^^^^^^^^^^ B025
   |
resources/test/fixtures/flake8_bugbear/B025.py:35:8: B025 try-except block with duplicate exception `ValueError`
   |
35 | except ValueError:
   |        ^^^^^^^^^^ B025
   |
resources/test/fixtures/flake8_bugbear/B025.py:37:18: B025 try-except block with duplicate exception `TypeError`
   |
37 | except (OSError, TypeError):
   |                  ^^^^^^^^^ B025
   |
Found 4 error(s).
```

---

_@charliermarsh reviewed on 2022-12-24 17:21_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/duplicate_exceptions.rs`:37 on 2022-12-24 17:21_

Lol I was just trying to get this to work... Seems like a false-positive with Clippy, since the autofix fails?

---

_Merged by @charliermarsh on 2022-12-24 17:22_

---

_Closed by @charliermarsh on 2022-12-24 17:22_

---
