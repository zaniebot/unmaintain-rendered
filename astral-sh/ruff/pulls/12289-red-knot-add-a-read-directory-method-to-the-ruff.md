```yaml
number: 12289
title: "[red-knot] Add a `read_directory()` method to the `ruff_db::system::System` trait"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: readdir
created_at: 2024-07-11T16:22:23Z
updated_at: 2024-07-12T20:45:54Z
url: https://github.com/astral-sh/ruff/pull/12289
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Add a `read_directory()` method to the `ruff_db::system::System` trait

---

_@AlexWaygood_

## Summary

Editable installations in Python are (usually[^1]) implemented via static `.pth` files that are inserted into `site-packages` by the build backend. If the sole contents of a `.pth` file in the `site-packages` directory are an absolute path to a directory on disk, Python will consider that path to be an additional module-resolution search path that will be appended to `sys.path` on startup.

To support editable installations in the red-knot module resolver, we'll need to similarly search through the top level of `site-packages` searching for `.pth` files. In order to achieve this, this PR adds a `read_dir()` method to the `ruff_db::system::System` trait.

A rough indication of the functionality we'll need for editable support can be seen in the following function, which is part of the port @charliermarsh did of pyright's module resolver:

https://github.com/astral-sh/ruff/blob/d0298dc26d471666acc01dacdb603e3e95aca06f/crates/ruff_python_resolver/src/search.rs#L97-L134

## Test Plan

`cargo test -p ruff_db`

[^1]: Newer versions of setuptools use a more dynamic approach for editable installations, where the `.pth` file contains Python code which, when executed, dynamically computes the path which should be appended to `sys.path`. Setuptools is alone in using this approach for editable installations, and no other type checkers support editable installations produced via this method. Setuptools also provides a way of switching to the old approach for editable installations, where the `.pth` file simply contains a static path. As such, I do not intend to attempt to support editable installations created using this approach.

---

_Label `red-knot` added by @AlexWaygood on 2024-07-11 16:22_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-11 16:22_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-11 16:22_

---

_Comment by @github-actions[bot] on 2024-07-11 16:43_

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

_@carljm approved on 2024-07-11 20:21_

Looks reasonable to me, but I'd rather have Micha look at this.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system.rs`:57 on 2024-07-11 20:26_

Can we rename the method to read_directory. We use directory everywhere else

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-07-11 20:28_

The implementation isn't just for testing. It's also used for wasm

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:252 on 2024-07-11 20:29_

Why does this copy all keys and not just collect the relevant paths?

If we already collect, then let's just return a vec iterator 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:98 on 2024-07-11 20:31_

Is there no into_path method? We should avoid allocating a new path here

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:99 on 2024-07-11 20:34_

Interesting, why can file_type fail. Should our DirEntry also implement file_type lazily (by storing a Result<FileType>?)

---

_@MichaReiser reviewed on 2024-07-11 20:34_

---

_Comment by @Hnasar on 2024-07-11 21:03_

> If the sole contents of a .pth file in the site-packages directory are an absolute path to a directory on disk

Just noting for the future that pth files may contain multiple paths, and the paths need not be absolute.

https://docs.python.org/3/library/site.html

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-07-11 21:40_

Ah, I didn't realise. That changes things :/

---

_@AlexWaygood reviewed on 2024-07-11 21:40_

---

_@AlexWaygood reviewed on 2024-07-11 21:44_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/memory_fs.rs`:252 on 2024-07-11 21:44_

I tried for quite a while to figure out a way of doing this without collecting, but couldn't get it past the borrow checker. In the end I gave up and figured it didn't matter much, since I believed this implementation was solely for testing. But you pointed out elsewhere that that's incorrect :( so I'll revisit this.

---

_@MichaReiser reviewed on 2024-07-12 08:26_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:252 on 2024-07-12 08:26_

I don't mind collecting to a `Vec` because returning an `Iterator` that holds on to the lock can cause a dead lock if the iterating code calls another memory file system method. 

I think you can do something like this:

```rust
		let mut entries = Vec::new();

        for (entry_path, entry) in by_path.range(normalized.clone()..).skip(1) {
            if !entry_path.starts_with(&normalized) {
                break;
            }

            if entry_path.parent() == Some(&normalized) {
                let file_type = match entry {
                    Entry::File(_) => FileType::File,
                    Entry::Directory(_) => FileType::Directory,
                };

                entries.push(Ok(DirectoryEntry {
                    path: SystemPathBuf::from_utf8_path_buf(entry_path.to_path_buf()),
                    file_type,
                }));
            }
        }

        Ok(ReadDirectory {
            entries: entries.into_iter(),
        })
