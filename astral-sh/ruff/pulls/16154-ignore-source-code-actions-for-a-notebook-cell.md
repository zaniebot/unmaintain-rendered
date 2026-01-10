```yaml
number: 16154
title: Ignore source code actions for a notebook cell
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/ignore-source-code-action-for-cell
created_at: 2025-02-14T07:04:02Z
updated_at: 2025-02-18T04:58:05Z
url: https://github.com/astral-sh/ruff/pull/16154
synced_at: 2026-01-10T19:57:22Z
```

# Ignore source code actions for a notebook cell

---

_Pull request opened by @dhruvmanila on 2025-02-14 07:04_

## Summary

Related to https://github.com/astral-sh/ruff-vscode/pull/686, this PR ignores handling source code actions for notebooks which are not prefixed with `notebook`.

The main motivation is that the native server does not actually handle it well which results in gibberish code. There's some context about this in https://github.com/astral-sh/ruff-vscode/issues/680#issuecomment-2647490812 and the following comments.

closes: https://github.com/astral-sh/ruff-vscode/issues/680

## Test Plan

Running a notebook with the following does nothing except log the message:
```json
  "notebook.codeActionsOnSave": {
    "source.organizeImports.ruff": "explicit",
  },
```

while, including the `notebook` code actions does make the edit (as usual):
```json
  "notebook.codeActionsOnSave": {
    "notebook.source.organizeImports.ruff": "explicit"
  },
```


---

_Label `server` added by @dhruvmanila on 2025-02-14 07:04_

---

_Comment by @github-actions[bot] on 2025-02-14 07:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @dhruvmanila on 2025-02-14 13:44_

---

_Marked ready for review by @dhruvmanila on 2025-02-14 13:44_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-14 13:44_

---

_@MichaReiser reviewed on 2025-02-14 15:03_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:56 on 2025-02-14 15:03_

Can we add some inline comment explaining why we're ignoring those actions or link to the relevant issue? It's otherwise somewhat confusing when reading this in the future.


Related: Should we use a warn or `info` severity here so that it's visible by default? Should the message include a hint of what the user should do instead or is this expected inside of notebooks?

---

_@MichaReiser approved on 2025-02-14 15:03_

---

_@dhruvmanila reviewed on 2025-02-17 11:06_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/code_action.rs`:56 on 2025-02-17 11:06_

> Can we add some inline comment explaining why we're ignoring those actions or link to the relevant issue? It's otherwise somewhat confusing when reading this in the future.

Yes, will do so.

> Related: Should we use a warn or `info` severity here so that it's visible by default? Should the message include a hint of what the user should do instead or is this expected inside of notebooks?

The reason I'm not using a warn or info severity is that this message will quickly fill up the log file because the code actions are request for cursor movements and possibly even while typing.

---

_@dhruvmanila reviewed on 2025-02-17 11:11_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/code_action.rs`:56 on 2025-02-17 11:11_

Regarding the hint, I'm not even sure if there are any editors other than VS Code-like ones that support notebooks natively along with the LSP capabilities so I'm hesitant to provide any suggestions here.

---

_Merged by @dhruvmanila on 2025-02-18 04:58_

---

_Closed by @dhruvmanila on 2025-02-18 04:58_

---

_Branch deleted on 2025-02-18 04:58_

---
