```yaml
number: 19264
title: "[ty] Track open files in the server"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/workspace-diagnostics-in-virtual-files
created_at: 2025-07-10T16:38:53Z
updated_at: 2025-07-18T14:03:37Z
url: https://github.com/astral-sh/ruff/pull/19264
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Track open files in the server

---

_Pull request opened by @dhruvmanila on 2025-07-10 16:38_

## Summary

This PR updates the server to keep track of open files both system and virtual files.

This is done by updating the project by adding the file in the open file set in `didOpen` notification and removing it in `didClose` notification. 

This does mean that for workspace diagnostics, ty will only check open files because the behavior of different diagnostic builder is to first check `is_file_open` and only add diagnostics for open files. So, this required updating the `is_file_open` model to be `should_check_file` model which validates whether the file needs to be checked based on the `CheckMode`. If the check mode is open files only then it will check whether the file is open. If it's all files then it'll return `true` by default.

Closes: astral-sh/ty#619

## Test Plan

### Before

There are two files in the project: `__init__.py` and `diagnostics.py`.

In the video, I'm demonstrating the old behavior where making changes to the (open) `diagnostics.py` file results in re-parsing the file:

https://github.com/user-attachments/assets/c2ac0ecd-9c77-42af-a924-c3744b146045

### After

Same setup as above.

In the video, I'm demonstrating the new behavior where making changes to the (open) `diagnostics.py` file doesn't result in re-parting the file:

https://github.com/user-attachments/assets/7b82fe92-f330-44c7-b527-c841c4545f8f



---

_Label `server` added by @dhruvmanila on 2025-07-10 16:38_

---

_Label `ty` added by @dhruvmanila on 2025-07-10 16:38_

---

_Comment by @github-actions[bot] on 2025-07-10 16:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-07-10 17:08_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:417 on 2025-07-10 17:08_

It probably doesn't matter in practice but a virtual file could exist that isn't in `open_files`. That's why I think it's incorrect to move this check above `open_files`.

---

_@MichaReiser reviewed on 2025-07-10 17:11_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:57 on 2025-07-10 17:11_

I think we need it to bump the file revision. Before the event: the file's revision is the timestamp on disk. After opening it, the revision is the document version.

---

_Comment by @dhruvmanila on 2025-07-11 03:28_

It seems like the issue that I'm seeing where if I open the same virtual file the second time after closing it without saving for the first time already exists on `main`. So, it doesn't seem to be related to this PR at all.

---

_Renamed from "[ty] Consider virtual files for workspace diagnostics" to "[ty] Track open files in the server" by @dhruvmanila on 2025-07-14 09:56_

---

_Comment by @github-actions[bot] on 2025-07-14 10:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-07-14 10:09_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:581 on 2025-07-14 10:09_

This does not seem useful at all because VS Code also sends us the `textDocument/diagnostic` request for open files and virtual files are always open. So, we could possibly just remove this field and all should still be fine.

---

_Marked ready for review by @dhruvmanila on 2025-07-15 05:39_

---

_Review requested from @carljm by @dhruvmanila on 2025-07-15 05:39_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-15 05:39_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-15 05:39_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-15 05:39_

---

_Label `internal` added by @dhruvmanila on 2025-07-15 05:40_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-15 07:17_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:310 on 2025-07-15 09:33_

I think we should remove this behavior and instead always check exactly what `CheckMode` configured. This also allows us to remove the `Option` from `open_files`

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:406 on 2025-07-15 09:34_

I don't think using `true` here is correct because it would also return `true` for non project files (e.g. third party files!). We still need to test if the file is in `self.files(db)`

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:581 on 2025-07-15 09:36_

Hmm, that's interesting. It would certainly simplify things a bit. Can you try if skipping over the virtual files still works as expected?

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:58 on 2025-07-15 09:38_

Let's explain why it should exist in this branch -> because it was added to the index, therefore the LSPSystem should return it when reading it

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:144 on 2025-07-15 09:40_

Having `check_mode` on `ProjectMetadata` feels a bit odd. I think I'd prefer to default to `All` (You can implement `Default` for `CheckMode` and add the `#[default]` attribute to the field) and have the LSP explicitly set the check mode after startup.

