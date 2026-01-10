```yaml
number: 18309
title: "[ty] Support publishing diagnostics in the server"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/publish-diagnostics
created_at: 2025-05-26T04:26:24Z
updated_at: 2025-05-28T07:45:12Z
url: https://github.com/astral-sh/ruff/pull/18309
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Support publishing diagnostics in the server

---

_Pull request opened by @dhruvmanila on 2025-05-26 04:26_

## Summary

This PR adds support for [publishing diagnostics](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_publishDiagnostics) from the ty language server.

It only adds support for it for text documents and not notebook documents because the server doesn't have full notebook support yet.

Closes: astral-sh/ty#79

## Test Plan

Testing this out in Helix and Zed since those are the two editors that I know of that doesn't support pull diagnostics:

### Helix

https://github.com/user-attachments/assets/e193f804-0b32-4f7e-8b83-6f9307e3d2d4



### Zed


https://github.com/user-attachments/assets/93ec7169-ce2b-4521-b009-a82d8afb9eaa



---

_Label `server` added by @dhruvmanila on 2025-05-26 04:26_

---

_Label `ty` added by @dhruvmanila on 2025-05-26 04:26_

---

_Comment by @github-actions[bot] on 2025-05-26 04:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-05-26 05:17_

---

_Review requested from @carljm by @dhruvmanila on 2025-05-26 05:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-05-26 05:17_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-05-26 05:17_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-05-26 05:17_

---

_Review requested from @dcreager by @dhruvmanila on 2025-05-26 05:17_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-05-26 05:18_

---

_Review request for @carljm removed by @dhruvmanila on 2025-05-26 05:18_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-05-26 05:18_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-05-26 05:18_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:82 on 2025-05-26 06:59_

Delete

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:99 on 2025-05-26 06:59_

Is `usize::default` correct here? What is it used for? I also don't understand the comment beauase it's unclear to me where we use source kind here

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_change.rs`:62 on 2025-05-26 07:01_

Could we add a `path_db` or similar method to `session` that handles the db lookup internally 

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:58 on 2025-05-26 07:02_

The code here seems to be the same as in `did_change`. Can we move it into a shared method?

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:47 on 2025-05-26 07:04_

It would be useful to have a comment explaining that all `uri`s belong to the same file because I was worried at first what would happen if `check_file(db, document_a`) returns two diagnostics for file `b` but `check_file(db, document_b)` returns 4, in which case publishing the diagnostics would override each other.

---

_@MichaReiser approved on 2025-05-26 07:05_

Thanks. This overall looks good. 

I think there's an opportunity to reduce code duplication a bit and there are some code paths that I don't understand (see inline comments)

---

_@MichaReiser reviewed on 2025-05-26 07:12_

Oh, we also need to think about how this integrates with `didWatchedFilesChange`. I don't think it's necessary to call publish diagnostic if a single python file changed because that file isn't open. But we have to re-publish all diagnostics when the configuration changed

---

_Review requested from @MichaReiser by @MichaReiser on 2025-05-26 07:12_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:99 on 2025-05-26 13:09_

We would need to use `source_kind` (which is currently commented out) to get the `NotebookIndex` which will be used to get the cell index in which the diagnostics occur. This index will be used to get the URI for that cell to publish the diagnostics for.

---

_@dhruvmanila reviewed on 2025-05-26 13:09_

---

_@dhruvmanila reviewed on 2025-05-26 13:11_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:47 on 2025-05-26 13:11_

Yeah, I can do that. You're correct that the URI here belong to the same document, it's mainly to accommodate the different text documents that represents the cell of a notebook. I could use an enum to make this more concrete like:

```rs
enum Diagnostics {
  TextDocument(Url, Vec<Diagnostic>),
  NotebookDocument(FxHashMap<Url, Vec<Diagnostic>>),
}
```

---

_@dhruvmanila reviewed on 2025-05-27 00:10_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:58 on 2025-05-27 00:10_

