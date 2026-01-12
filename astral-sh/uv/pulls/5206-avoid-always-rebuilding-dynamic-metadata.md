```yaml
number: 5206
title: Avoid always rebuilding dynamic metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - performance
assignees: []
merged: true
base: main
head: charlie/dynamic
created_at: 2024-07-19T01:21:05Z
updated_at: 2024-07-23T18:41:14Z
url: https://github.com/astral-sh/uv/pull/5206
synced_at: 2026-01-12T16:06:41Z
```

# Avoid always rebuilding dynamic metadata

---

_@charliermarsh_

## Summary

I don't think that "always reinstall" is tenable for `uv run`. My perspective on this is that if you want "always reinstall", you can now set it persistently in your `pyproject.toml` or `uv.toml`.

As a smaller change, we could instead disable this _only_ for the Project API.

Closes https://github.com/astral-sh/uv/issues/4946.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-19 01:21_

---

_Review requested from @konstin by @charliermarsh on 2024-07-19 01:21_

---

_Marked ready for review by @charliermarsh on 2024-07-19 01:21_

---

_Label `breaking` added by @charliermarsh on 2024-07-19 01:22_

---

_@konstin approved on 2024-07-19 08:40_

---

_Merged by @charliermarsh on 2024-07-22 00:04_

---

_Closed by @charliermarsh on 2024-07-22 00:04_

---

_Branch deleted on 2024-07-22 00:04_

---

_Label `breaking` removed by @zanieb on 2024-07-23 18:41_

---

_Label `enhancement` added by @zanieb on 2024-07-23 18:41_

---

_Label `performance` added by @zanieb on 2024-07-23 18:41_

---
