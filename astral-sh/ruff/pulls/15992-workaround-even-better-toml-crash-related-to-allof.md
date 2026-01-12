```yaml
number: 15992
title: "Workaround Even Better TOML crash related to `allOf`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: micha/workaround-better-toml-crash
created_at: 2025-02-06T12:02:23Z
updated_at: 2025-02-06T16:47:58Z
url: https://github.com/astral-sh/ruff/pull/15992
synced_at: 2026-01-12T15:55:53Z
```

# Workaround Even Better TOML crash related to `allOf`

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15978

Even Better TOML doesn't support `allOf` well. In fact, it just crashes. 

This PR works around this limitation by avoid using `allOf` in the automatically
derived schema for the docstring formatting setting. 

### Alternatives

schemars introduces `allOf` whenver it sees a `$ref` alongside other object properties
because this is no longer valid according to Draft 7. We could replace the 
visitor performing the rewrite but I prefer not to because replacing `allOf` with `oneOf`
is only valid for objects that don't have any other `oneOf` or `anyOf` schema. 

## Test Plan

https://github.com/user-attachments/assets/25d73b2a-fee1-4ba6-9ffe-869b2c3bc64e





---

_Label `configuration` added by @MichaReiser on 2025-02-06 12:02_

---

_Label `bug` added by @MichaReiser on 2025-02-06 12:02_

---

_Comment by @github-actions[bot] on 2025-02-06 12:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2025-02-06 12:23_

---

_Review requested from @ntBre by @MichaReiser on 2025-02-06 12:23_

---

_@charliermarsh approved on 2025-02-06 14:41_

---

_Merged by @MichaReiser on 2025-02-06 15:00_

---

_Closed by @MichaReiser on 2025-02-06 15:00_

---

_Branch deleted on 2025-02-06 15:00_

---

_@ntBre reviewed on 2025-02-06 16:47_

I'm a bit late, but thanks for tagging me! I hadn't looked into the schema stuff yet.

---
