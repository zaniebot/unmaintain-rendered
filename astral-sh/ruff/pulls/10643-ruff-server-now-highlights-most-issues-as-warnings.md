```yaml
number: 10643
title: "`ruff server` now highlights most issues as warnings"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/correct-diagnostic-severity
created_at: 2024-03-28T05:58:02Z
updated_at: 2024-03-28T11:14:18Z
url: https://github.com/astral-sh/ruff/pull/10643
synced_at: 2026-01-10T22:47:02Z
```

# `ruff server` now highlights most issues as warnings

---

_Pull request opened by @snowsignal on 2024-03-28 05:58_

## Summary

Fixes #10588.

Most diagnostics from `ruff server` now appear as a much less alarming warning instead of an error. Three diagnostics still appear as errors: `F821` (`undefined name <name>`), `E902` (`IOError`) and `E999` (`SyntaxError`).

## Test Plan

With an extension using the path to a locally-built executable, open a file with multiple highlighted problems. Toggle the `Experimental Server` setting on and off. The highlights should stay as warnings.

Then, modify the file to have a syntactically incorrect element. The start of the invalid syntax should now have a red highlight.

---

_Label `server` added by @snowsignal on 2024-03-28 05:58_

---

_Comment by @github-actions[bot] on 2024-03-28 06:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-03-28 07:56_

Is my assumption correct, that this now matches the severity handling of our existing lsp?

---

_@dhruvmanila approved on 2024-03-28 08:06_

---

_Comment by @snowsignal on 2024-03-28 11:14_

> Is my assumption correct, that this now matches the severity handling of our existing lsp?

Correct!

---

_Merged by @snowsignal on 2024-03-28 11:14_

---

_Closed by @snowsignal on 2024-03-28 11:14_

---

_Branch deleted on 2024-03-28 11:14_

---
