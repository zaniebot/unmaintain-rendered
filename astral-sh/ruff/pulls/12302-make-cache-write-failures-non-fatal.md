```yaml
number: 12302
title: Make cache-write failures non-fatal
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/caches
created_at: 2024-07-12T12:58:56Z
updated_at: 2024-07-13T07:53:58Z
url: https://github.com/astral-sh/ruff/pull/12302
synced_at: 2026-01-12T15:55:40Z
```

# Make cache-write failures non-fatal

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12284.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-07-12 12:59_

---

_Label `bug` added by @charliermarsh on 2024-07-12 12:59_

---

_Label `windows` added by @charliermarsh on 2024-07-12 12:59_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-07-12 13:00_

---

_Converted to draft by @charliermarsh on 2024-07-12 13:03_

---

_Comment by @charliermarsh on 2024-07-12 13:03_

One sec, making some changes.

---

_Marked ready for review by @charliermarsh on 2024-07-12 13:07_

---

_@charliermarsh reviewed on 2024-07-12 13:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/cache.rs`:183 on 2024-07-12 13:08_

The `open` method on this struct already uses a similar pattern of warning (but ignoring) some errors.

---

_Comment by @github-actions[bot] on 2024-07-12 13:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-07-12 14:31_

---

_Comment by @zanieb on 2024-07-12 14:31_

Should we be retrying like we do in uv?

---

_Comment by @charliermarsh on 2024-07-12 14:33_

I don't think we should in this case because it's not a guard against anti-virus stuff like with binaries; it's a guard against the file being in-use, and it's not clear how long we'd expect that to be true or whether we should expect it to resolve.

---

_Merged by @charliermarsh on 2024-07-12 14:33_

---

_Closed by @charliermarsh on 2024-07-12 14:33_

---

_Branch deleted on 2024-07-12 14:33_

---

_Comment by @jaraco on 2024-07-13 07:53_

Thanks for the fast fix (and the fast release cadence so I don't have to inquire about when we might get the fix)!

---
