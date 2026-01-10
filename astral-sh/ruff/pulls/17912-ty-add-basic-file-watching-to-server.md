```yaml
number: 17912
title: "[ty] Add basic file watching to server"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/server-watch
created_at: 2025-05-07T12:44:14Z
updated_at: 2025-05-07T17:03:32Z
url: https://github.com/astral-sh/ruff/pull/17912
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Add basic file watching to server

---

_Pull request opened by @MichaReiser on 2025-05-07 12:44_

## Summary

This PR does the absolut minimum to add file watching support to ty's LSP. 

The main limitation is that it only watches file in the project directory. It doesn't watch for
changes to:

* The user configuration
* search paths outside the project

It also has no special handling in case the user adds or removes a `ty.toml` or `pyproject.toml` which would change the project's root directory (other than what ty's `apply_changes` provides by default). The solution
here is a tighter integration (or at least code sharing) with `ProjectWatcher`. Ideally, the CLI and LSP would share all code and only differ by the file watching backend. 

This gives us:

* Updated diagnostics when changig files outside the main editor (e.g. git pull)
* Updated inlays and diagnostics when changing the configuration 

## Test Plan

I edited a file outside of VS code and verified that diagnostics and inlay hints are correctly recomputed. 

I verified that changing the python version correctly propagates to inlays.


---

_Review requested from @carljm by @MichaReiser on 2025-05-07 12:44_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-07 12:44_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-07 12:44_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-07 12:44_

---

_Label `server` added by @MichaReiser on 2025-05-07 12:44_

---

_Label `ty` added by @MichaReiser on 2025-05-07 12:44_

---

_Comment by @github-actions[bot] on 2025-05-07 12:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-05-07 14:05_

---

_Review comment by @sharkdp on `crates/ty_server/src/server.rs`:196 on 2025-05-07 14:05_

Mostly curious: does this mean that we're reacting to changes that match the `**/ty.toml` pattern. Or that we actively scan the whole project for this pattern when starting the server (which could be quite slow?)

---

_@sharkdp reviewed on 2025-05-07 14:09_

---

_Review comment by @sharkdp on `crates/ty_server/src/server.rs`:208 on 2025-05-07 14:09_

If we want to be (more) exhaustive here, we would probably also have to add `.git/info/exclude`?

---

_@sharkdp approved on 2025-05-07 14:09_

Thank you!

---

_@MichaReiser reviewed on 2025-05-07 14:15_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:196 on 2025-05-07 14:15_

The VS code file watcher sends us notifications if any file inside the project matching `ty.toml` gets changed. 

---

_@MichaReiser reviewed on 2025-05-07 14:16_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:208 on 2025-05-07 14:16_

Maybe. It's not something that we currently handle in `apply_changes`. (but maybe should?)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-07 14:18_

---

_@dhruvmanila reviewed on 2025-05-07 15:24_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:78 on 2025-05-07 15:24_

Is this suppose to be `ty_db` ?

---

_@dhruvmanila reviewed on 2025-05-07 15:25_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:78 on 2025-05-07 15:25_

Oh, nevermind, I missed the definition. I think it would be useful to include something before `??_by_db`

---

_@dhruvmanila approved on 2025-05-07 15:26_

Looks good, thanks!

---

_@dhruvmanila reviewed on 2025-05-07 15:28_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:226 on 2025-05-07 15:28_

Wait, why do we need these here? Shouldn't they be handled by the `didChange` notifications?

---

_@MichaReiser reviewed on 2025-05-07 15:32_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:226 on 2025-05-07 15:32_

`didChange` notifications are only sent for files open in the editor. Files that change outside the editor (`nano -w my.py` or `git pull`) use the file watcher notification. I tested if VS code would send two events for files open in the editor that also match the file watcher pattern but this isn't the case. It only sends `didChange` for files open in the editor.

---

_@dhruvmanila reviewed on 2025-05-07 15:47_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:226 on 2025-05-07 15:47_

I see, thanks, that makes sense.

---

_@MichaReiser reviewed on 2025-05-07 16:42_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:208 on 2025-05-07 16:42_

I'll ignore this for now. There's another issue around better handling for git ignored files

---

_Renamed from "Add basic file watching to server" to "[ty] Add basic file watching to server" by @MichaReiser on 2025-05-07 16:45_

---

_Merged by @MichaReiser on 2025-05-07 17:03_

---

_Closed by @MichaReiser on 2025-05-07 17:03_

---

_Branch deleted on 2025-05-07 17:03_

---
