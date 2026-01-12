```yaml
number: 88
title: "refactor: run `cargo clippy --fix`"
type: pull_request
state: merged
author: TeamTamoad
labels: []
assignees: []
merged: true
base: main
head: refactor-clippy-fix
created_at: 2022-09-02T18:54:01Z
updated_at: 2022-09-03T07:47:48Z
url: https://github.com/astral-sh/ruff/pull/88
synced_at: 2026-01-12T15:55:04Z
```

# refactor: run `cargo clippy --fix`

---

_@TeamTamoad_

Fix the following
- https://rust-lang.github.io/rust-clippy/master/index.html#needless_borrow
- https://rust-lang.github.io/rust-clippy/master/index.html#map_flatten

By default `clippy` doesn't check test modules. You can check them by running `cargo clippy --tests`
However, running `cargo clippy --fix` will check and auto-fix the test modules.

---

_Renamed from "refactor: run `clippy --fix`" to "refactor: run `cargo clippy --fix`" by @TeamTamoad on 2022-09-02 18:54_

---

_Merged by @charliermarsh on 2022-09-02 21:01_

---

_Closed by @charliermarsh on 2022-09-02 21:01_

---

_Comment by @charliermarsh on 2022-09-02 21:01_

Thank you!

---

_Branch deleted on 2022-09-03 07:47_

---
