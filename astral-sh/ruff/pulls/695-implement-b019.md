```yaml
number: 695
title: Implement B019
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: B019
created_at: 2022-11-12T06:37:01Z
updated_at: 2022-11-12T16:14:04Z
url: https://github.com/astral-sh/ruff/pull/695
synced_at: 2026-01-12T05:48:45Z
```

# Implement B019

---

_Pull request opened by @harupy on 2022-11-12 06:37_

#389

---

_@charliermarsh reviewed on 2022-11-12 14:28_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/cached_instance_method.rs`:13 on 2022-11-12 14:28_

Do you mind tweaking this to use the `checker.from_imports` to verify that these are actually imported from `functools`?

---

_@charliermarsh reviewed on 2022-11-12 14:31_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/cached_instance_method.rs`:13 on 2022-11-12 14:31_

(There are some TODOs to do this in other files too, like `src/flake8_bugbear/plugins/mutable_argument_default.rs`, but it's a newer feature.)

---

_@harupy reviewed on 2022-11-12 14:35_

---

_Review comment by @harupy on `src/flake8_bugbear/plugins/cached_instance_method.rs`:13 on 2022-11-12 14:35_

Got it. Do we have an example?

---

_@charliermarsh reviewed on 2022-11-12 14:37_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/cached_instance_method.rs`:13 on 2022-11-12 14:37_

`flake8_2020/plugins.rs`

---

_@harupy reviewed on 2022-11-12 14:53_

---

_Review comment by @harupy on `src/flake8_bugbear/plugins/cached_instance_method.rs`:13 on 2022-11-12 14:53_

Done!

---

_Merged by @charliermarsh on 2022-11-12 16:14_

---

_Closed by @charliermarsh on 2022-11-12 16:14_

---