---

_@MichaReiser reviewed on 2025-07-15 09:42_

Thank you. I think the `should_check_file` implementation is currently incorrect as it also returns `true` for third party files, which we don't want. It might be worth adding a CLI test for it. 

I also think that we should remove the implicit behavior where setting `open_files` to `Some` changes the check mode. I think this is confusing now where we also have an explicit check mode.  Instead, `check` should always do whatever is configured by `CheckMode`. This may require updating the benchmarks and ty_wasm to explicitly set `CheckMode`

---

_@dhruvmanila reviewed on 2025-07-16 04:51_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:144 on 2025-07-16 04:51_

Why should it default to `AllFiles`? Currently, I've kept the default as `OpenFiles`

---

_@dhruvmanila reviewed on 2025-07-16 04:53_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:310 on 2025-07-16 04:53_

> This also allows us to remove the `Option` from `open_files`

Does this mean that the `take_open_files` would not be needed and we can mutate the open files directly? The open file set is behind an `Arc`, so might need some workaround to that.

---

_@MichaReiser reviewed on 2025-07-16 06:06_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:310 on 2025-07-16 06:06_

I think we still need the `take_open_files` internally to avoid cloning the entire hash set every time a file is opened or closed. This could be a reason for keep using an `Option<Arc<...>` internally but we should change/remove the implicit behavior that the project checks all files if `open_file_set` is none even when check mode is `OpenFiles` (which is contradicting)

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-16 06:10_

---

_@MichaReiser reviewed on 2025-07-16 06:10_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:144 on 2025-07-16 06:10_

It would match my expectation to `Project` more closely. By default, I want to check all files. It's only the editor-like environments where we may want to deviate from this behavior (LSP, WASM). This also has the advantage that it doesn't require any additional setup code in the CLI (it does in the LSP and WASM but they opt into that by setting check mode to open files)

---

_Comment by @dhruvmanila on 2025-07-16 11:59_

### Post review changes

- Update `should_check_file` to check whether the file exists in the indexed file set in `AllFiles` check mode. This will avoid any third party files to be checked in this mode
- Remove the `check_mode` field from the `ProjectMetadata`, instead expose a method `set_check_mode` from `ProjectDatabase` that updates the `Project`'s check mode
- Update the default check mode to be `AllFiles` and update the server code to set the check mode as per the diagnostic mode, update the wasm and benchmark code to always use the `OpenFiles` check mode
- Update the logic to check if a file is open or not to only check whether it's explicitly in the open file set and avoid checking the indexed file set
- Remove virtual file handling in `ProjectFiles` as it isn't required because editors send a `textDocument/diagnostic` request for open files and virtual files are always open
- Use `ChangeEvent::Created` in the `didOpen` handler for system path to notify the `Project` that there might be a new file that's added in the project so the `Indexed` file set should be updated accordingly

---

_@dhruvmanila reviewed on 2025-07-16 11:59_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:144 on 2025-07-16 11:59_

Updated the default check mode and updated wasm, benchmark and the server to explicitly set the check mode.

---

_@dhruvmanila reviewed on 2025-07-16 11:59_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:58 on 2025-07-16 11:59_

Done

---

_@dhruvmanila reviewed on 2025-07-16 12:00_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:406 on 2025-07-16 12:00_

Updated to check `self.files(db)` and `path.is_system_virtual_path`

---

_@dhruvmanila reviewed on 2025-07-16 12:00_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:310 on 2025-07-16 12:00_

I've kept the `Option<Arc<...>>` and updated the logic to use the check mode explicitly and removed the implicit behavior of `None`

---

_@dhruvmanila reviewed on 2025-07-16 12:13_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:581 on 2025-07-16 12:13_

I've removed this special handling for virtual files.

I tested out Zed which also supports virtual files but it seems that it doesn't send any notification to the client about it's existence. 


https://github.com/user-attachments/assets/14a8e76e-3ae9-4edd-a604-56f41b6087e0

