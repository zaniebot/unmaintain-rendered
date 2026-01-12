```yaml
number: 15666
title: "Consider `unsafe-fixes` settings for code actions"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/server-unsafe-fixes
created_at: 2025-01-22T05:58:38Z
updated_at: 2025-01-22T08:14:15Z
url: https://github.com/astral-sh/ruff/pull/15666
synced_at: 2026-01-12T15:55:52Z
```

# Consider `unsafe-fixes` settings for code actions

---

_@dhruvmanila_

## Summary

Closes: #13960 

## Test Plan

Using the example from https://github.com/astral-sh/ruff-vscode/issues/672:

https://github.com/user-attachments/assets/7bdb01ef-8752-4cb7-9b5d-8a0d131984da




---

_Label `bug` added by @dhruvmanila on 2025-01-22 05:58_

---

_Label `server` added by @dhruvmanila on 2025-01-22 05:58_

---

_Comment by @github-actions[bot] on 2025-01-22 06:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-22 06:09_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:155 on 2025-01-22 07:01_

Do you remember what's the reason for only storing parts of the settings instead of the entire `settings` struct with accessors for the linter/formatter settings?

---

_@MichaReiser approved on 2025-01-22 07:01_

---

_@MichaReiser approved on 2025-01-22 07:02_

Thanks. This looks good. It makes me wonder if we should simply store the entire `Settings` struct in `RuffSettings`. It seems like we eventually will need all of them anyway?

---

_@dhruvmanila reviewed on 2025-01-22 08:11_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:155 on 2025-01-22 08:11_

I think it started out with just having `LinterSettings` and `FormatterSettings` (https://github.com/astral-sh/ruff/pull/10950) but later on we realized (via bug reports) that exclusions weren't being considered by the server which led to adding the `FileResolverSettings` (https://github.com/astral-sh/ruff/pull/11551).

> It makes me wonder if we should simply store the entire `Settings` struct in `RuffSettings`. It seems like we eventually will need all of them anyway?

Yeah, I agree. I can do that as a follow-up.

---

_Merged by @dhruvmanila on 2025-01-22 08:14_

---

_Closed by @dhruvmanila on 2025-01-22 08:14_

---

_Branch deleted on 2025-01-22 08:14_

---
