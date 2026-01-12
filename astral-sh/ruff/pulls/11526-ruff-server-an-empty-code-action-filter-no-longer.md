```yaml
number: 11526
title: "`ruff server`: An empty code action filter no longer returns notebook source actions"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: jane/server/no-duplicate-code-actions
created_at: 2024-05-24T06:51:08Z
updated_at: 2024-05-24T07:30:27Z
url: https://github.com/astral-sh/ruff/pull/11526
synced_at: 2026-01-12T15:55:38Z
```

# `ruff server`: An empty code action filter no longer returns notebook source actions

---

_@snowsignal_

## Summary

Fixes #11516

`ruff server` was sending both regular source actions and notebook source actions back when passed an empty action filter. This PR makes a few small changes so that notebook source actions are not sent when regular source actions are sent, which means that an empty filter will only return regular source actions.

## Test Plan

I confirmed that duplicate code actions no longer appeared in Neovim, using a configuration similar to the one from the original issue.

<img width="509" alt="Screenshot 2024-05-23 at 11 48 48 PM" src="https://github.com/astral-sh/ruff/assets/19577865/9a5d6907-dd41-48bd-b015-8a344c5e0b3f">


---

_Label `bug` added by @snowsignal on 2024-05-24 06:51_

---

_Label `server` added by @snowsignal on 2024-05-24 06:51_

---

_Comment by @github-actions[bot] on 2024-05-24 07:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/code_action.rs`:60 on 2024-05-24 07:12_

nit: can we combine this under a common condition?

```rs
        if snapshot.client_settings().fix_all() {
            if supported_code_actions.contains(&SupportedCodeAction::SourceFixAll) {
                response.push(fix_all(&snapshot).with_failure_code(ErrorCode::InternalError)?);
            } else if supported_code_actions.contains(&SupportedCodeAction::NotebookSourceFixAll) {
                response
                    .push(notebook_fix_all(&snapshot).with_failure_code(ErrorCode::InternalError)?);
            }
        }
```

---

_@dhruvmanila approved on 2024-05-24 07:13_

---

_Merged by @snowsignal on 2024-05-24 07:20_

---

_Closed by @snowsignal on 2024-05-24 07:20_

---

_Branch deleted on 2024-05-24 07:20_

---
