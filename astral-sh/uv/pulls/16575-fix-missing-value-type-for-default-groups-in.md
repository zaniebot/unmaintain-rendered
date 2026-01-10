```yaml
number: 16575
title: "Fix missing value_type for `default-groups` in schema"
type: pull_request
state: merged
author: fllesser
labels: []
assignees: []
merged: true
base: main
head: uv-schema/default-groups
created_at: 2025-11-03T06:57:26Z
updated_at: 2025-11-03T14:00:38Z
url: https://github.com/astral-sh/uv/pull/16575
synced_at: 2026-01-10T06:28:12Z
```

# Fix missing value_type for `default-groups` in schema

---

_Pull request opened by @fllesser on 2025-11-03 06:57_

## Summary

Fix incomplete value_type attribute for default-groups field in the ToolUv struct schema definition. The value_type was missing its value, which should be str | list[str] to reflect that default-groups can accept either the literal "all" or a list of group names. (#16574)





---

_@ssbarnea approved on 2025-11-03 11:27_

---

_@ssbarnea reviewed on 2025-11-03 11:31_

Not sure when this regression happened but switching case is breaking. I had to temporary stop using the default-groups feature because it became a source of instability.

---

_Comment by @charliermarsh on 2025-11-03 13:54_

@ssbarnea - Sorry, what is breaking? This just appears to be a change in the schema.

---

_@charliermarsh approved on 2025-11-03 13:55_

---

_Merged by @charliermarsh on 2025-11-03 13:55_

---

_Closed by @charliermarsh on 2025-11-03 13:55_

---

_Branch deleted on 2025-11-03 14:00_

---
