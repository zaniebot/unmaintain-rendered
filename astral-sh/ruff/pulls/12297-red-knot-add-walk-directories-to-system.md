```yaml
number: 12297
title: "[red-knot] Add `walk_directories` to `System`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: system-walk-directories
created_at: 2024-07-12T09:25:20Z
updated_at: 2024-07-16T06:54:36Z
url: https://github.com/astral-sh/ruff/pull/12297
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Add `walk_directories` to `System`

---

_@MichaReiser_


## Summary

This PR adds a `ignore::WalkDir` like API to `System` that allows walking a directory tree. 

We'll need this to implement the analysis of an entire directory. 

## Test Plan

Added unit tests.


---

_Label `red-knot` added by @MichaReiser on 2024-07-12 09:25_

---

_Comment by @codspeed-hq[bot] on 2024-07-12 09:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/system-walk-directories)

### Merging #12297 will **not alter performance**

<sub>Comparing <code>system-walk-directories</code> (7cfe17b) with <code>system-walk-directories</code> (b22ac5e)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:257 on 2024-07-12 17:55_

I'm happy to revert this pack to returning an `impl`. I had to change it, because a first version of the walker had to store the current iteration state in a `struct` which doesn't work with `impl` (Unless you use `dyn`). I introduced the named type and thought I'll leave it, because it can be useful. 

It's also nice that the type now implements `Debug`

---

_@MichaReiser reviewed on 2024-07-12 17:57_

---

_Marked ready for review by @MichaReiser on 2024-07-12 17:59_

---

_Review requested from @carljm by @MichaReiser on 2024-07-12 17:59_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-12 17:59_

---

_Comment by @github-actions[bot] on 2024-07-12 18:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_@AlexWaygood reviewed on 2024-07-12 20:31_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system.rs`:160 on 2024-07-12 20:31_

Heh, this is what I had originally in #12289, before https://github.com/astral-sh/ruff/pull/12289/commits/be11d8682d706e0151b8e6ac51114a4550b58422! Doesn't this just reverse the change you asked me to make in https://github.com/astral-sh/ruff/pull/12289#discussion_r1674657856? ;)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system.rs`:160 on 2024-07-12 21:00_

Yeah, that's probably true. I ran into the issue that `io::Error` isn't `Clone`. I could have worked around this problem by destructing both the `path` and the `result` from `file_type`, but I'm unsure if it's worth it. 

The main advantage of `std::fs::DirEntry::file_type` is that the operation is lazy. This is different in our implementation, where we call `file_type` when returning the `DirEntry` (so it is eager). 

Now, there's a semantic difference in the new implementation. The `read_dir` call will fail when calling `file_type` fails. I don't know in what situations it is possible to know that the entry is there, but it fails to read the file type. 

I just spent some time looking into what `walker` does. It seems `walker' also eagerly reads the file type and returns an `Err` if it fails to retrieve it. 

Ideally, I think we would make the `file_type` call lazy, but that's quiet a bit more complicated because it would require holding on to `System`. 

TLDR: Not sure, but it felt like the most straightforward path and it's unclear to me what the benefit is of keeping the Result.

---

_@MichaReiser reviewed on 2024-07-12 21:00_

---

_@AlexWaygood reviewed on 2024-07-12 21:07_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system.rs`:160 on 2024-07-12 21:07_

That all makes sense to me. If we're no longer storing a `Result`, though, I think I'd also make the following simplifications:

```diff
diff --git a/crates/ruff_db/src/system.rs b/crates/ruff_db/src/system.rs
index 2805137ec..5b1ce5b12 100644
--- a/crates/ruff_db/src/system.rs
+++ b/crates/ruff_db/src/system.rs
@@ -115,7 +115,7 @@ impl Metadata {
     }
 }
 
-#[derive(Copy, Clone, Eq, PartialEq, Debug, Hash)]
+#[derive(Copy, Clone, Eq, PartialEq, Debug, Hash, PartialOrd, Ord)]
 pub enum FileType {
     File,
     Directory,
@@ -136,7 +136,7 @@ impl FileType {
     }
 }
 
-#[derive(Debug)]
+#[derive(Debug, PartialEq, Eq, PartialOrd, Ord)]
 pub struct DirectoryEntry {
     path: SystemPathBuf,
     file_type: FileType,
@@ -155,9 +155,3 @@ impl DirectoryEntry {
         self.file_type
     }
 }
-
-impl PartialEq for DirectoryEntry {
-    fn eq(&self, other: &Self) -> bool {
-        self.path == other.path
-    }
-}
diff --git a/crates/ruff_db/src/system/os.rs b/crates/ruff_db/src/system/os.rs
index 2b7dd397d..99fff9449 100644
--- a/crates/ruff_db/src/system/os.rs
+++ b/crates/ruff_db/src/system/os.rs
@@ -283,7 +283,7 @@ mod tests {
             .unwrap()
             .map(Result::unwrap)
             .collect();
-        sorted_contents.sort_by(|a, b| a.path.cmp(&b.path));
+        sorted_contents.sort();
 
         let expected_contents = vec![
             DirectoryEntry::new(tempdir_path.join("a/bar.py"), FileType::File),
```

---

_@MichaReiser reviewed on 2024-07-12 21:08_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system.rs`:160 on 2024-07-12 21:08_

Oh yeah, we can implement `PartialEq` and `Eq`. 

I don't think I want to implement `PartialOrd` for `FileType` nor `DirectoryEntry`. Neither type has a natural ordering, in my view (mainly `FileType` but, because of that, also `DirectoryEntry`)

---

_@AlexWaygood reviewed on 2024-07-12 21:10_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system.rs`:160 on 2024-07-12 21:10_

> Not sure what the use case (or even the order) is.

