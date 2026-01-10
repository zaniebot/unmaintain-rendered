```yaml
number: 5679
title: Support legacy tool receipts with PEP 508 requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/dep
created_at: 2024-08-01T02:58:06Z
updated_at: 2024-08-01T16:43:31Z
url: https://github.com/astral-sh/uv/pull/5679
synced_at: 2026-01-10T13:37:23Z
```

# Support legacy tool receipts with PEP 508 requirements

---

_Pull request opened by @charliermarsh on 2024-08-01 02:58_

## Summary

In #5494, I made breaking changes to the tool receipt format. This would break existing tools for all users. This PR makes the change backwards-compatible by supporting deserialization for the deprecated format.

Closes https://github.com/astral-sh/uv/issues/5680.

## Test Plan

Beyond the automated tests, you can run `cargo run tool list` on your existing machine.

Before:

```
warning: `uv tool list` is experimental and may change without warning
warning: Ignoring malformed tool `black` (run `uv tool uninstall black` to remove)
warning: Ignoring malformed tool `poetry` (run `uv tool uninstall poetry` to remove)
warning: Ignoring malformed tool `ruff` (run `uv tool uninstall ruff` to remove)
```

After:

```
warning: `uv tool list` is experimental and may change without warning
black v0.1.0
- black
poetry v1.8.3
- poetry
ruff v0.0.60
- ruff
```


---

_Review requested from @zanieb by @charliermarsh on 2024-08-01 02:59_

---

_Label `bug` added by @charliermarsh on 2024-08-01 02:59_

---

_Label `preview` added by @charliermarsh on 2024-08-01 02:59_

---

_@zanieb approved on 2024-08-01 16:42_

---

_Merged by @charliermarsh on 2024-08-01 16:43_

---

_Closed by @charliermarsh on 2024-08-01 16:43_

---

_Branch deleted on 2024-08-01 16:43_

---
