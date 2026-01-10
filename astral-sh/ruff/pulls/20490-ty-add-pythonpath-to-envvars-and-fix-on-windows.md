```yaml
number: 20490
title: "[ty] Add PYTHONPATH to EnvVars and fix on Windows"
type: pull_request
state: merged
author: mmlb
labels:
  - ty
assignees: []
merged: true
base: main
head: push-pqxtsnrxkuly
created_at: 2025-09-20T21:23:23Z
updated_at: 2025-09-23T13:20:15Z
url: https://github.com/astral-sh/ruff/pull/20490
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Add PYTHONPATH to EnvVars and fix on Windows

---

_Pull request opened by @mmlb on 2025-09-20 21:23_

## Summary

* Add `PYTHONPATH` to `ty_static::EnvVars` so it shows up in envvar docs.
* Remove custom env path splitting with hard coded separator and use stdlib instead, fixing windows. 

## Test Plan

No new tests added.


---

_Review requested from @carljm by @mmlb on 2025-09-20 21:23_

---

_Review requested from @MichaReiser by @mmlb on 2025-09-20 21:23_

---

_Review requested from @sharkdp by @mmlb on 2025-09-20 21:23_

---

_Review requested from @dcreager by @mmlb on 2025-09-20 21:23_

---

_Review requested from @AlexWaygood by @mmlb on 2025-09-20 21:23_

---

_Comment by @mmlb on 2025-09-20 21:25_

The EnvVar bit came out of https://github.com/astral-sh/ty/issues/1219 and coming up with a doc for that issue I checked CPython's own docs and noted the mention of os.pathsep and :man_facepalming: so fixed it here too. Not sure how to add a windows test and also unsure if the `panic!` is the right call in system.rs

---

_Review comment by @mmlb on `crates/ruff_db/src/system.rs`:196 on 2025-09-20 21:25_

not sure if there's a better alternative

---

_Comment by @github-actions[bot] on 2025-09-20 21:25_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:308 on 2025-09-20 21:25_

should this be `to_string_lossy` instead?

---

_@mmlb reviewed on 2025-09-20 21:26_

---

_Label `ty` added by @AlexWaygood on 2025-09-20 21:31_

---

_Comment by @github-actions[bot] on 2025-09-20 21:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @github-actions[bot] on 2025-09-20 21:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ruff_db/src/system.rs`:196 on 2025-09-22 10:24_

We shouldn't provide a default implementation that always panics. Instead, each `System` implementation should add its own implementation. 
* Implementing it for `TestSystem` is trivial because we can forward to the inner system (the same as all other methods do). 
* Implementing it for `LSPSystem` is trivial too, simply forward to the `native_system`
* Implementing it for `MemorySystem` is a bit trickier. I'd be inclined to simply split by `:` 

