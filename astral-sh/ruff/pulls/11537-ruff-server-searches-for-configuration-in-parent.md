```yaml
number: 11537
title: "`ruff server` searches for configuration in parent directories"
type: pull_request
state: merged
author: snowsignal
labels:
  - configuration
  - server
assignees: []
merged: true
base: main
head: jane/server/fix-config-search
created_at: 2024-05-24T22:16:49Z
updated_at: 2024-05-26T18:20:48Z
url: https://github.com/astral-sh/ruff/pull/11537
synced_at: 2026-01-12T15:55:38Z
```

# `ruff server` searches for configuration in parent directories

---

_@snowsignal_

## Summary

Fixes #11506.

`RuffSettingsIndex::new` now searches for configuration files in parent directories.

## Test Plan

I confirmed that the original test case described in the issue worked as expected.


---

_Label `server` added by @snowsignal on 2024-05-24 22:16_

---

_Label `configuration` added by @snowsignal on 2024-05-24 22:17_

---

_Comment by @github-actions[bot] on 2024-05-24 22:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-05-26 17:43_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/index/ruff_settings.rs`:91 on 2024-05-26 17:43_

I think we should only do this once, for the root path.

---

_@charliermarsh reviewed on 2024-05-26 17:44_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/index/ruff_settings.rs`:91 on 2024-05-26 17:44_

There's no need to search in parent directories for _every_ directory, since there's at most one pyproject above the root path.

---

_Merged by @charliermarsh on 2024-05-26 18:11_

---

_Closed by @charliermarsh on 2024-05-26 18:11_

---

_Branch deleted on 2024-05-26 18:11_

---
