```yaml
number: 1278
title: Update RustPython to use correct Tuple location
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: update-rust-python
created_at: 2022-12-18T13:27:44Z
updated_at: 2022-12-18T13:55:04Z
url: https://github.com/astral-sh/ruff/pull/1278
synced_at: 2026-01-12T15:55:06Z
```

# Update RustPython to use correct Tuple location

---

_@harupy_

Update RustPython for https://github.com/RustPython/RustPython/pull/4340.

```
$ cargo run -- --no-cache --show-source --select B013 resources/test/fixtures/flake8_bugbear/B013.py
resources/test/fixtures/flake8_bugbear/B013.py:3:8: B013 A length-one tuple literal is redundant. Write `except ValueError` instead of `except (ValueError,)`.
  |
3 | except (ValueError,):
  |        ^^^^^^^^^^^^^ B013
  |
1 potentially fixable with the --fix option.
```

B013 now covers the correct range.

---

_Merged by @charliermarsh on 2022-12-18 13:53_

---

_Closed by @charliermarsh on 2022-12-18 13:53_

---

_Branch deleted on 2022-12-18 13:55_

---