The method also can't return `std::env::SplitPaths` because it means that the only possible implementation is `std::env::split_paths`. The return type either has to be a `vec[SystemPath]` or `Box<dyn Iterator<Item=&'a SystemPath>>`


Having said all this. We've only added methods to `System` when we had a need to abstract over them. For example, we only added `env_var` because we ran into issues with test isolation. All file system operations exist because we need to "patch" some in the LSP case AND because we wanted to use a memory file system in tests. 

That's why I'd suggest removing the method for now. We can always add it to `System` when the need arises and it simplifies our lifes now :)

There are other platform-specific behaviors we're currently not normalizing by using `System`. For example, `SystemPath` uses the underlying `Path` type. That means Windows and Unix use different path representations. We could normalize all paths to Unix paths but we haven't seen a need for it just yet.

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:307 on 2025-09-22 10:24_

Oh nice find with `split_paths`. I think we should update our CLI test to correctly set the `PYTHONPATH` variable depending on the platform (by using `std::env::join_paths`)

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:308 on 2025-09-22 10:28_

What you have here is fine but I suggest using `let Ok(path) = ... else {` and using `SystemPathBuf::from_std_path` instead (which avoids any allocations if the path is a valid UTF8 path)


```rust
                let Ok(path) = SystemPathBuf::from_path_buf(path) else {
                    tracing::debug!(
                        "Skipping `{path}` listed in `PYTHONPATH` because it is not a valid UTF-8 path."
                    );
                    continue;
                };

                let possible_path = SystemPath::absolute(path, system.current_directory());
```

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:323 on 2025-09-22 10:33_

Reading this code again. I'm wondering what the precedence between `PYTHONPATH` and `extra_paths` should be. What's implemented now is that `PYTHONPATH` takes precedence over the user-provided `extra_paths`. This contradicts the extra paths documentation.

https://github.com/astral-sh/ruff/blob/2fec3f75ee42a149ccb39e498470f1300ec93466/crates/ty_project/src/metadata/options.rs#L546-L548

@AlexWaygood do you think it makes sense that `PYTHONPATH` takes precedence over `extra_paths`, or should it be the other way round?

---

_Review comment by @MichaReiser on `crates/ty_static/src/env_vars.rs`:45 on 2025-09-22 10:34_

I don't think I'd know what this means as a ty user (what does augmented mean in this case?)

uv's documentation for `PYTHONPATH` is 

```
Adds directories to Python module search path (e.g., PYTHONPATH=/path/to/modules).
```

I suggest something similar

Adds additional directories to ty's search paths. The format is the same as the shell’s PATH: one or more directory pathnames separated by [os.pathsep](https://docs.python.org/3/library/os.html#os.pathsep) (e.g. colons on Unix or semicolons on Windows). 

---

_@MichaReiser reviewed on 2025-09-22 10:35_

Thank you. A few nits and suggestions around the documentation. I suggest removing `split_paths` from `System` for now unless there's a need today (e.g. the wasm tests are failing in CI)

---

_@AlexWaygood reviewed on 2025-09-22 11:18_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:323 on 2025-09-22 11:18_

I agree with you — I think ty's explicit configuration (`extra_paths`) should take precedence over paths in `PYTHONPATH`, which is an external environment variable (used for configuring Python itself) that isn't ty-specific

---

_@mmlb reviewed on 2025-09-22 13:45_

---

_Review comment by @mmlb on `crates/ruff_db/src/system.rs`:196 on 2025-09-22 13:45_

my initial thought was to just use os.pathsep equivalent, but its not exposed. I flirted with just defining our own and using with the split I had already but then switched over to the stdlib's function. I'll just go back to my first PR with platform specific separator, that seemed the cleanest way.

---

_@mmlb reviewed on 2025-09-22 13:58_

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:308 on 2025-09-22 13:58_

Going back to previous code that was simpler, so this is unnecessary.

---

_Comment by @mmlb on 2025-09-22 14:35_

I've updated the PR by using a platform specific separator instead of the hard coded `:` and added join_paths based test to ensure correctness. I've also switched the precedence between PYTHONPATH and extra-paths as requested. PTAL.

---

_Renamed from "[ty]: Add PYTHONPATH to EnvVars and fix on Windows" to "[ty] Add PYTHONPATH to EnvVars and fix on Windows" by @mmlb on 2025-09-22 14:49_

---

_Comment by @mmlb on 2025-09-22 15:16_

Looks like windows is failing to replace `C:\...` with `<temp_dir>`. Not sure if its because `insta::with_filters` needs `C:\` or if I did something weird.

---

_@MichaReiser reviewed on 2025-09-22 16:37_

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/python_environment.rs`:1934 on 2025-09-22 16:37_

I'm not sure what's different with this test that the path filtering doesn't work. Can you try splitting this second case into its own test, just to ensure that two `CliTest` don't interfere with each other (in some unfortunate way?)

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/python_environment.rs`:1934 on 2025-09-22 17:59_

Glad that it worked. I'm not sure why because `CliTest` does use `add_filter`

---

_@MichaReiser reviewed on 2025-09-22 17:59_

---

_@mmlb reviewed on 2025-09-22 18:01_

---

_Review comment by @mmlb on `crates/ty/tests/cli/python_environment.rs`:1934 on 2025-09-22 18:01_

Hmm looks like that worked for some reason...

---

_@MichaReiser reviewed on 2025-09-22 18:01_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:313 on 2025-09-22 18:01_

I think we may still want to use `split_paths` here for:

> This also performs unquoting on Windows.



---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:313 on 2025-09-22 18:02_

Ah I didn't see that previously.

---

_@mmlb reviewed on 2025-09-22 18:02_

---

_@mmlb reviewed on 2025-09-22 18:44_

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:313 on 2025-09-22 18:44_

So with that known, I'm thinking of going back to std::env::split_paths will that work for wasm (I think that's the only potential issue, LSP, Test, Memory Systems don't really influence here specifically)? I'll convert to SystemPath too.

---

_Review request for @carljm removed by @carljm on 2025-09-22 19:43_

---

_Comment by @mmlb on 2025-09-22 20:20_

Ok looks like CI is happy and I've gone back to std::env::split_paths with `SystemPath::from_std_path` to avoid allocating on valid utf8. If thats all good I'll squash the shouty commits into appropriate parent and re-push.

---

_@MichaReiser approved on 2025-09-23 08:23_

Thank you. This is great. 

We squash the commits on merge. So there's no need on your end to squash the commits before merging.

---

_Merged by @MichaReiser on 2025-09-23 08:27_

---

_Closed by @MichaReiser on 2025-09-23 08:27_

---

_Branch deleted on 2025-09-23 13:20_

---
