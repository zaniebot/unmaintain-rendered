```yaml
number: 8339
title: Allow dashes and underscores in custom index names
type: pull_request
state: merged
author: vinibrsl
labels:
  - enhancement
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-10-18T16:49:12Z
updated_at: 2024-10-18T17:27:15Z
url: https://github.com/astral-sh/uv/pull/8339
synced_at: 2026-01-10T12:54:07Z
```

# Allow dashes and underscores in custom index names

---

_Pull request opened by @vinibrsl on 2024-10-18 16:49_

Previously, `uv add --index` command threw an error when the index name included characters like hyphens or underscores.

Closes #8315

---

_Comment by @zanieb on 2024-10-18 16:54_

Could you add a note to the documentation page about allowed names?

---

_Label `enhancement` added by @zanieb on 2024-10-18 16:54_

---

_Comment by @charliermarsh on 2024-10-18 17:00_

This looks like a reasonable fix -- thank you! It might be nice to add an `IndexName` type like `struct IndexName(String)` and move this validation into `IndexName::from_str`, but it's not critical. We can always do that later.

---

_Comment by @vinibrsl on 2024-10-18 17:03_

Thanks for the quick review!

> Could you add a note to the documentation page about allowed names?

Changed in f6c6c8f.

> This looks like a reasonable fix -- thank you! It might be nice to add an `IndexName` type like `struct IndexName(String)` and move this validation into `IndexName::from_str`, but it's not critical. We can always do that later.

That may be beyond my Rust skills, sorry! But feel free to commit in my branch :-) 

---

_@zanieb approved on 2024-10-18 17:06_

---

_Merged by @charliermarsh on 2024-10-18 17:24_

---

_Closed by @charliermarsh on 2024-10-18 17:24_

---

_Branch deleted on 2024-10-18 17:27_

---
