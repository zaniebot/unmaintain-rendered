```yaml
number: 7419
title: Move documentation to docs.astral.sh/ruff
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/docs-redirect
created_at: 2023-09-15T20:40:11Z
updated_at: 2023-09-16T02:49:43Z
url: https://github.com/astral-sh/ruff/pull/7419
synced_at: 2026-01-12T15:55:23Z
```

# Move documentation to docs.astral.sh/ruff

---

_@charliermarsh_

## Summary

We're planning to move the documentation from [https://beta.ruff.rs/docs](https://beta.ruff.rs/docs) to [https://docs.astral.sh/ruff](https://docs.astral.sh/ruff), for a few reasons:

1. We want to remove the `beta` from the domain, as Ruff is no longer considered beta software.
2. We want to migrate to a structure that could accommodate multiple future tools living under one domain.

The docs are actually already live at [https://docs.astral.sh/ruff](https://docs.astral.sh/ruff), but later today, I'll add a permanent redirect from the previous to the new domain. **All existing links will continue to work, now and in perpetuity.**

This PR contains the code changes necessary for the updated documentation. As part of this effort, I moved the playground and documentation from my personal Cloudflare account to our team Cloudflare account (hence the new `--project-name` references). After merging, I'll also update the secrets on this repo.


---

_Review requested from @zanieb by @charliermarsh on 2023-09-15 20:40_

---

_Label `documentation` added by @charliermarsh on 2023-09-15 20:40_

---

_@zanieb approved on 2023-09-15 20:54_

---

_Comment by @github-actions[bot] on 2023-09-16 00:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-09-16 02:49_

---

_Closed by @charliermarsh on 2023-09-16 02:49_

---

_Branch deleted on 2023-09-16 02:49_

---
