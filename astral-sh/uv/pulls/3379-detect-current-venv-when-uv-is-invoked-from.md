```yaml
number: 3379
title: "Detect current venv when `uv` is invoked from within a virtualenv"
type: pull_request
state: merged
author: gotcha
labels:
  - bug
assignees: []
merged: true
base: main
head: called_from_venv
created_at: 2024-05-05T20:02:44Z
updated_at: 2024-05-06T13:56:43Z
url: https://github.com/astral-sh/uv/pull/3379
synced_at: 2026-01-12T16:05:36Z
```

# Detect current venv when `uv` is invoked from within a virtualenv

---

_@gotcha_

Fixes #3378


Does this still demands a test ?


---

_@charliermarsh approved on 2024-05-05 22:39_

---

_Comment by @charliermarsh on 2024-05-05 22:39_

Thanks, this looks reasonable to me.

---

_Label `bug` added by @charliermarsh on 2024-05-05 22:39_

---

_Renamed from "Detect current venv if called from within." to "Detect current venv when `uv` is invoked from within a virtualenv" by @charliermarsh on 2024-05-05 22:40_

---

_Merged by @charliermarsh on 2024-05-05 22:53_

---

_Closed by @charliermarsh on 2024-05-05 22:53_

---

_Review comment by @gotcha on `crates/uv-interpreter/src/virtualenv.rs`:79 on 2024-05-06 09:17_

Thanks for merging !

The comment is not in line with the strategy.

Should I make a PR or let you fix it ?

---

_Review comment by @gotcha on `crates/uv-interpreter/src/virtualenv.rs`:98 on 2024-05-06 09:19_

What is the reasoning behind moving the check for `pyvenv.cfg` before search for `.venv` to after the search ?

---

_@gotcha reviewed on 2024-05-06 09:19_

Still have questions...

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/virtualenv.rs`:98 on 2024-05-06 13:41_

The main thing I wanted to change here was looking in ancestor directories, so that if you're in `.venv/bin`, we still find the current venv. Whether this check comes before or after the `// Search for a `.venv` directory.` step is somewhat less clear, that's a weird situation (you have a nested venv within a venv).

---

_@charliermarsh reviewed on 2024-05-06 13:41_

---

_@charliermarsh reviewed on 2024-05-06 13:56_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/virtualenv.rs`:79 on 2024-05-06 13:56_

https://github.com/astral-sh/uv/pull/3406

---

_@charliermarsh reviewed on 2024-05-06 13:56_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/virtualenv.rs`:98 on 2024-05-06 13:56_

https://github.com/astral-sh/uv/pull/3406

---