```

Which only collects the relevant paths instead of all of them.

---

_@MichaReiser reviewed on 2024-07-12 08:27_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-07-12 08:27_

I think you want to use `metadata` here to raise the correct error when the file doesn't exist:

```rust
        let metadata = self.metadata(path)?;
        if !metadata.file_type().is_directory() {
            return Err(not_a_directory());
        }
```

---

_@AlexWaygood reviewed on 2024-07-12 10:49_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system.rs`:57 on 2024-07-12 10:49_

I called it `read_dir()` for consistency with `std::fs::read_dir()`, `camino::Utf8Path::read_dir()` and `camino::Utf8Path::read_dir_utf8()`. But I guess our equivalent of e.g. `is_dir()` in those other libraries is `is_directory()` (and I prefer fully spelling it out as well). So I'll make the change!

---

_Renamed from "[red-knot] Add a `read_dir()` method to the `ruff_db::system::System` trait" to "[red-knot] Add a `read_directory()` method to the `ruff_db::system::System` trait" by @AlexWaygood on 2024-07-12 10:51_

---

_@AlexWaygood reviewed on 2024-07-12 11:00_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/os.rs`:99 on 2024-07-12 11:00_

> Interesting, why can file_type fail.

Unclear. The `camino::Utf8DirEntry::file_type` method just delegates to `std::fs::DirEntry::file_type`. Neither method's documentation gives any information about why it might return an error.

> Should our DirEntry also implement file_type lazily (by storing a Result?)

Sure, I can do that.

---

_@AlexWaygood reviewed on 2024-07-12 11:44_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/memory_fs.rs`:249 on 2024-07-12 11:44_

Isn't that already taken care of a couple of lines above, where I do

```rs
let entry = by_path.get(&normalized).ok_or_else(not_found)?;
```

?

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-12 11:44_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:241 on 2024-07-12 11:53_

Let's make this `pub`. I think it's useful outside this crate as well.

---

_@MichaReiser reviewed on 2024-07-12 11:53_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:244 on 2024-07-12 11:55_

I like to have named return types (over `impl`) because callers can then name those types (and e.g. store it in a `Struct`). 

I would suggest introducing a `ReadDirectory` struct similar to `std::fs::read_dir` that is a thin wrapper around `std::vec::IntoIter` (or whatever the type of your wild iter chain below is)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:161 on 2024-07-12 11:57_

Remove `dbg`

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/path.rs`:308 on 2024-07-12 11:58_


```suggestion
    pub fn as_utf8_path(&self) -> &Utf8Path {
```

To be consistent with `from_utf8_path_buf` and to match the type's name (and not the crate)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system.rs`:80 on 2024-07-12 11:58_

Nit: Some documentation? Especially around the semantics for paths that aren't UTF8

---

_@MichaReiser approved on 2024-07-12 12:00_

Neat

---

_@AlexWaygood reviewed on 2024-07-12 12:08_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/memory_fs.rs`:244 on 2024-07-12 12:08_

I see the value in doing that generally, but here I wonder if an opaque return type might actually be better? If you're e.g. relying on the exact type being returned from this method in a test (for example), you might get into difficulties if you later switch the test to using the `OsSystem` and find that a different type is returned from that struct's `read_directory()` method. I think there's some value in saying that the only API guarantee we give here is that some kind of iterator is returned.

---

_@MichaReiser reviewed on 2024-07-12 12:12_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:244 on 2024-07-12 12:12_

If the argument is API compatibility with `System` than the method should also return a `Box<dyn...>` (and accept `&SystemPath` instead of `AsRef<SystemPath>` as the argument). I think it's rare that we would switch between systems in tests and the change would be minimal. But having a concrete type can be helpful for a system implementation that's based on the memory file system (e.g. WASM).

Anyway, I don't feel strongly (except that we shouldn't change the return type to `Box<dyn>`)

---

_@AlexWaygood reviewed on 2024-07-12 12:23_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/memory_fs.rs`:244 on 2024-07-12 12:23_

I don't feel strongly either. But I propose we add it when we need it

---

_Merged by @AlexWaygood on 2024-07-12 12:31_

---

_Closed by @AlexWaygood on 2024-07-12 12:31_

---

_Branch deleted on 2024-07-12 12:31_

---

_Comment by @MichaReiser on 2024-07-12 20:45_

I just noticed one technical difference. The `path` returned on `DirEntry` by`std::fs::read_dir` returns the joined path of the path passed to `read_dir` and the entry in the directory. That means, if you call `read_dir("a")`, you get `a/bar.py` and `a/foo.py` but not an absolute path. The `MemoryFileSystem::read_directory` always returns absolute paths. 

This might be fine, but it could also be a real foot gun where tests pass because the paths are absolute and later fail in production.

---