I could move it to `Session` so that it's `session.publish_diagnostics` but the main issue is that the data structure that's required for this is part of `server` crate and not the `session` crate. I could just expose them at the `ty_server` crate level but I'm not sure if `session` should depend on `server` but `server` _already_ depends on `session` crate as `Server` contains the `Session`.

---

_@dhruvmanila reviewed on 2025-05-27 00:12_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change.rs`:62 on 2025-05-27 00:12_

Added `Session::project_db_or_default`

---

_@MichaReiser reviewed on 2025-05-27 04:54_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:58 on 2025-05-27 04:54_

Sorry, if I was misleading. A standalone function is fine too

---

_Comment by @dhruvmanila on 2025-05-27 07:34_

Video testing that any changes to configuration files refreshes the diagnostics via publish mechanism in Zed:

https://github.com/user-attachments/assets/433008db-fe8e-4f8c-afe8-8dfa9bae4faa



---

_@MichaReiser reviewed on 2025-05-27 09:04_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:66 on 2025-05-27 09:04_

I'd prefer if we put this into `apply_changes` to avoid duplicating the logic when a project has changed.

---

_@dhruvmanila reviewed on 2025-05-27 10:13_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:66 on 2025-05-27 10:13_

I think this change already exists in `apply_changes`

https://github.com/astral-sh/ruff/blob/cd1fd80f595b3e41943d11f8479b34460d446bd1/crates/ty_project/src/db/changes.rs#L45-L53

, this is present here mainly to check whether we need to refresh the diagnostics or not as the `didChangeWatchedFiles` is registered for `.py`, `.pyi`, and `.ipynb` files as well for which we shouldn't need to do that.

---

_@MichaReiser reviewed on 2025-05-27 10:23_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:66 on 2025-05-27 10:23_

Yes. I'd prefer if we wouldn't duplicate the logic here and instead add a return value to `apply_changes` indicating what changed. There are other cases where it assumes a `project_changed` event that aren't covered by what you have in `did_change_watched_files`.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:66 on 2025-05-28 05:59_

Got it, I've made the change to return a `ChangeResult` that contains boolean fields to indicate what changes were detected by the `apply_changes` method.

---

_@dhruvmanila reviewed on 2025-05-28 06:00_

---

_@dhruvmanila reviewed on 2025-05-28 06:19_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:58 on 2025-05-28 06:19_

No, I think I misunderstood. I've added `publish_diagnostics` in [`fc2d725` (#18309)](https://github.com/astral-sh/ruff/pull/18309/commits/fc2d72500238fb3d92b3b1d5d4ae7bdd2601151a) although it does make call to `url_to_any_system_path` twice for `didOpen` and `didChange` handler (not for `didChangeWatchedFiles` handler).

---

_@dhruvmanila reviewed on 2025-05-28 06:20_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:97 on 2025-05-28 06:20_

@MichaReiser can you verify this comment?

---

_@dhruvmanila reviewed on 2025-05-28 06:21_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:101 on 2025-05-28 06:21_

I think this should be fine even after we add support for multiple workspaces.

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-05-28 06:22_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:22 on 2025-05-28 06:30_

I don't think this comment is accurate. We set `project_changed` to `true `for changes that *may have* changed the project structure. There's no guarantee for it. I would remove the part about files were added or removed because this is only the case when:

* The file watcher sends a rescan event -> which means the file watcher is out of sync
* A directory was deleted that was likely to contain the project configuration.

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:281 on 2025-05-28 06:30_


```suggestion
        result
```

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:97 on 2025-05-28 06:32_

I think this is accurate. It may change in the future but we'll then need to change this line :)

---

_@MichaReiser approved on 2025-05-28 06:33_

---

_Merged by @dhruvmanila on 2025-05-28 07:45_

---

_Closed by @dhruvmanila on 2025-05-28 07:45_

---

_Branch deleted on 2025-05-28 07:45_

---
