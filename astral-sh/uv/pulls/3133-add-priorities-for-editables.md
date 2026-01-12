```yaml
number: 3133
title: Add priorities for editables
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ed
created_at: 2024-04-19T01:52:22Z
updated_at: 2024-04-19T02:04:59Z
url: https://github.com/astral-sh/uv/pull/3133
synced_at: 2026-01-12T16:05:27Z
```

# Add priorities for editables

---

_@charliermarsh_

## Summary

We weren't setting a priority for editables, so they were being visited last.

I think there's still a problem whereby we're not aggressive enough in visiting recursive extras (and, in fact, that's making it really hard to write a test -- I wrote a test, but the most-reduced case still fails, and I'd need to add a layer of indirection to make it fail-on-main-but-pass-on-this-branch), but that problem likely already existed on main prior to #3087, so I just want to get this quick fix out now.

Closes https://github.com/astral-sh/uv/issues/3127.

## Test Plan

- `git clone https://github.com/cda-tum/mqt-core.git`
- `cargo run venv`
- `cargo run pip install 'scikit-build-core[pyproject]>=0.8.1' 'setuptools_scm>=7' 'pybind11>=2.12' --resolution=lowest-direct`
- `cargo run pip install --no-build-isolation '-ve.[test,qiskit,evaluation,coverage]' --resolution=lowest-direct`


---

_Marked ready for review by @charliermarsh on 2024-04-19 01:54_

---

_Label `bug` added by @charliermarsh on 2024-04-19 01:55_

---

_Comment by @charliermarsh on 2024-04-19 01:56_

I plan to follow-up with another fix and a real test.

---

_Merged by @charliermarsh on 2024-04-19 02:04_

---

_Closed by @charliermarsh on 2024-04-19 02:04_

---

_Branch deleted on 2024-04-19 02:04_

---
