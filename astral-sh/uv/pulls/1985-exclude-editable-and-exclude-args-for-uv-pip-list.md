```yaml
number: 1985
title: "`--exclude-editable` and `--exclude` args for `uv pip list`"
type: pull_request
state: merged
author: sbrugman
labels:
  - compatibility
  - cli
assignees: []
merged: true
base: main
head: pip-list-args
created_at: 2024-02-26T14:16:47Z
updated_at: 2024-02-26T20:52:48Z
url: https://github.com/astral-sh/uv/pull/1985
synced_at: 2026-01-10T14:54:43Z
```

# `--exclude-editable` and `--exclude` args for `uv pip list`

---

_Pull request opened by @sbrugman on 2024-02-26 14:16_

Allows filtering out editable (`--exclude-editable`) and explicitly provided packages (`--exclude setuptools`) from `uv pip list`.

https://pip.pypa.io/en/stable/cli/pip_list/

Continuation of https://github.com/astral-sh/uv/issues/1401

## Test Plan

Extended existing tests to cover these new arguments.


---

_@charliermarsh reviewed on 2024-02-26 14:58_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:704 on 2024-02-26 14:58_

This is the default, right?

---

_@sbrugman reviewed on 2024-02-26 15:04_

---

_Review comment by @sbrugman on `crates/uv/src/main.rs`:704 on 2024-02-26 15:04_

By default all packages are listed (confirmed with `pip list` locally)

---

_@charliermarsh approved on 2024-02-26 20:47_

Thanks!

---

_Label `compatibility` added by @charliermarsh on 2024-02-26 20:47_

---

_Label `cli` added by @charliermarsh on 2024-02-26 20:47_

---

_Merged by @charliermarsh on 2024-02-26 20:52_

---

_Closed by @charliermarsh on 2024-02-26 20:52_

---

_Branch deleted on 2024-02-26 20:52_

---
