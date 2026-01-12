```yaml
number: 3517
title: Avoid keyword arguments for PEP 517 build hooks
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/prep
created_at: 2024-05-10T21:01:02Z
updated_at: 2024-05-10T21:15:08Z
url: https://github.com/astral-sh/uv/pull/3517
synced_at: 2026-01-12T16:05:41Z
```

# Avoid keyword arguments for PEP 517 build hooks

---

_@charliermarsh_

## Summary

pip passes these as positional arguments, and at least one build backend relies on that. My personal opinion is that it's a spec violation, and the build backend should be updated, but I'd prefer to favor compatibility over strictness here.

Closes https://github.com/astral-sh/uv/issues/3509.

## Test Plan

`cargo run pip install cryptacular==1.6.2`


---

_Label `compatibility` added by @charliermarsh on 2024-05-10 21:01_

---

_@charliermarsh reviewed on 2024-05-10 21:02_

---

_Review comment by @charliermarsh on `crates/uv-build/src/lib.rs`:830 on 2024-05-10 21:02_

I swapped the order of arguments as `config_settings` is first and `metadata_directory` is second. See: https://peps.python.org/pep-0517/#build-wheel.

---

_Merged by @charliermarsh on 2024-05-10 21:15_

---

_Closed by @charliermarsh on 2024-05-10 21:15_

---

_Branch deleted on 2024-05-10 21:15_

---
