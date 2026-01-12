```yaml
number: 19408
title: "[ty] Add caching for submodule completion suggestions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/submodule-caching
created_at: 2025-07-17T18:07:26Z
updated_at: 2025-07-18T15:54:28Z
url: https://github.com/astral-sh/ruff/pull/19408
synced_at: 2026-01-12T15:56:39Z
```

# [ty] Add caching for submodule completion suggestions

---

_@BurntSushi_

This change makes it so we aren't doing a directory traversal every time
we ask for completions from a module. Specifically, submodules that
aren't attributes of their parent module can only be discovered by
looking at the directory tree. But we want to avoid doing a directory
scan unless we think there are changes.

To make this work, this change does a little bit of surgery to
`FileRoot`. Previously, a `FileRoot` was only used for library search
paths. Its revision was bumped whenever a file in that tree was added,
deleted or even modified (to support the discovery of `pth` files and
changes to its contents). This generally seems fine since these are
presumably dependency paths that shouldn't change frequently.

In this change, we add a `FileRoot` for the project. But having the
`FileRoot`'s revision bumped for every change in the project makes
caching based on that `FileRoot` rather ineffective. That is, cache
invalidation will occur too aggressively. To the point that there is
little point in adding caching in the first place. To mitigate this, a
`FileRoot`'s revision is only bumped on a change to a child file's
contents when the `FileRoot` is a `LibrarySearchPath`. Otherwise, we
only bump the revision when a file is created or added.

The effect is that, at least in VS Code, when a new module is added or
removed, this change is picked up and the cache is properly invalidated.
Other LSP clients with worse support for file watching (which seems to
be the case for the CoC vim plugin that I use) don't work as well. Here,
the cache is less likely to be invalidated which might cause completions
to have stale results. Unless there's an obvious way to fix or improve
this, I propose punting on improvements here for now.


---

_Review requested from @carljm by @BurntSushi on 2025-07-17 18:07_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-07-17 18:07_

---

_Review requested from @sharkdp by @BurntSushi on 2025-07-17 18:07_

---

_Review requested from @dcreager by @BurntSushi on 2025-07-17 18:07_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-07-17 18:07_

---

_Review request for @dcreager removed by @BurntSushi on 2025-07-17 18:07_

---

_Review request for @carljm removed by @BurntSushi on 2025-07-17 18:07_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-07-17 18:07_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-07-17 18:07_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-07-17 18:07_

---

_Label `internal` added by @BurntSushi on 2025-07-17 18:07_

---

_Label `server` added by @BurntSushi on 2025-07-17 18:07_

---

_Label `ty` added by @BurntSushi on 2025-07-17 18:07_

---

_Comment by @BurntSushi on 2025-07-17 18:08_

