```yaml
number: 19322
title: "[ty] Add virtual files to the only project database"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/virtual-files-in-project-db
created_at: 2025-07-14T08:37:50Z
updated_at: 2025-07-14T14:47:53Z
url: https://github.com/astral-sh/ruff/pull/19322
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Add virtual files to the only project database

---

_Pull request opened by @dhruvmanila on 2025-07-14 08:37_

## Summary

Previously, the virtual files were being added to the default database that's present on the session. This is wrong because the default database is for any files that don't belong to any project i.e., they're outside of any projects managed by the server. Virtual files are neither part of the project nor it is outside the projects. This was not the intention as in the initial version, virtual files were being added to the only project database managed by the server.

This PR fixes this by reverting back to the original behavior where virtual files will be added to the only project database present. When support for multiple workspace and project is added, this will require updating (https://github.com/astral-sh/ty/issues/794).

This is required for #19264 because workspace diagnostics doesn't check the default project database yet. Ideally, the default db should be checked as well.

The implementation of this PR means that virtual files are now being included for workspace diagnostics but it doesn't work completely e.g., if I save an untitled file the diagnostics disappears but it doesn't appear back for the (now) saved file on disk as shown in the following video demonstration:


https://github.com/user-attachments/assets/123e8d20-1e95-4c7d-b7eb-eb65be8c476e



---

_Review requested from @carljm by @dhruvmanila on 2025-07-14 08:37_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-14 08:37_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-14 08:37_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-14 08:37_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-14 08:37_

---

_Label `internal` added by @dhruvmanila on 2025-07-14 08:37_

---

_Label `ty` added by @dhruvmanila on 2025-07-14 08:37_

---

_Label `server` added by @dhruvmanila on 2025-07-14 08:37_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-07-14 08:38_

---

_Review request for @carljm removed by @dhruvmanila on 2025-07-14 08:38_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-07-14 08:38_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-07-14 08:38_

---

_Comment by @github-actions[bot] on 2025-07-14 08:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `internal` removed by @dhruvmanila on 2025-07-14 08:52_

---

_Comment by @dhruvmanila on 2025-07-14 08:52_

Actually, this is not an "internal" change as the server now starts checking virtual files in workspace diagnostic mode.

---

_Comment by @dhruvmanila on 2025-07-14 09:06_

> The implementation of this PR means that virtual files are now being included for workspace diagnostics but it doesn't work completely e.g., if I save an untitled file the diagnostics disappears but it doesn't appear back for the (now) saved file on disk as shown in the following video demonstration:

I think the reason the other case isn't working is that the file doesn't exists on disk and so the `File`'s initial status is `NotFound`. This is based on the warning that I get in #19264:

```
2025-07-14 14:17:26.316681000  WARN notification{method="textDocument/didOpen"}: Failed to create a salsa file for /Users/dhruv/playground/ty-server/src/ty_server/other.py
```

Now, the linked PR does fix the issue here which I'm not exactly sure how.

---

_Comment by @MichaReiser on 2025-07-14 11:14_

> This is required for https://github.com/astral-sh/ruff/pull/19264 because workspace diagnostics doesn't check the default project database yet. Ideally, the default db should be checked as well.

I don't think this will be necessary. We only use the default database so that we can provide basic LSP features for those files. I don't think we have to provide diagnostics.

> I think the reason the other case isn't working is that the file doesn't exists on disk and so the File's initial status is NotFound. This is based on the warning that I get in https://github.com/astral-sh/ruff/pull/19264:

I'm not sure where this message is logged which makes it difficult to help. It probably makes sense to add a log statement to `system_path_to_file` to see why it returns `None` and if it's because the status is `NotFound`, why `apply_changes` doesn't update the file status.

---

_@MichaReiser approved on 2025-07-14 14:13_

---

_Comment by @dhruvmanila on 2025-07-14 14:36_

Somehow, this change is also initializing the default database when I open a virtual file and select the Python language for that virtual file.

---

_Merged by @dhruvmanila on 2025-07-14 14:47_

---

_Closed by @dhruvmanila on 2025-07-14 14:47_

---

_Branch deleted on 2025-07-14 14:47_

---
