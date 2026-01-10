```yaml
number: 15844
title: "Remove non-existing `lint.extendIgnore` editor setting"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/fix-editor-docs
created_at: 2025-01-31T05:56:48Z
updated_at: 2025-01-31T06:00:18Z
url: https://github.com/astral-sh/ruff/pull/15844
synced_at: 2026-01-10T19:57:22Z
```

# Remove non-existing `lint.extendIgnore` editor setting

---

_Pull request opened by @dhruvmanila on 2025-01-31 05:56_

This setting doesn't exist in the first place. I must've added it by mistake thinking that it exists similar to `extendSelect`. One reason to have auto-generated docs.

https://github.com/astral-sh/ruff/blob/988be01fbec37fe6ba6658a24cda82c73241e4ea/crates/ruff_server/src/session/settings.rs#L124-L133

Closes: #14665 

---

_Label `documentation` added by @dhruvmanila on 2025-01-31 05:56_

---

_Merged by @dhruvmanila on 2025-01-31 06:00_

---

_Closed by @dhruvmanila on 2025-01-31 06:00_

---

_Branch deleted on 2025-01-31 06:00_

---