Zed seems to be sending a `didClose` for the new `other.py` and then a `didOpen` which is what caused the error in the above video. I'm not sure why though. But, it does send a `FileChangeType::Created` which seems fine but then why is it sending a `didClose` event?

```
// Send:
{"jsonrpc":"2.0","method":"workspace/didChangeWatchedFiles","params":{"changes":[{"uri":"file:///Users/dhruv/playground/ty-server/src/ty_server/other.py","type":1}]}}

// Send:
{"jsonrpc":"2.0","method":"textDocument/didClose","params":{"textDocument":{"uri":"file:///Users/dhruv/playground/ty-server/src/ty_server/other.py"}}}

// Send:
{"jsonrpc":"2.0","method":"textDocument/didOpen","params":{"textDocument":{"uri":"file:///Users/dhruv/playground/ty-server/src/ty_server/other.py","languageId":"python","version":0,"text":"x"}}}
```

Neovim also has virtual file support but it has it's own issues: https://github.com/astral-sh/ruff/issues/15392

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-16 12:14_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:587 on 2025-07-17 06:26_

The `HashSet` iterator implements default. This allows us to simplify the type in `OpenFiles` to just `hash_set::Iter`.


```suggestion
                ProjectFilesIter::OpenFiles(files.map(|files| files.iter()).unwrap_or_default())
```

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:390 on 2025-07-17 06:31_

The `file_set` part of the comment no longer applies here. We also removed this optimization from `CheckMode::AllFiles`. 


I suggest moving the `path.is_vendored_path` out of the match to avoid any unnecessary dependencies

I also suggest removing the *Virtual files are always considered open* from the `OpenFiles` branch. 
I think this is confusing. We should really only consider files as open in `CheckMode::OpenFiles` that are open.


```rust
        if path.is_vendored_path() {
            // Try to return early to avoid adding a dependency on `open_files` or
            // `file_set` which both have a durability of `LOW`.
            return false
        }

        match self.check_mode(db) {
            CheckMode::OpenFiles => {
                self.open_files(db).is_some_and(|files| files.contains(&file))
            }
            CheckMode::AllFiles => {
                // Virtual files are always checked.
                path.is_system_virtual_path() || self.files(db).contains(&file)
            }
        }
```

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:59 on 2025-07-17 06:42_

I think the `None` description here is not entirely accurate. The value can be `Some` with an empty hash set. 

Instead, we should document that we use an `Option` here so that we can implement an in-place update mechanism that avoids cloning or allocating. 

Hmm, actually. I don't think this has to be an `Arc<Option<..>>` at all. This can just be an `FxHashSet<File>`. 

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:64 on 2025-07-17 06:49_

I don't think it's necessary to call `apply_changes` twice. Instead, what we should do here is to test if it is a new file and then adjust the event on line 73.

```rust
    let path = key.path();

    let is_new_file = path.as_system().is_some_and(|path| !db.system().path_exists(path));

    let document = TextDocument::new(...);
    ...

    match path {
        AnySystemPath::System(system_path) => {
            let event = if is_new_file {
                ChangeEvent::Created { path: system_path.clone(), kind: CreateKind::File }
            } else {
                ChangeEvent::Opened(system_path.clone())
            };

            session.apply_changes(vec![event]);

            match system_path_to_file(db, system_path) {
                Ok(file) => db.project().open_file(db, file),
                Err(err) => {
                    // This can only fail when the path is a directory or it doesn't exists but
                    // the file should exists for this handler in this branch because it was
                    // added to the `Index` (using `open_text_document` above) and the
                    // `LSPSystem` should return it when reading it from the index.
                    tracing::warn!("Failed to create a salsa file for {system_path}: {err}");
                }
            }
        }
    }
```

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:81 on 2025-07-17 06:52_

Can you explain me why it's no longer necessary to trigger an `Opened` event. 

I don't remember the exact details (we should maybe document why it's needed) but I suspect it is for when workspace diagnostics are disabled and you start VS code but some changes weren't saved in the last session. That's when the `Opened` event is important so that ty knows that there's now updated content in the editor.

