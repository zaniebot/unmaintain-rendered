```yaml
number: 12492
title: "[red-knot] Add support for untitled files"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/untitled-files
created_at: 2024-07-24T14:35:22Z
updated_at: 2024-08-02T15:49:29Z
url: https://github.com/astral-sh/ruff/pull/12492
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Add support for untitled files

---

_@dhruvmanila_

## Summary

This PR adds support for untitled files in the Red Knot project.

Refer to the [design discussion](https://github.com/astral-sh/ruff/discussions/12336) for more details.

### Changes
* The `parsed_module` always assumes that the `SystemVirtual` path is of `PySourceType::Python`.
* For the module resolver, as suggested, I went ahead by adding a new `SystemOrVendoredPath` enum and renamed `FilePathRef` to `SystemOrVendoredPathRef` (happy to consider better names here).
* The `file_to_module` query would return if it's a `FilePath::SystemVirtual` variant because a virtual file doesn't belong to any module.
* The sync implementation for the system virtual path is basically the same as that of system path except that it uses the `virtual_path_metadata`. The reason for this is that the system (language server) would provide the metadata on whether it still exists or not and if it exists, the corresponding metadata.

For point (1), VS Code would use `Untitled-1` for Python files and `Untitled-1.ipynb` for Jupyter Notebooks. We could use this distinction to determine whether the source type is `Python` or `Ipynb`.

## Test Plan

Added test cases in #12526 


---

_Label `red-knot` added by @dhruvmanila on 2024-07-24 14:35_

---

_Comment by @github-actions[bot] on 2024-07-24 14:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/path.rs`:627 on 2024-07-25 09:09_

I don't think we can safely offer this method. For example, calling it for `untitled://name` would fail.

---

_@MichaReiser reviewed on 2024-07-25 09:09_

---

_@dhruvmanila reviewed on 2024-07-25 09:20_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/system/path.rs`:627 on 2024-07-25 09:20_

Is it because the file doesn't really exists and that the methods on `std::path::Path` would error?

---

_Marked ready for review by @dhruvmanila on 2024-07-25 10:52_

---

_Review requested from @carljm by @dhruvmanila on 2024-07-25 10:52_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-07-25 10:52_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-25 10:53_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/path.rs`:627 on 2024-07-25 12:29_

I don't think it would error. It's more that operations like `components` may not work as intended. 

---

_@MichaReiser reviewed on 2024-07-25 12:29_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:36 on 2024-07-25 13:10_

I think adding an extension method for virtual path should be possible if we document that a virtual path must:

* Use the file name as the last component
* Is only allowed to use `/` as separator. 



---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/test.rs`:78 on 2024-07-25 13:13_

I think I would add a hash map storing the virtual files to system instead of adding it to `MemoryFileSystem` because `MemoryFileSystem` isn't a full `System`, it's just a building block for `System` implementations that don't want to use a real fs.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:35 on 2024-07-25 13:17_

I would remove this method for now and instead add a `Files::add_virtual_file(path)` method (we'll want a `remove_virtual_file` long term). The reason why I think we don't need a `system_virtual_path_to_file` function is because we should never really have the need to go from path to file. Instead, the code that creates the virtual file should hold on to it (the LSP should store the `File` directly). 

---

_@MichaReiser approved on 2024-07-25 13:18_

Looks good to me. It would be great if we could add a test in parsed to demonstrate that it is working. 

@AlexWaygood is in the middle of refactoring `path` and I think your new struct will no longer be needed in the new version. But I'll let him have a look at the changes. 

---

_Comment by @AlexWaygood on 2024-07-25 18:29_

@dhruvmanila -- #12494 is ready to land now. I'm going to land it and then push to your branch to fixup the merge conflicts for you, since it feels unfair to do such a huge refactor and then expect you to have to fix the conflicts :-)

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/parsed.rs`:36 on 2024-07-26 07:52_

Do you think it's ok to use the `std::path::Path::extension` method to do this or should we add our own implementation?

---

_@dhruvmanila reviewed on 2024-07-26 07:52_

---

_@MichaReiser reviewed on 2024-07-26 08:31_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:36 on 2024-07-26 08:31_

Not sure. It would need some tests to show that it's Okay. The alternative is to split of the part after the last `/` and then search for the last `.` 

---

_@dhruvmanila reviewed on 2024-07-26 08:34_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/system/test.rs`:78 on 2024-07-26 08:34_

I don't think I follow this. Would this be storing a hashmap (key is virtual file, value is system) or just storing a hashmap from virtual file path to virtual file in the `TestSystem`?

---

_@MichaReiser reviewed on 2024-07-26 09:35_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/test.rs`:78 on 2024-07-26 09:35_

Thinking about this more, let's just add it to `MemoryFileSystem` that internally stores a ` FxHashMap<SystemVirtualPathBuf, File>` (or `DashMap`)

---

_Merged by @dhruvmanila on 2024-07-26 12:43_

---

_Closed by @dhruvmanila on 2024-07-26 12:43_

---

_Branch deleted on 2024-07-26 12:43_

---
