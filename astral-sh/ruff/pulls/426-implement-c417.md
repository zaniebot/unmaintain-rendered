```yaml
number: 426
title: Implement C417
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: C417
created_at: 2022-10-14T14:51:43Z
updated_at: 2022-10-14T16:34:01Z
url: https://github.com/astral-sh/ruff/pull/426
synced_at: 2026-01-12T05:48:45Z
```

# Implement C417

---

_Pull request opened by @harupy on 2022-10-14 14:51_

See #305

---

_@harupy reviewed on 2022-10-14 14:54_

---

_Review comment by @harupy on `resources/test/fixtures/C417.py`:6 on 2022-10-14 14:54_

```
cargo run resources/test/fixtures/C417.py --select C417
```

gives:

```
resources/test/fixtures/C417.py:2:1: C417 Unnecessary map usage - rewrite using a generator expression
resources/test/fixtures/C417.py:3:1: C417 Unnecessary map usage - rewrite using a generator expression
resources/test/fixtures/C417.py:4:1: C417 Unnecessary map usage - rewrite using a list comprehension
resources/test/fixtures/C417.py:4:6: C417 Unnecessary map usage - rewrite using a generator expression
resources/test/fixtures/C417.py:5:1: C417 Unnecessary map usage - rewrite using a set comprehension
resources/test/fixtures/C417.py:5:5: C417 Unnecessary map usage - rewrite using a generator expression
resources/test/fixtures/C417.py:6:1: C417 Unnecessary map usage - rewrite using a dict comprehension
resources/test/fixtures/C417.py:6:6: C417 Unnecessary map usage - rewrite using a generator expression
```

`map` calls in `dict` | `set` | `list` need to be ignored.



---

_@harupy reviewed on 2022-10-14 14:55_

---

_Review comment by @harupy on `resources/test/fixtures/C417.py`:6 on 2022-10-14 14:55_

`flake8-comprehensions` creates a set and stores already-visited map calls in it to ignore them. 

https://github.com/adamchainz/flake8-comprehensions/blob/baa8acd66b4582e473d21143ddd3f76887ab9c39/src/flake8_comprehensions/__init__.py#L49

---

_@charliermarsh reviewed on 2022-10-14 15:50_

---

_Review comment by @charliermarsh on `resources/test/fixtures/C417.py`:6 on 2022-10-14 15:50_

I think it's ok to have multiple checks on one line even if slightly redundant. (At least, I prefer that to having to track already-visited map calls.)

---

_@charliermarsh reviewed on 2022-10-14 16:24_

---

_Review comment by @charliermarsh on `README.md`:305 on 2022-10-14 16:24_

@harupy - Do you want to do the honor of removing `(15/16)` from the README?

---

_Review comment by @harupy on `README.md`:305 on 2022-10-14 16:26_

Removed!

---

_@harupy reviewed on 2022-10-14 16:26_

---

_Merged by @charliermarsh on 2022-10-14 16:34_

---

_Closed by @charliermarsh on 2022-10-14 16:34_

---
