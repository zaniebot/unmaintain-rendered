```yaml
number: 19335
title: "[ty] publish settings diagnostics"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: gankra/global-err
created_at: 2025-07-14T16:49:00Z
updated_at: 2025-07-17T15:57:02Z
url: https://github.com/astral-sh/ruff/pull/19335
synced_at: 2026-01-10T17:58:13Z
```

# [ty] publish settings diagnostics

---

_Pull request opened by @Gankra on 2025-07-14 16:49_

_No description provided._

---

_Review requested from @carljm by @Gankra on 2025-07-14 16:49_

---

_Review requested from @MichaReiser by @Gankra on 2025-07-14 16:49_

---

_Label `server` added by @Gankra on 2025-07-14 16:49_

---

_Review requested from @AlexWaygood by @Gankra on 2025-07-14 16:49_

---

_Review requested from @sharkdp by @Gankra on 2025-07-14 16:49_

---

_Review requested from @dcreager by @Gankra on 2025-07-14 16:49_

---

_Label `ty` added by @Gankra on 2025-07-14 16:49_

---

_Label `diagnostics` added by @Gankra on 2025-07-14 16:49_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-14 16:50_

---

_Comment by @Gankra on 2025-07-14 16:50_

Diagnostics now properly show up like this:

<img width="1280" height="464" alt="Screenshot 2025-07-14 at 12 50 09 PM" src="https://github.com/user-attachments/assets/40770924-d50a-4819-9853-ab215e94a488" />

And clear out whenever the issue is fixed.


---

_Comment by @Gankra on 2025-07-14 16:55_

Oh boy micha really merge conflicted me bad h'ok...

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:59 on 2025-07-14 16:57_

Can you tell me why we need to publish settings diagnostics during `did_open` and `did_change`? Is it that the diagnostics show up after starting the server? If so, here might be a better place for it: 

https://github.com/astral-sh/ruff/blob/b4c42eb83b1215ad980a11265f38d84bdeba7e28/crates/ty_server/src/session.rs#L273-L285

Edit: I just saw that you already have a handler on that line. I guess it's simply not clear to me why we need this. I'd expect that `did_change_watched_files` is good enough

---

_@MichaReiser reviewed on 2025-07-14 16:57_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:117 on 2025-07-14 17:01_

I wonder if we should change this method to take a `&mut Project` instead. It should be unlikely that a user changes all projects at once. It's more likely that they changed a setting for a specific project. 

This would also ensure that we don't publish diagnostics for the default database (it should never have setting diagnostics but initializing it isn't entirely free)

---

_@MichaReiser reviewed on 2025-07-14 17:01_

---

_Comment by @MichaReiser on 2025-07-14 17:05_

> Oh boy micha really merge conflicted me bad h'ok...

Lol sorry. But not all of the merge conflicts are because of me. Some are because of Dhruv ;)

---

_Comment by @Gankra on 2025-07-14 17:11_

Rebased

---

_@Gankra reviewed on 2025-07-14 17:13_

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/notifications/did_open.rs`:59 on 2025-07-14 17:13_

The code for apply_changes was complex enough to me that it wasn't clear that these entry points *couldn't* trigger a reparse of the ProjectMetadata, and the overhead of reprocessing the settings diagnostics seemed negligible (since 99.9% of the time there are no diagnostics and we do nothing).

---

_Comment by @github-actions[bot] on 2025-07-14 17:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @Gankra on `crates/ty_server/src/server/api/diagnostics.rs`:117 on 2025-07-14 17:16_

Huh, really? the default project really can't have any settings diagnostics?

---

_@Gankra reviewed on 2025-07-14 17:16_

---

_@Gankra reviewed on 2025-07-14 17:18_

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/diagnostics.rs`:163 on 2025-07-14 17:18_

Any insight on this point would be appreciated

---

_@Gankra reviewed on 2025-07-14 17:20_

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/diagnostics.rs`:117 on 2025-07-14 17:20_

Also the current signature is "required" by the design of DidChangeWatchedFiles which processes random changes to random databases all together, producing only a single project_changed boolean. I could indeed make this more granular if we tracked project_changed per-project (but it's not clear to me it's worth the effort?).

---

_@MichaReiser reviewed on 2025-07-14 17:35_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:59 on 2025-07-14 17:35_

My reasoning here would be that: Given that setting files are files that aren't tracked by the LSP, `didOpen` or `didChange` can never impact a setting file. 

---

_Comment by @MichaReiser on 2025-07-14 17:35_

I'll do a full review tomorrow as it's late here. 

---

_@MichaReiser reviewed on 2025-07-14 17:37_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:163 on 2025-07-14 17:37_

You can get it from session (Session::position_encoding) and it's passed to all request handlers as an explicit argument. You might need to add a getter on `Session`

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:117 on 2025-07-15 09:06_

`did_change_watched_files` does group all files by database and then calls `apply_changes` exacltly once per database. See this loop where `root `maps to the root of a database:


https://github.com/astral-sh/ruff/blob/fe8f0cfcf9760d39aa716deb180678bd72cd3aba/crates/ty_server/src/server/api/notifications/did_change_watched_files.rs#L88-L94

This should allow you to make `publish_settings_diagnostics` per database.

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:110 on 2025-07-15 09:12_

We should skip calling `publish_settings_diagnostics` if the client uses workspace diagnostics as it is unnecessary

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:385 on 2025-07-15 09:12_

I think we need to call into `publish_settings_diagnostics` here to ensure the server pushes the diagnostics on initiale load. 

---

_@MichaReiser reviewed on 2025-07-15 09:14_

This overall looks good. 

We should remove the `publish_settings_diagnostics` calls in `did_change` and `did_open` because they're unnecessary. Instead, we should call `publish_settings_diagnostics` in `Session::initialize_workspaces`

I'd also recommend making `publish_settings_diagnostics` to take a specific `project` instead of iterating over all projects. 

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-16 06:14_

---

_Comment by @Gankra on 2025-07-17 14:39_

Review addressed, should be ready to land.

---

_@Gankra reviewed on 2025-07-17 14:40_

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:110 on 2025-07-17 14:40_

I moved the check inside the call so each callsite doesn't need to think about it.

---

_@MichaReiser approved on 2025-07-17 14:41_

Thank you

---

_Merged by @Gankra on 2025-07-17 15:57_

---

_Closed by @Gankra on 2025-07-17 15:57_

---

_Branch deleted on 2025-07-17 15:57_

---