I guess the use case is that you can easily derive `PartialOrd` on `DirectoryEntry`... but yeah, agreed that that one is questionable :) We can just not derive `PartialOrd` on `DirectoryEntry` and continue using `.sort_by()` rather than `.sort()` in `system/os.rs`, that's fine.

---

_@AlexWaygood reviewed on 2024-07-13 14:24_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/memory_fs.rs`:281 on 2024-07-13 14:24_

nit: I don't mind returning the named type too much, but I'm not sure I see the need to change to an imperative approach here. Does doing so fix a bug? You could do this with a much smaller diff by keeping most of the code as it was:

```suggestion
        let collected = by_path
            .range(normalized.clone()..)
            .skip(1)
            .take_while(|(path, _)| path.starts_with(&normalized))
            .filter_map(|(path, entry)| {
                if path.parent()? == normalized {
                    Some(Ok(DirectoryEntry {
                        path: SystemPathBuf::from_utf8_path_buf(path.to_owned()),
                        file_type: entry.file_type(),
                    }))
                } else {
                    None
                }
            })
            .collect()
```

---

_Review comment by @carljm on `crates/ruff_db/src/system/walk_directory.rs`:263 on 2024-07-15 16:51_

```suggestion
    /// Test helper that creates a visual representation of the visited directory entries.
```

---

_Review comment by @carljm on `crates/ruff_db/src/system/walk_directory.rs`:320 on 2024-07-15 17:19_

```suggestion
        /// Stores the visited path. The key is the relative path to the root, using `/` as path separator.
```

---

_Review comment by @carljm on `crates/ruff_db/src/system/walk_directory.rs`:59 on 2024-07-15 17:25_

It doesn't seem right to always set `ignore_hidden` to `true` here, even if someone is turning _off_ standard filters.

(And maybe add a test that would catch this?)

```suggestion
        self.ignore_hidden = standard_filters;
```

---

_Review comment by @carljm on `crates/ruff_db/src/system/walk_directory.rs`:54 on 2024-07-15 17:28_

It's not clear to me what this is supposed to mean.

Does it mean that you cannot ever set `standard_filters = true` and `ignore_hidden = false`? (If so, we would need some additional code to enforce that, because right now you can just call `.ignore_hidden(false)` and easily achieve that state.)

Or does it just mean that `standard_filters(true)` also sets `ignore_hidden = true`, but you can still override with `ignore_hidden(false)` if you want? That seems to be the current implementation, but in that case I think this comment would be clearer if it said something like "Unless explicitly overriden with `ignore_hidden(false)`, this defaults to ignoring hidden files."

---

_Review comment by @carljm on `crates/ruff_db/src/system/walk_directory.rs`:157 on 2024-07-15 17:35_

Not sure what this comment means, since `file_type` isn't `Option`

---

_Review comment by @carljm on `crates/ruff_db/src/system/walk_directory.rs`:168 on 2024-07-15 17:43_

This method appears to be unused currently (not sure why clippy isn't complaining about it), and I'm having trouble seeing where it would ever be useful, with these odd semantics. In what use case would you ever want just the name for files but the full path for directories?

---

_Review comment by @carljm on `crates/ruff_db/src/system/walk_directory.rs`:181 on 2024-07-15 17:43_

```suggestion
    /// If the entry given is a directory, don't descend into it.
```

---

_Review comment by @carljm on `crates/ruff_db/src/system/walk_directory.rs`:246 on 2024-07-15 17:46_

It seems like most places we prefer having enum variants wrap independent named structs over struct enum variants. I'm curious if you have thoughts on when to choose one vs the other.

---

_Review comment by @carljm on `crates/ruff_db/src/system/test.rs`:41 on 2024-07-15 17:57_

```suggestion
    fn use_system<S>(&mut self, system: S)
```

---

_Review comment by @carljm on `crates/ruff_db/src/system/os.rs`:144 on 2024-07-15 18:04_

This safety comment isn't clear to me, I assume because I'm missing some important context. How do we know that `entry` is a system path? What is a "stdin path"?

---

_Review comment by @carljm on `crates/ruff_db/src/system/os.rs`:152 on 2024-07-15 18:06_

How do we know that this is an error related to a git ignore file?

---

_@carljm approved on 2024-07-15 18:41_

---

_@MichaReiser reviewed on 2024-07-15 19:53_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:152 on 2024-07-15 19:53_

This is the contract from the ignore crate. 

---

_@carljm reviewed on 2024-07-15 23:35_

---

_Review comment by @carljm on `crates/ruff_db/src/system/os.rs`:152 on 2024-07-15 23:35_

I see; I guess it wouldn't hurt to clarify that in the comment.

---

_@MichaReiser reviewed on 2024-07-16 06:30_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/walk_directory.rs`:246 on 2024-07-16 06:30_

To me it depends on how the enum variants are used. Our AST used to use struct enum variants over named structs. This was very cumbersome in downstream tools because it was impossible to pass an entire `StmtFunctionDef`, because it lacked a name. That's why you still see many lint rules accepting all individual fields of their node instead of the entire node. 

Another motivation for named structs is if you want to lock-down access to individual fields because the enum fields always have the same visibility as the enum itself. 


On the other hand, using variant structs allows Rust to be more clever when it comes to layout optimizations and it's a lot less boilerplate to define the enum and to match on it (no nesting). 


I'm using struct variants here because

a) I think it's unlikely that some code would want to pass the entire variant (have it a named type) and even if, that code would at most have to accept two arguments
b) The main use case is to match on the variant and extract the fields, which is more brief with struct variants.

---

_Merged by @MichaReiser on 2024-07-16 06:40_

---

_Closed by @MichaReiser on 2024-07-16 06:40_

---

_Branch deleted on 2024-07-16 06:40_

---
