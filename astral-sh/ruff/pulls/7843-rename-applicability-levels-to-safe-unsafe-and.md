```yaml
number: 7843
title: "Rename applicability levels to `Safe`, `Unsafe`, and `Display`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/applicability-levels
created_at: 2023-10-06T21:46:29Z
updated_at: 2023-10-07T01:50:06Z
url: https://github.com/astral-sh/ruff/pull/7843
synced_at: 2026-01-12T02:32:41Z
```

# Rename applicability levels to `Safe`, `Unsafe`, and `Display`

---

_Pull request opened by @zanieb on 2023-10-06 21:46_

After working with the previous change in https://github.com/astral-sh/ruff/pull/7821 I found the names a bit unclear and their relationship with the user-facing API muddied. Since the applicability is exposed to the user directly in our JSON output, I think it's important that these names align with our configuration options. I've replaced `Manual` or `Never` with `Display` which captures our intent for these fixes (only for display). Here, we create room for future levels, such as `HasPlaceholders`, which wouldn't fit into the `Always`/`Sometimes`/`Never` levels.

Unlike https://github.com/astral-sh/ruff/pull/7819, this retains the flat enum structure which is easier to work with.

---

_Comment by @zanieb on 2023-10-06 21:53_

Sorry for the churn I know this touches a lot of files.

---

_Comment by @github-actions[bot] on 2023-10-06 22:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-10-07 00:18_

I like these names a lot.

---

_Marked ready for review by @zanieb on 2023-10-07 00:41_

---

_Merged by @zanieb on 2023-10-07 01:50_

---

_Closed by @zanieb on 2023-10-07 01:50_

---

_Branch deleted on 2023-10-07 01:50_

---
