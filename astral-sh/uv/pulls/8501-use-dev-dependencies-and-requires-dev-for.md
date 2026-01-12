```yaml
number: 8501
title: "Use `dev-dependencies` and `requires-dev` for lockfile compatibility"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: tracking/735
head: charlie/rev
created_at: 2024-10-23T13:27:08Z
updated_at: 2024-10-23T13:32:14Z
url: https://github.com/astral-sh/uv/pull/8501
synced_at: 2026-01-12T16:08:21Z
```

# Use `dev-dependencies` and `requires-dev` for lockfile compatibility

---

_@charliermarsh_

## Summary

Per discussion on Discord, we're going to keep these names for now (changed in #8391 but not yet released) for compatibility. They're just cosmetic, but with the changes as-is, if you ran an older uv version over a newer lockfile...

- For `uv sync`: the lockfile would be invalidated, and we'd re-resolve.
- For `uv sync --frozen`: we'd silently skip the development dependencies.

We'll change these names in the future once we've added better error handling around lockfile versions (#8465).

---

_Label `compatibility` added by @charliermarsh on 2024-10-23 13:27_

---

_@zanieb approved on 2024-10-23 13:31_

---

_Merged by @charliermarsh on 2024-10-23 13:32_

---

_Closed by @charliermarsh on 2024-10-23 13:32_

---

_Branch deleted on 2024-10-23 13:32_

---
