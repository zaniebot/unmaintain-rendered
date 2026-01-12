```yaml
number: 657
title: Implement ANN401
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: feat/ANN401
created_at: 2022-11-08T10:07:14Z
updated_at: 2022-11-08T16:11:11Z
url: https://github.com/astral-sh/ruff/pull/657
synced_at: 2026-01-12T05:48:45Z
```

# Implement ANN401

---

_Pull request opened by @edgarrmondragon on 2022-11-08 10:07_

_No description provided._

---

_Comment by @charliermarsh on 2022-11-08 14:22_

This is a really strong PR, nice work!

---

_Review comment by @charliermarsh on `src/flake8_annotations/plugins.rs`:102 on 2022-11-08 14:24_

It'd be nice to do this clone (`.to_string()`) within `check_dynamically_typed` (and have that take `&str` instead of `String`), but maybe challenging with the `format!` calls.

---

_@charliermarsh reviewed on 2022-11-08 14:24_

---

_@charliermarsh reviewed on 2022-11-08 14:26_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_annotations/allow_star_arg_any.py`:1 on 2022-11-08 14:26_

I initially took these test files from flake8-annotations (sort of -- in flake8-annotations, the tests are encoded as strings in a test runner instead of raw code). So this deviates from that pattern IIUC. But I think it's fine, it looks like a good test suite.

---

_Merged by @charliermarsh on 2022-11-08 14:49_

---

_Closed by @charliermarsh on 2022-11-08 14:49_

---

_Comment by @charliermarsh on 2022-11-08 14:49_

Thanks for a great first PR!

---

_Branch deleted on 2022-11-08 16:11_

---