Demo (although I guess this is technically indistinguishable from what's on `main`):

https://github.com/user-attachments/assets/36317313-eb5a-4ed0-aa4e-53c82cadb3ae

---

_Comment by @github-actions[bot] on 2025-07-17 18:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @BurntSushi on 2025-07-17 18:51_

The test failure on Windows looks interesting. Maybe the file watcher isn't picking up the deleted file? If it doesn't, then the cache doesn't get invalidated and you end up with `wazoo` in the completions (which should be gone).

---

_Review comment by @MichaReiser on `crates/ty/tests/file_watching.rs`:1903 on 2025-07-18 11:32_

I assume we're waiting here on the event for `bar/wazoo.py`. If so, I suggest using `event_for_file("bar/wazoo.py")`. That makes the test less flaky because the runner explicitly waits for an event for this file (or `Rescan`) rather than any event
```suggestion
    let changes = case.stop_watch(event_for_file("wazoo.py"));
```

---

_Review comment by @MichaReiser on `crates/ty/tests/file_watching.rs`:1947 on 2025-07-18 11:33_

Same here, use `event_file` if possible

---

_Review comment by @MichaReiser on `crates/ty/tests/file_watching.rs`:1981 on 2025-07-18 11:37_

I'm not sure this test is testing what you want. A file watcher could decide not to emit any event here because the operations happen to quickly (and the observable effect is nothing changed).

I think what you want to do here is to write the file, wait for the file watching event, update the db, then delete the file...

```suggestion
    std::fs::write(case.project_path("bar/wazoo.py").as_std_path(), "")?;
    let changes = case.take_watch_changes(event_for_file("wazoo.py"));
    case.apply_changes(changes, None);
    
    std::fs::remove_file(case.project_path("bar/wazoo.py").as_std_path())?;
    let changes = case.stop_watch(event_for_file("wazoo.py"));
```

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:138 on 2025-07-18 11:39_

Isn't `sync_recursively` already updating all roots?

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:175 on 2025-07-18 11:43_

On line 284: We'll need to re-create the `FileRoot`. The case handled there is if a user deletes a `pyproject.toml` (or creates a new `ty.toml`). Doing so can change the root path of the project. You might want to move the `FileRoot` creation into the `from_metadata` method

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/module.rs`:20 on 2025-07-18 11:54_

It's probably best to do this in a separate PR but I think it could make sense to explore making `Module` a `salsa::tracked` and removing the inner `Arc`. 

The decision for making something a salsa ingredient vs e.g. using an `Arc` is mostly on whether there are any salsa queries that take the struct as an argument. `Module` is an `Arc` because, up to now, no query took `Module` as an argument so it was simply unnecessary for it to be a sala tracked struct. This changes with your PR. That's why I think it's worth to give it a short try to see how big the fallout is if we make it a `salsa::struct`. 

I would probably design it like this:

```rust
#[salsa::tracked(debug)]
#[derive(PartialEq, Eq)]
pub(crate) struct FileModule {
	#[returns(ref)]
	name: ModuleName, 
	kind: ModuleKind,
	#[returns(ref)]
  search_path: SearchPath,
  file: File,
  known: Option<KnownModule>,
}

#[salsa::tracked(debug)]
#[derive(PartialEq, Eq)]
pub(crate) struct NamespacePackage {
	#[returns(ref)]
	name: ModuleName
}

#[derive(salsa::Supertype, Eq, PartialEq, Debug)]
pub(crate) enum Module {
	File(FileModule),
	Namespace(NamespacePackage)
}
```

---

_@MichaReiser approved on 2025-07-18 11:56_

This looks great. I left a few comments around the file watching test. I hope it helps resolve the windows issue. 

The only thing that needs addressing before landing is that we properly handle the case where a user creates or deletes a new `ty.toml` or `pyproject.toml` (see inline comment). 

I also think it's worth considering making `Module` a salsa tracked ingredient to remove the `()` pseudo argument hack but that's something for a separate PR and only worth doing if the fallout isn't too big

---

_@BurntSushi reviewed on 2025-07-18 14:01_

---

_Review comment by @BurntSushi on `crates/ty/tests/file_watching.rs`:1903 on 2025-07-18 14:01_

Ah okay, I didn't realize that providing a specific file path would help avoid flakes. That makes sense. I noticed the other tests did that, but wondered about it. I added some comments on `stop_watch` explaining this.

---

_@BurntSushi reviewed on 2025-07-18 14:04_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/module.rs`:20 on 2025-07-18 14:04_

Thank you! I'll give this a try in a follow-up.

---

_@MichaReiser reviewed on 2025-07-18 14:04_

---

_Review comment by @MichaReiser on `crates/ty/tests/file_watching.rs`:1903 on 2025-07-18 14:04_

Thank you. I guess there are two benefits:

* It prevents "stopping" if there was a spurious file watcher event
* It gives better error messages because we can now say: We waited for an event for `wazoo.py` but we only saw ...


---

_@BurntSushi reviewed on 2025-07-18 14:06_

---

_Review comment by @BurntSushi on `crates/ty/tests/file_watching.rs`:1981 on 2025-07-18 14:06_

Perfect, thank you!

---

_@BurntSushi reviewed on 2025-07-18 14:19_

---

_Review comment by @BurntSushi on `crates/ty_project/src/db/changes.rs`:138 on 2025-07-18 14:19_

It looks like it doesn't. It only seems to sync roots that are beneath the path given. Which is maybe not intended? Specifically, if this is flipped around

https://github.com/astral-sh/ruff/blob/ba7ed3a6f99aa37c82b888c0ecea3ebf1ba0ec03/crates/ruff_db/src/files.rs#L235

then I think it works how you describe. But it seems like the intent of `sync_recursively` is only to sync files/directories _beneath_ the path given. But here, we want to sync the root for the path given, which should be _above_ it.

---

_@MichaReiser reviewed on 2025-07-18 14:22_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:138 on 2025-07-18 14:22_

Uhm, this looks the wrong way round... It doesn't seem very useful to bump a root if its parent directory changed? The idea of `sync_recursively` is to resync the entire subtree. 

---

_@BurntSushi reviewed on 2025-07-18 14:29_

---

_Review comment by @BurntSushi on `crates/ty_project/src/db/changes.rs`:138 on 2025-07-18 14:29_

Ah okay. So for file roots you want to go up, but for the actual files you want to go down. So I think this part is correct:

https://github.com/astral-sh/ruff/blob/ba7ed3a6f99aa37c82b888c0ecea3ebf1ba0ec03/crates/ruff_db/src/files.rs#L226-L230

I can flip this condition around.

---

_@BurntSushi reviewed on 2025-07-18 14:41_

---

_Review comment by @BurntSushi on `crates/ty_project/src/db/changes.rs`:175 on 2025-07-18 14:41_

Good catch. I actually had this at an even higher level, but I agree that putting it in `from_metadata` is even better.

Adding a regression test for this seems a little tricky. I think it's because the project is created initially and a file root is added. And even when the `Project` is re-created, I guess by that point the file root has already been added so everything still works?

Anyway, this is done. And I did add a test.

---

_Merged by @BurntSushi on 2025-07-18 15:54_

---

_Closed by @BurntSushi on 2025-07-18 15:54_

---

_Branch deleted on 2025-07-18 15:54_

---
