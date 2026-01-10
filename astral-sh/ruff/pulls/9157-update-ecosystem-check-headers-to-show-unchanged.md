```yaml
number: 9157
title: Update ecosystem check headers to show unchanged project count
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/ecosystem-changed-projects
created_at: 2023-12-16T03:29:46Z
updated_at: 2023-12-16T06:05:40Z
url: https://github.com/astral-sh/ruff/pull/9157
synced_at: 2026-01-10T23:31:11Z
```

# Update ecosystem check headers to show unchanged project count

---

_Pull request opened by @zanieb on 2023-12-16 03:29_

Instead of displaying the total completed project count in the "changed" section of a header, we now separately calculated the changed and unchanged count to make the header message nice and clear.

e.g.

> ℹ️ ecosystem check **detected format changes**. (+1772 -1859 lines in 239 files in 26 projects; 6 project errors; 9 projects unchanged)

and

> ℹ️ ecosystem check **detected linter changes**. (+4598 -5023 violations, +0 -40 fixes in 13 projects; 4 project errors; 24 projects unchanged)


Previously, it would have included the unchanged count in the first project count.

---

_Label `ci` added by @zanieb on 2023-12-16 03:32_

---

_Comment by @github-actions[bot] on 2023-12-16 03:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila approved on 2023-12-16 04:40_

---

_Comment by @T-256 on 2023-12-16 04:40_

My suggestion for new phrasing:

> ℹ️ 26 projects changes +1772 -1859 lines in 239 files (6 project errors; 9 projects unchanged)

and

> ℹ️ 13 projects changes +4598 -5023 violations, +0 -40 fixes (4 project errors; 24 projects unchanged)

---

_Comment by @zanieb on 2023-12-16 06:05_

Hm I prefer that it goes from specific -> general consistently right now but there is something to that. I'll mull over better phrasing in general! I'd be nice to do something more readable like what they do in Black's CI reports.

---

_Merged by @zanieb on 2023-12-16 06:05_

---

_Closed by @zanieb on 2023-12-16 06:05_

---

_Branch deleted on 2023-12-16 06:05_

---
