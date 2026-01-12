```yaml
number: 17121
title: Change schema for exclude-newer type into optional string
type: pull_request
state: merged
author: eggplants
labels:
  - enhancement
assignees: []
merged: true
base: main
head: fix-exclude-newer-type
created_at: 2025-12-13T09:04:32Z
updated_at: 2025-12-16T11:52:57Z
url: https://github.com/astral-sh/uv/pull/17121
synced_at: 2026-01-12T16:12:37Z
```

# Change schema for exclude-newer type into optional string

---

_@eggplants_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

fix: #17103 

## Test Plan

The following settings will be enabled for the schema.

```toml
[tool.uv]
exclude-newer = "P7D"
```

---

_Comment by @zanieb on 2025-12-13 15:36_

You can't just edit the schema directly, you need to change the definition in the code.

---

_@zanieb approved on 2025-12-13 19:41_

---

_Merged by @zanieb on 2025-12-13 19:42_

---

_Closed by @zanieb on 2025-12-13 19:42_

---

_Branch deleted on 2025-12-13 19:46_

---

_Renamed from "Change exclude-newer type into optional string" to "Change schema for exclude-newer type into optional string" by @konstin on 2025-12-16 11:52_

---

_Label `enhancement` added by @konstin on 2025-12-16 11:52_

---
