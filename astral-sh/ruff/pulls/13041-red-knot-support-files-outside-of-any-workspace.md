```yaml
number: 13041
title: "[red-knot] Support files outside of any workspace"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/non-workspace-files
created_at: 2024-08-22T06:56:57Z
updated_at: 2024-08-23T07:00:54Z
url: https://github.com/astral-sh/ruff/pull/13041
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Support files outside of any workspace

---

_Pull request opened by @dhruvmanila on 2024-08-22 06:56_

## Summary

This PR adds basic support for files outside of any workspace in the red knot server.

This also limits the red knot server to only work in a single workspace. The server will not start if there are multiple workspaces.

## Test Plan

https://github.com/user-attachments/assets/de601387-0ad5-433c-9d2c-7b6ae5137654




---

_Label `red-knot` added by @dhruvmanila on 2024-08-22 06:56_

---

_@dhruvmanila reviewed on 2024-08-22 09:44_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 09:44_

I want this entire logic in the `Session` but the borrow checker always complains when I do something like:
```rs
        self.workspaces
            .range_mut(..=path.as_ref().to_path_buf())
            .next_back()
            .map(|(_, db)| db)
			.unwrap_or_else(|| self.default_workspace_db_mut())
```
There are two mutable references:
```
error[E0500]: closure requires unique access to `*self` but it is already borrowed
   --> crates/red_knot_server/src/session.rs:104:29
    |
97  |           &mut self,
    |           - let's call the lifetime of this reference `'1`
...
100 |           self.workspaces
    |           ---------------
    |           |
    |  _________borrow occurs here
    | |
101 | |             .range_mut(..=path.as_ref().to_path_buf())
102 | |             .next_back()
103 | |             .map(|(_, db)| db)
104 | |             .unwrap_or_else(|| self.default_workspace_db_mut())
    | |_____________________________^^_----___________________________- returning this value requires that `self.workspaces` is borrowed for `'1`
    |                               |  |
    |                               |  second borrow occurs due to use of `*self` in closure
    |                               closure construction occurs here

For more information about this error, try `rustc --explain E0500`.
```

---

_@dhruvmanila reviewed on 2024-08-22 09:45_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/notifications/did_close.rs`:40 on 2024-08-22 09:45_

I don't think it's required for the server to sync the file when it's closed. The concern here was that when an unsaved file content is either accepted or discarded, it might require us to sync it but the client will send us the `didChange` request before the `didClose`. So, at this point the file content is already in sync.

---

_@dhruvmanila reviewed on 2024-08-22 09:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/requests/diagnostic.rs`:71 on 2024-08-22 09:47_

This is just to make sure the diagnostics are highlighted for at most one character. Without this, it would spill over to the next line.

---

_Comment by @github-actions[bot] on 2024-08-22 09:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-22 11:24_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_close.rs`:40 on 2024-08-22 11:43_

I think calling `sync` is necessary when the file is a virtual file to mark it as "deleted". 

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/notifications/did_open.rs`:37 on 2024-08-22 11:44_

I'm not sure if `file_created` is the correct event here. The analyzer might already have seen the file when traversing the file system. I think it is only virtual files which would be "created".

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api.rs`:87 on 2024-08-22 11:47_

Nit: We may want to consider using system path in the LSP as well to remove the back and forth between different types

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 11:48_

This seems to work

```rust
    pub(crate) fn workspace_db_for_path_or_default(&self, path: impl AsRef<Path>) -> &RootDatabase {
        self.workspace_db_for_path(path)
            .unwrap_or_else(|| self.default_workspace_db())
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 11:51_

This also compiles fine for me... 

```rust
    pub(crate) fn workspace_db_for_path(&self, path: impl AsRef<Path>) -> &RootDatabase {
        self.workspaces
            .range(..=path.as_ref().to_path_buf())
            .next_back()
            .map(|(_, db)| db)
            .unwrap_or_else(|| self.default_workspace_db())
    }
```

Not sure if I'm doing something wrong. But I think you face the problem with `workspace_db_for_path_mut`?

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 12:00_

But yeah, not sure why the `mut` case doesn't work. Sounds like a borrow checker limitation? Maybe @BurntSushi has an idea

I tried to hint rust as best as I can but it just won't believe that we only return one mutable borrow 

```rust
        let workspace = self
            .workspaces
            .range_mut(..=path.as_ref().to_path_buf())
            .next_back();

        if let Some((_, db)) = workspace {
            return db;
        }
        drop(workspace);

        return self.workspaces.values_mut().next().unwrap();
```

---

_@MichaReiser approved on 2024-08-22 12:00_

---

_@dhruvmanila reviewed on 2024-08-22 12:06_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 12:06_

> Not sure if I'm doing something wrong. But I think you face the problem with `workspace_db_for_path_mut`?

Yes, it's for the mutable version.

> I tried to with as many hints as possible but it just won't believe that we only return one mutable borrow

Yeah, I tried a couple of them myself, all raised the same error

---

_@dhruvmanila reviewed on 2024-08-22 12:09_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/notifications/did_close.rs`:40 on 2024-08-22 12:09_

Shouldn't that need to happen explicitly by updating the file status? This means it requires a dedicated `ChangedEvent` as done in https://github.com/astral-sh/ruff/pull/13044.

---

_@BurntSushi reviewed on 2024-08-22 15:22_

---

_Review comment by @BurntSushi on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 15:22_

Nothing is immediately popping out at me here. This might be something that is fixed by Polonius. Specifically, this might be the problem you're hitting? https://blog.rust-lang.org/inside-rust/2023/10/06/polonius-update.html#background-on-polonius

---

_@MichaReiser reviewed on 2024-08-22 15:28_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-22 15:28_

Thanks. Yes, that might be it. 

An alternative solution could be to accept a closure that applies the mutation to the `db`. Not as nice but  hopefully that works?

Otherwise transmute your way to happiness :joy: 

---

_@dhruvmanila reviewed on 2024-08-23 06:36_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api.rs`:90 on 2024-08-23 06:36_

> An alternative solution could be to accept a closure that applies the mutation to the `db`. Not as nice but hopefully that works?

Huh, this works but it gets a little bit complicated because we need to move the `path` to the `ChangedEvent` (and also that the path could be system or virtual). I'll keep the existing logic for now with a TODO to link to this discussion.


---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/notifications/did_open.rs`:37 on 2024-08-23 06:40_

Yeah, I think that's correct. I think we might not need to emit any event here.

---

_@dhruvmanila reviewed on 2024-08-23 06:40_

---

_@dhruvmanila reviewed on 2024-08-23 06:43_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/notifications/did_open.rs`:37 on 2024-08-23 06:43_

Or maybe it's required when publishing diagnostics for clients that doesn't support pull diagnostics. I'm thinking of adding a new event kind like `Opened(SystemPathBuf)`.

---

_@dhruvmanila reviewed on 2024-08-23 06:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api.rs`:87 on 2024-08-23 06:47_

Yeah, I think that should be done after adding virtual file support because then there will be two paths.

---

_Merged by @dhruvmanila on 2024-08-23 06:51_

---

_Closed by @dhruvmanila on 2024-08-23 06:51_

---

_Branch deleted on 2024-08-23 06:51_

---
