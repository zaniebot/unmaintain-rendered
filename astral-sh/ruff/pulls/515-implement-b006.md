```yaml
number: 515
title: Implement B006
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: B006
created_at: 2022-10-30T02:46:30Z
updated_at: 2022-10-30T16:57:58Z
url: https://github.com/astral-sh/ruff/pull/515
synced_at: 2026-01-12T15:55:05Z
```

# Implement B006

---

_@harupy_

See #389 

---

_@charliermarsh reviewed on 2022-10-30 13:37_

---

_Review comment by @charliermarsh on `src/checks_gen.rs`:894 on 2022-10-30 13:37_

@harupy - BTW -- any thoughts on the approach I took to this problem? I.e., code-generating the prefix map, to allow static enums everywhere?

---

_Review comment by @charliermarsh on `resources/test/fixtures/B006.py`:1 on 2022-10-30 13:38_

Could we instead use the test suite from flake8-bugbear? https://github.com/PyCQA/flake8-bugbear/blob/main/tests/b006_b008.py

---

_@charliermarsh reviewed on 2022-10-30 13:38_

---

_@charliermarsh reviewed on 2022-10-30 13:44_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/mutable_argument_default.rs`:9 on 2022-10-30 13:44_

Nit: can we rename `default` to `arg` or `expr`? `default` is a keyword so the syntax highlighting is confusing here.

---

_Comment by @charliermarsh on 2022-10-30 13:45_

Awesome, a couple comments!

---

_@harupy reviewed on 2022-10-30 15:07_

---

_Review comment by @harupy on `src/flake8_bugbear/plugins/mutable_argument_default.rs`:9 on 2022-10-30 15:07_

Got it, will change this.

---

_@harupy reviewed on 2022-10-30 15:10_

---

_Review comment by @harupy on `src/flake8_bugbear/plugins/mutable_argument_default.rs`:9 on 2022-10-30 15:10_

How about `default_value`?

---

_@harupy reviewed on 2022-10-30 15:11_

---

_Review comment by @harupy on `src/flake8_bugbear/plugins/mutable_argument_default.rs`:9 on 2022-10-30 15:11_

Chose `expr`.

---

_Review comment by @harupy on `src/checks_gen.rs`:894 on 2022-10-30 15:26_

How does flake8 implement this?

---

_@harupy reviewed on 2022-10-30 15:26_

---

_Merged by @charliermarsh on 2022-10-30 16:57_

---

_Closed by @charliermarsh on 2022-10-30 16:57_

---