---

_@MichaReiser reviewed on 2025-07-17 06:54_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:390 on 2025-07-17 08:43_

> The `file_set` part of the comment no longer applies here. We also removed this optimization from `CheckMode::AllFiles`.

Right, thanks for pointing that out. But, once I apply your patch, the comment would still apply because of the `self.files` call that calls into `self.file_set`.

---

_@dhruvmanila reviewed on 2025-07-17 08:43_

---

_@dhruvmanila reviewed on 2025-07-17 08:48_

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:59 on 2025-07-17 08:48_

> Hmm, actually. I don't think this has to be an `Arc<Option<..>>` at all. This can just be an `FxHashSet<File>`.

Yeah, actually, both `Option` and `Arc` isn't required. But, just to confirm, that would mean that we'd need to update the `take_open_files` to reset it to an empty set, right? Or, did you have something else in mind?

```rs
    /// Sets the open files in the project.
    #[tracing::instrument(level = "debug", skip(self, db))]
    pub fn set_open_files(self, db: &mut dyn Db, open_files: FxHashSet<File>) {
        tracing::debug!("Set open project files (count: {})", open_files.len());

        self.set_open_fileset(db).to(open_files);
    }

    /// This takes the open files from the project and returns them.
    fn take_open_files(self, db: &mut dyn Db) -> FxHashSet<File> {
        tracing::debug!("Take open project files");

        // Salsa will cancel any pending queries and remove its own reference to `open_files`
        // so that the reference counter to `open_files` now drops to 1.
        self.set_open_fileset(db).to(FxHashSet::default())
    }
```

---

_@MichaReiser reviewed on 2025-07-17 08:58_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:59 on 2025-07-17 08:58_

> update the take_open_files to reset it to an empty set, right? Or, did you have something else in mind?

Yes, that's correct

---

_@dhruvmanila reviewed on 2025-07-17 09:00_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:81 on 2025-07-17 09:00_

> you start VS code but some changes weren't saved in the last session.

Why does this matter? When a user starts VS Code, it'll be starting a new server instance. Or, are you looking at it from a caching perspective?

---

_@MichaReiser reviewed on 2025-07-17 09:57_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:81 on 2025-07-17 09:57_

I would have to play with it again to see where/when it is needed but you added it ;) 

https://github.com/astral-sh/ruff/pull/13041/commits/0a37fadb4c50e0c1a4bf59d1429087ce0a197b05

We should probably add the same handling to `didOpenNotebook`



---

_@MichaReiser reviewed on 2025-07-17 10:05_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:81 on 2025-07-17 10:05_

I haven't been able to reproduce a case where it is needed. It would have to be a case where the initial content is different from the content on disk (and VS code doesn't send a change notification). In that case, it's important that the `File::revision` gets updated to the VS code revision.

If you decide that it isn't required: Then delete the event because it should now be unused. 


---

_@dhruvmanila reviewed on 2025-07-17 10:06_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:81 on 2025-07-17 10:06_

Sounds good, thanks for trying it out!

---

_@dhruvmanila reviewed on 2025-07-17 14:57_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:64 on 2025-07-17 14:57_

Wait, isn't it actually important that we call `apply_changes` before the document is added to the index so that the `Project` knows that new file has been added?

---

_@dhruvmanila reviewed on 2025-07-17 15:01_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:81 on 2025-07-17 15:01_

> I haven't been able to reproduce a case where it is needed. It would have to be a case where the initial content is different from the content on disk (and VS code doesn't send a change notification). In that case, it's important that the `File::revision` gets updated to the VS code revision.

Even in this case, I think the client would send us the `didOpen` request with the file content that's in the editor and not the one on disk. And, the fact that we need to add this file to the open file set, we're using `system_path_to_file` which then creates and interns the `File` with the revision from the index.

So, I think `ChangeEvent::Opened` isn't required.

---

_@MichaReiser reviewed on 2025-07-17 16:25_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:81 on 2025-07-17 16:25_

