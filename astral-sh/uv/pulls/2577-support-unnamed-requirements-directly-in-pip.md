```yaml
number: 2577
title: Support unnamed requirements directly in pip uninstall
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: main
head: charlie/bare-v
created_at: 2024-03-20T21:25:58Z
updated_at: 2024-03-21T04:02:07Z
url: https://github.com/astral-sh/uv/pull/2577
synced_at: 2026-01-10T14:49:08Z
```

# Support unnamed requirements directly in pip uninstall

---

_Pull request opened by @charliermarsh on 2024-03-20 21:25_

## Summary

In `pip uninstall`, we shouldn't need to resolve unnamed requirements, since we already index packages in `site-packages` by their URL.

This also changes `uninstall` to ignore editables, which matches pip's behavior.

Part of: https://github.com/astral-sh/uv/issues/313.

## Test Plan

Run `cargo run pip install ./scripts/editable-installs/black_editable`, followed by each of the following:

- `cargo run pip uninstall ./scripts/editable-installs/black_editable`
- `cargo run pip uninstall black`
- `cargo run pip uninstall ./scripts/editable-installs/black_editable black`


---

_Label `enhancement` added by @charliermarsh on 2024-03-20 23:41_

---

_Marked ready for review by @charliermarsh on 2024-03-20 23:41_

---

_@zanieb reviewed on 2024-03-21 04:00_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:966 on 2024-03-21 04:00_

Huh why did we have this in the first place? Is there an actual behavior change here or do we just treat editables like normal packages during uninstall?

We should note this as a breaking change in the changelog.

---

_@zanieb approved on 2024-03-21 04:01_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:966 on 2024-03-21 04:01_

We just treated editables like normal packages (sort of). We removed any packages that matched the URL (regardless of whether they're editable). I guess this probably existed because we didn't support direct URL requirements, so you couldn't do `uv pip uninstall ./path/to/file`. But pip does _not_ allow this. I'll note it as such in the changelog.

---

_@charliermarsh reviewed on 2024-03-21 04:01_

---

_Label `breaking` added by @charliermarsh on 2024-03-21 04:02_

---

_Merged by @charliermarsh on 2024-03-21 04:02_

---

_Closed by @charliermarsh on 2024-03-21 04:02_

---

_Branch deleted on 2024-03-21 04:02_

---
