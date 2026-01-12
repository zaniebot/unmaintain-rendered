```yaml
number: 13044
title: "[red-knot] Support untitled files in the server"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/lsp-virtual-file
created_at: 2024-08-22T06:58:48Z
updated_at: 2024-08-23T07:20:06Z
url: https://github.com/astral-sh/ruff/pull/13044
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Support untitled files in the server

---

_@dhruvmanila_

## Summary

This PR adds support for untitled files in the red knot server.

## Test Plan


https://github.com/user-attachments/assets/57fa5db6-e1ad-4694-ae5f-c47a21eaa82b




---

_Label `red-knot` added by @dhruvmanila on 2024-08-22 06:58_

---

_@dhruvmanila reviewed on 2024-08-22 11:43_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/requests/diagnostic.rs`:76 on 2024-08-22 11:43_

I think this should be temporary. This is because the virtual path would contain the entire URL (`untitled:Untitled-1`) which means the message would have an extra `:`

---

_@dhruvmanila reviewed on 2024-08-22 11:44_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/session.rs`:278 on 2024-08-22 11:44_

I thought about adding `File` to the `DocumentController` but my concern is about notebook cells which also uses `TextDocument` to represent them. I'm not exactly sure how to represent them. I do want to follow-up on this when adding support for notebooks.

---

_@dhruvmanila reviewed on 2024-08-22 11:45_

---

_Review comment by @dhruvmanila on `crates/red_knot_workspace/src/db/changes.rs`:143 on 2024-08-22 11:45_

Or, should we expose a `close_virtual_file` on `Files` instead?

---

_Marked ready for review by @dhruvmanila on 2024-08-22 11:45_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-22 11:45_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-22 11:45_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-22 11:45_

---

_Comment by @github-actions[bot] on 2024-08-22 11:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_server/src/system.rs`:30 on 2024-08-22 12:10_

What are the cases where `url.to_file_path()` can fail if the schema is `file`? 

It would be nice if this method wouldn't have to return a `Result`.

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/watch.rs`:81 on 2024-08-22 12:11_

Nit: Rename the method to `system_path`

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 12:15_

I think my preferred solution would be for `Index` to directly store a reference to `VirtualFile` or `File`. But I think you had the valid concern that the `Index` is independent of the `Workspace` and closing the workspace would result in having `File` pointers that point to a collected database. Although we could consider this a bug because the LSP should send a close event for all files when closing a workspace?

The benefit of storing the `File` on the `DocumentController` is that we can remove the need to convert between URLs. 

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_change.rs`:47 on 2024-08-22 12:17_

This is more a note to myself. It's unfortunate that we need to go through `apply_changes` here which requires going from `file -> path` but this seems necessary when they e.g. open a `pyproject.toml` (and we support linting it in the future)

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/session.rs`:278 on 2024-08-22 12:19_

Ah okay :) 



---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/db/changes.rs`:143 on 2024-08-22 12:19_

I think this is fine for now

---

_@MichaReiser approved on 2024-08-22 12:19_

---

_@MichaReiser reviewed on 2024-08-22 12:19_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 12:19_

I saw your comment below.

---

_@dhruvmanila reviewed on 2024-08-23 07:02_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/system.rs`:30 on 2024-08-23 07:02_

From the docs (https://docs.rs/url/2.5.2/url/struct.Url.html#method.to_file_path):

> Returns `Err` if the host is neither empty nor `"localhost"` (except on Windows, where `file:` URLs may have a non-local host), or if `Path::new_opt()` returns `None`. (That is, if the percent-decoded path contains a NUL byte or, for a Windows path, is not UTF-8.)


---

_@dhruvmanila reviewed on 2024-08-23 07:14_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/system.rs`:30 on 2024-08-23 07:14_

Not sure what `Path::new_opt` is but the code doesn't seem to be doing that.

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/system.rs`:30 on 2024-08-23 07:16_

Going to merge the PR, will look at this in a follow-up.

---

_@dhruvmanila reviewed on 2024-08-23 07:16_

---

_Merged by @dhruvmanila on 2024-08-23 07:17_

---

_Closed by @dhruvmanila on 2024-08-23 07:17_

---

_Branch deleted on 2024-08-23 07:17_

---