> Even in this case, I think the client would send us the didOpen request with the file content that's in the editor and not the one on disk. And, the fact that we need to add this file to the open file set, we're using system_path_to_file which then creates and interns the File with the revision from the index.

I don't think this reasoning is sound. The file might already been interned before. E.g. because workspace diagnostics are on and the file was checked. Opening the file with different content then on disk then absolutely **has to** bump the revision to ensure ty re-reads the source 

I think the "safest" is to just trigger the event. It might invalidate some internal state unnecessarily. Or we try and remove it and wait for a user report telling us what breaks :) 



---

_@dhruvmanila reviewed on 2025-07-18 04:01_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:81 on 2025-07-18 04:01_

> I think the "safest" is to just trigger the event. It might invalidate some internal state unnecessarily. Or we try and remove it and wait for a user report telling us what breaks :)

Yeah, make sense. Although what you've suggested in an earlier comment doesn't work -- the part where the `apply_changes` is being called after the document has been added in the index. It doesn't work in the case when a virtual file is being saved as a system file.

I've pushed a change where it splits the `apply_changes` call as per "is this a new system file?" boolean where if it's a new file, we `apply_changes` with `ChangeEvent::Created` before the document is added to the index so that the `Project` is notified of this change and it updates it accordingly and `apply_changes` with `ChangeEvent::Opened` after the document has been added to the index.

---

_@MichaReiser reviewed on 2025-07-18 05:47_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:50 on 2025-07-18 05:47_

The check here is incorrect. `path_exists` will return `true` for existing and new files. 

Thinking this through more. I suspect that doing the check before `open_text_document` doesn't work in the virtual -> regular file case because the file does exist on disk at the moment `didOpen` is called. 

So I think we need to move the check after `open_text_document` and it needs to combine the state of the db with the state of `system`:

```rust
	match system_path_to_file(db, system_path) {
		Ok(file) => {
			session.apply_changes(path, vec![ChangeEvent::Opened(system_path.clone())]);
		}
		Err(FileError::NotFound) => {
			if db.system().path_exists(system_path) {
				// trigger create event
			}
			system_path_to_file(db, system_path).unwrap()
		}
```


or, after opening the document

```rust
match path {
	AnySystemPath::System(system_path) => {
		if db.files().system(db, path).status() == FileStatus::NotFound && db.system().path_exists(path) {
	db.apply_changes(vec![ChangeEvent::Created { ... });
} else {
		db.apply_changes(vec![ChangeEvent::Opened();
	}

	let file = match system_path_to_file {...}

	}
```



---

_@dhruvmanila reviewed on 2025-07-18 05:51_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_open.rs`:50 on 2025-07-18 05:51_

> Thinking this through more. I suspect that doing the check before `open_text_document` doesn't work in the virtual -> regular file case because the file does exist on disk at the moment `didOpen` is called.

It does work:


https://github.com/user-attachments/assets/5c219c7b-6333-456a-adab-cc0e5aa05d67



---

_@MichaReiser reviewed on 2025-07-18 06:13_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:50 on 2025-07-18 06:13_

> It does work:

Sorry, what I meant by this is that the check now is overly aggressive. We now always re-index the project because every change is a "new file". And what doesn't work is using `path_exists` on its own to determine if the file exists (because it will when changing from virtual -> real file)

---

_Comment by @dhruvmanila on 2025-07-18 09:56_

### Test cases

* Open a virtual file, save it as system file
* Delete the system file while it's open
* Delete the system file while it's open but the editor focus is in a different file
* Add an import that references a non-existant file, create a virtual file and save it with the same name as the import references

https://github.com/user-attachments/assets/8bf966e0-33b8-45a8-978b-33063f409e53





---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-18 10:03_

---

_@MichaReiser approved on 2025-07-18 11:10_

Uff, that was tricky. Thank you for seeing this through! 

---

_Merged by @dhruvmanila on 2025-07-18 14:03_

---

_Closed by @dhruvmanila on 2025-07-18 14:03_

---

_Branch deleted on 2025-07-18 14:03_

---
