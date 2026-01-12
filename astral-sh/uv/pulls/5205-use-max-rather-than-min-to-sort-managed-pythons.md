```yaml
number: 5205
title: Use max rather than min to sort managed Pythons
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ord
created_at: 2024-07-19T01:14:02Z
updated_at: 2024-07-19T12:35:18Z
url: https://github.com/astral-sh/uv/pull/5205
synced_at: 2026-01-12T16:06:41Z
```

# Use max rather than min to sort managed Pythons

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/uv/issues/5139 and https://github.com/astral-sh/uv/pull/5201#discussion_r1683636935.

## Test Plan

Verified that 3.12 was chosen above 3.8 in:

- `cargo run -- python uninstall --all`
- `cargo run -- python install 3.8 3.12`
- `cargo run -- tool run -v httpx`


---

_Marked ready for review by @charliermarsh on 2024-07-19 01:14_

---

_Comment by @charliermarsh on 2024-07-19 01:14_

\cc @j178

---

_Review requested from @zanieb by @charliermarsh on 2024-07-19 01:14_

---

_Label `bug` added by @charliermarsh on 2024-07-19 01:14_

---

_@zanieb reviewed on 2024-07-19 01:36_

---

_Review comment by @zanieb on `crates/uv-python/src/implementation.rs`:19 on 2024-07-19 01:36_

This is pretty unintuitive, hope it doesn't bite us later :)

---

_@zanieb approved on 2024-07-19 01:36_

Probably #internal?

---

_Label `bug` removed by @charliermarsh on 2024-07-19 12:25_

---

_Label `internal` added by @charliermarsh on 2024-07-19 12:25_

---

_@charliermarsh reviewed on 2024-07-19 12:25_

---

_Review comment by @charliermarsh on `crates/uv-python/src/implementation.rs`:19 on 2024-07-19 12:25_

I guess but it's also the standard derive implementation!

---

_Merged by @charliermarsh on 2024-07-19 12:35_

---

_Closed by @charliermarsh on 2024-07-19 12:35_

---

_Branch deleted on 2024-07-19 12:35_

---
