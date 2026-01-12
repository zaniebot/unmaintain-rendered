```yaml
number: 10800
title: "The linter and code actions can now be disabled in client settings for `ruff server`"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/settings/lint-enable
created_at: 2024-04-06T00:23:42Z
updated_at: 2024-04-08T14:53:29Z
url: https://github.com/astral-sh/ruff/pull/10800
synced_at: 2026-01-12T15:55:33Z
```

# The linter and code actions can now be disabled in client settings for `ruff server`

---

_@snowsignal_

## Summary

This is a follow-up to https://github.com/astral-sh/ruff/pull/10764. Support for diagnostics, quick fixes, and source actions can now be disabled via client settings.

## Test Plan

### Manual Testing

Set up your workspace as described in the test plan in https://github.com/astral-sh/ruff/pull/10764, up to step 2. You don't need to add a debug statement.
The configuration for `folder_a` and `folder_b` should be as follows:
`folder_a`:
```json
{
    "ruff.codeAction.fixViolation": {
        "enable": true
    }
}
```

`folder_b`
```json
{
    "ruff.codeAction.fixViolation": {
        "enable": false
    }
}
```
Finally, open up your VS Code User Settings and un-check the `Ruff > Fix All` setting.

1. Open a Python file in `folder_a` that has existing problems. The problems should be highlighted, and quick fix should be available. `source.fixAll` should not be available as a source action.
2. Open a Python file in `folder_b` that has existing problems. The problems should be highlighted, but quick fixes should not be available for any of them. `source.fixAll` should not be available as a source action.
3. Open up your VS Code Workspace Settings (second tab under the search bar) and un-check `Ruff > Lint: Enable`
4. Both files you tested in steps 1 and 2 should now lack any visible diagnostics. `source.organizeImports` should still be available as a source action.

---

_Label `server` added by @snowsignal on 2024-04-06 00:23_

---

_Renamed from "`ruff server`: Implement enable/disable settings for the linter and code actions" to "`ruff server`: The linter and code actions can now be disabled in settings" by @snowsignal on 2024-04-06 00:24_

---

_Renamed from "`ruff server`: The linter and code actions can now be disabled in settings" to "The linter and code actions can now be disabled in client settings for `ruff server`" by @snowsignal on 2024-04-06 00:25_

---

_Comment by @github-actions[bot] on 2024-04-06 00:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-04-08 08:14_

Test plan: Shouldn't the folder b configuration set `enabled: false`?

---

_@MichaReiser approved on 2024-04-08 08:16_

---

_@dhruvmanila approved on 2024-04-08 12:09_

Tested out in NeoVim 👍 

---

_Comment by @snowsignal on 2024-04-08 14:52_

@MichaReiser You're right about the test plan, my bad. Let me update it.

---

_Merged by @snowsignal on 2024-04-08 14:53_

---

_Closed by @snowsignal on 2024-04-08 14:53_

---

_Branch deleted on 2024-04-08 14:53_

---
