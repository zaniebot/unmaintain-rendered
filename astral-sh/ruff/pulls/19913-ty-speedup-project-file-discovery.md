```yaml
number: 19913
title: "[ty] Speedup project file discovery "
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/fix-walk-files-waterfall
created_at: 2025-08-14T11:59:26Z
updated_at: 2025-08-14T18:38:41Z
url: https://github.com/astral-sh/ruff/pull/19913
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Speedup project file discovery 

---

_@MichaReiser_

## Summary

The `ProjectFilesWalker` discovers all the files to check in a ty project by spawning multiple threads that walk the directory tree.
The walker's output is a `Vec` or `Set` of all the `File`s. The way this works today is that we first collect all file paths (multi threaded)
into a single `Vec`, and the main thread then interns the paths to their `File`'s. 

The problem with this approach is that `system_path_to_file` is fairly expensive when called for many files because it does a `stat` call internally. 

The ideal solution would move the `system_path_to_file` call to the walker threads. That would also eliminate the shared `Vec` that collects all paths. Unfortunately, this isn't possible because `&dyn Db` isn't `Send` nor does `&dyn Db` implement `Clone` (Salsa does have a `fork_db` feature but this isn't publicly exposed as of today). We also can't add a `fork` method to `ty_project::Db` that returns a cloned `Self` because the `Db` trait must be dyn compatible. 

~~The solution I've taken here is to move the stat files into the walker threads, which fixes most of the waterfall effect and is much simpler than exposing `fork_db` properly in salsa.~~

I implemented the ideal solution thanks to @ibraheemdev's suggestion to add a `dyn_clone`!

Fixes https://github.com/astral-sh/ty/issues/970


## Test Plan

Before 
<img width="689" height="960" alt="Screenshot 2025-08-14 at 14 04 46" src="https://github.com/user-attachments/assets/67908e8c-0881-4a98-9446-9d03a984706d" />


After

<img width="689" height="960" alt="Screenshot 2025-08-14 at 14 04 59" src="https://github.com/user-attachments/assets/3501d3da-912d-46b0-9ead-daa937b31f73" />



---

_Label `performance` added by @MichaReiser on 2025-08-14 11:59_

---

_Label `ty` added by @MichaReiser on 2025-08-14 11:59_

---

_Label `performance` added by @MichaReiser on 2025-08-14 11:59_

---

_Label `ty` added by @MichaReiser on 2025-08-14 11:59_

---

_Comment by @github-actions[bot] on 2025-08-14 12:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 18:13:06.319161224 +0000
+++ new-output.txt	2025-08-14 18:13:06.387161856 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(409cd)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(ac97)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-14 12:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-08-14 12:05_

---

_Review requested from @carljm by @MichaReiser on 2025-08-14 12:05_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-14 12:05_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-14 12:05_

---

_Review request for @dcreager removed by @MichaReiser on 2025-08-14 12:05_

---

_Review request for @carljm removed by @MichaReiser on 2025-08-14 12:05_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-08-14 12:05_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-08-14 12:05_

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-08-14 12:05_

---

_Renamed from "[ty] Fix waterfall when discovering project files" to "[ty] Speedup project file discovery " by @MichaReiser on 2025-08-14 12:06_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/files.rs`:107 on 2025-08-14 17:17_

How come? If `metadata` lets you cheaply query the file type, can you still return a `Result` here?

I mention this because it seems like we are otherwise careful not to allow a `File` to be backed by a directory.

---

_Review comment by @BurntSushi on `crates/ty_project/src/walk.rs`:242 on 2025-08-14 17:24_

I was wondering why you can't use `ignore::DirEntry::file_type` here, and I think it's because you also need the file permissions? Because directory entries (on Unix) will include the `FileType` free of charge. But not permissions.

---

_@BurntSushi approved on 2025-08-14 17:25_

Nice! This makes sense to me.

---

_@ibraheemdev approved on 2025-08-14 17:38_

> We also can't add a fork method to ty_project::Db that returns a cloned Self because the Db trait must be dyn compatible.

The usual workaround for this is to have a `dyn_clone` method that returns a `Box<dyn Db>`, but the solution here also seems perfectly fine.

---

_@MichaReiser reviewed on 2025-08-14 17:51_

---

_Review comment by @MichaReiser on `crates/ty_project/src/walk.rs`:242 on 2025-08-14 17:51_

Yeah, I thought about using the walk dir entry directly, but we, unfortunately, also need the permissions and, more importantly, the last modified time

---

_Comment by @MichaReiser on 2025-08-14 18:01_

> The usual workaround for this is to have a dyn_clone method that returns a Box<dyn Db>, but the solution here also seems perfectly fine.

Oh nice, that's exactly what I was looking for. 

---

_@MichaReiser reviewed on 2025-08-14 18:04_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:107 on 2025-08-14 18:04_

Yeah we could. I had a versio where I did just that. 

I considered it to be fine because `Files::try_system` also doesn't return a `Result` but the file directly (even if the path maps to a directory). This where I consider the distinction between `system_path_to_file` which makes sure it adds a query dependency on status (so that your query re-runs if the status changes) and protects you from getting a `File` that points to a directory or doesn't exist. 

Either way, I'm going to revert all of this thanks to @ibraheemdev's suggestion

---

_@ibraheemdev approved on 2025-08-14 18:38_

---

_Merged by @MichaReiser on 2025-08-14 18:38_

---

_Closed by @MichaReiser on 2025-08-14 18:38_

---

_Branch deleted on 2025-08-14 18:38_

---
