```yaml
number: 19996
title: "[ty] Experimental persistent caching"
type: pull_request
state: open
author: ibraheemdev
labels:
  - ty
assignees: []
base: main
head: ibraheem/persistent-caching
created_at: 2025-08-19T21:57:46Z
updated_at: 2025-08-22T23:41:18Z
url: https://github.com/astral-sh/ruff/pull/19996
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Experimental persistent caching

---

_Pull request opened by @ibraheemdev on 2025-08-19 21:57_

## Summary

Adds persistent caching experimentally under the `TY_PERSIST={path_to_cache_file}` environment variable. This currently has many limitations and is not ready for public usage:
- Performance is quite bad and significantly slower than a cold run.
- The size of the file grows unbounded across runs, because we persist stale values.
- The cache file is determined by the environment variable instead of using a `.ty` director with a key for the given project configuration. I intentionally avoided this for now.

Limitations aside, I still want to land this to make it easier to iterate on in-tree.

---

_Label `ty` added by @ibraheemdev on 2025-08-19 21:57_

---

_Review requested from @carljm by @ibraheemdev on 2025-08-19 21:57_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-08-19 21:57_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-08-19 21:57_

---

_Review requested from @dcreager by @ibraheemdev on 2025-08-19 21:57_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-08-19 21:57_

---

_@ibraheemdev reviewed on 2025-08-19 21:59_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:74 on 2025-08-19 21:59_

@MichaReiser I'm not actually sure if we need a cache key, because the `Program` input is persisted, and its fields are updated (bumping the revision) if the settings have changed? A local `.ty_cache` file/directory might be enough.

---

_@ibraheemdev reviewed on 2025-08-19 21:59_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/metadata.rs`:38 on 2025-08-19 21:59_

`bincode` doesn't support the `serde(skip_serializing_if)` or `serde(untagged)` attributes.

---

_@ibraheemdev reviewed on 2025-08-19 22:01_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/semantic_index/scope.rs`:23 on 2025-08-19 22:01_

This is a notable change, and might have an impact on performance.

---

_@ibraheemdev reviewed on 2025-08-19 22:01_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:338 on 2025-08-19 22:01_

I'm not sure exactly what to do about this, we can't deserialize into a `&'static str` (without leaking). Making this store a `String`/`Cow` is really hard because `Type` is `Copy`. Could we use an enum here?

---

_@ibraheemdev reviewed on 2025-08-19 22:02_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:427 on 2025-08-19 22:02_

I disabled garbage collection for all interned values because it significantly reduces the amount of data we need to persist, but I'm not sure if that change should be made in this PR.

---

_Comment by @github-actions[bot] on 2025-08-19 22:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-22 23:38:30.325218289 +0000
+++ new-output.txt	2025-08-22 23:38:32.929235818 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(bcb1)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a0e7a06/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(bcb1)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-19 22:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct metadata = ~2MB
+     struct metadata = ~3MB

trio (https://github.com/python-trio/trio)
-     memo metadata = ~22MB
+     memo metadata = ~21MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~26MB
+     struct metadata = ~27MB

```
</details>


---

_@carljm reviewed on 2025-08-19 22:21_

This looks generally fine to me, assuming CI is cleared up and there are no observed regressions. Ideally I'd prefer @MichaReiser to take a look, if he has time.

If there is a noticeable performance regression for non-persisting cases, I'd be hesitant to pay that cost to land an experimental feature that we haven't even confirmed can be a net win, and I'd hope we can find a way to eliminate that regression, even if it's via conditional compilation.

---

_@MichaReiser reviewed on 2025-08-20 07:05_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:74 on 2025-08-20 07:05_

A not uncommon use case is to check a project with different Python versions: Each run then uses a different `ProgramSettings`. Ideally, the caches would be separate 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:872 on 2025-08-20 07:13_

It's not the end of the world but it seems unfortunate if we have to change such fundamental types for an experimental feature. 

The alternative here would be to retrieve the lint registry from the database (or a thread local) during deserialization and to use it to resolve the lint metadata. 

https://github.com/astral-sh/ruff/blob/99d0ac60b417ee623bb9257a6502a7c21c444b28/crates/ty_python_semantic/src/db.rs#L14

---

_Review comment by @MichaReiser on `crates/ruff_source_file/src/lib.rs`:168 on 2025-08-20 07:18_

I'd prefer if we don't add serialize implementation for those types. We could simply panic in the `UnifiedFile` implementation whenever we encounter a ruff file.

The reason is that it seems unnecessary to maintain this complexity when Ruff doesn't even use salsa's caching and serializing the line index is a little useles (it's very cheap to build it from the source)



---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:150 on 2025-08-20 07:22_

Does this mean that all deserialized options now have a source CLI even if they were originally loaded from a settings file?

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:187 on 2025-08-20 07:23_

What happens if the user moved the `metadata` file (e.g. if they moved it a directory up)? Do we replace the metadata somewhere (by calling update project), or do we use a stale configuration?

I think what would be ideal is if we wouldn't serialize the project settings at all and always load them instead. But that would require some form of hashing to know when they've changed (including which fields) which is out of scope for now

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:87 on 2025-08-20 07:25_

and the reason for the `OnceLock` is so that we can change the field without bumping the salsa revision?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/python_platform.rs`:19 on 2025-08-20 07:31_

`PythonPlatform` is used in the user-facing options struct: I don't see any schema changes, but we can't remove it here if it does change the schema (or we have to use another type in the user-facing options)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/place.rs`:146 on 2025-08-20 07:31_

Do all the `serialize` / `deserialize` impls notably impact the build time?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/scope.rs`:23 on 2025-08-20 07:34_

Hmm, this seems very unfortunate as it now introduces a global lock into semantic index building (which I think is the first?). 

Can you run some benchmarks on large projects to see how it impacts performance? It seems a very unfortunate change for an experimental feature where it isn't even clear if we'll use it long term.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:338 on 2025-08-20 07:37_

We could do the same as what you did with `AstNodeRef` where we use an `Option<&'static str>` and set it to `None` when deserializing. This could lead to issues downstream because types now suddenly compare equal that didn't before but I wonder if that should even be the case, given that the release implementation's equal implementation never compares the string.  

An enum is an option too

I don't think we should spend too much time working around this because the ultimate goal is to remove this type entirely


CC: @sharkdp 



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:427 on 2025-08-20 07:38_

I think this should be a separate PR and requires some reasoning why it's fine to do this

---

_@MichaReiser reviewed on 2025-08-20 07:42_

Thank you.

I need to take a closer look at the project serialization/deserialization. It's not entirely clear to me how that works. 

Overall, I'd prefer if this PR made fewer changes to the overall code base, given that this is an experimental feature that we might decide not to ship. This is mainly about changing `LintName` to use a `Cow` and disabling interning. For the latter, I think the solution is to gate the more questionable changes behind a feature flag. This allows us to pull this change in without regressing the production experience and gives us some more time to a) figure out a solution for those problem b) decide that its what we want, given that it enables persistent caching.

---

_@sharkdp reviewed on 2025-08-20 07:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:338 on 2025-08-20 07:55_

> We could do the same as what you did with `AstNodeRef` where we use an `Option<&'static str>` and set it to `None` when deserializing. This could lead to issues downstream because types now suddenly compare equal that didn't before but I wonder if that should even be the case, given that the release implementation's equal implementation never compares the string.

Hmm.. yes. I think we should be fine if all `TodoType`s compare equal. Maybe that will lead to some observable differences (different todo messages showing up in `reveal_type` assertions?), but those differences should never be behavioral. All of these types are completely dynamic types and should behave the same way. And if it's not consistent between debug and release right now, maybe that's something we should fix anyway (always have them compare equal, because there's no other way in release mode).

Instead of `Option<&'static str>` you could maybe also deserialize these messages as `"<deserialized>"`, making it clear that the todo-messages are not persisted?

> I don't think we should spend too much time working around this because the ultimate goal is to remove this type entirely

I agree.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-20 10:17_

---

_@ibraheemdev reviewed on 2025-08-20 18:05_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:150 on 2025-08-20 18:05_

The only place I see the options being created is [with `ValueSource::Cli` here](https://github.com/astral-sh/ruff/blob/main/crates/ty/src/args.rs#L184). Are they created elsewhere with a different source?

---

_@ibraheemdev reviewed on 2025-08-20 18:06_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:187 on 2025-08-20 18:06_

This was incorrect, I updated the check to be `project.metadata(db) == metadata`. If the metadata has changed, i.e. there is no project with matching metadata, a new `Project` input will be created.

---

_@ibraheemdev reviewed on 2025-08-20 18:07_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/lib.rs`:87 on 2025-08-20 18:07_

Yes, the settings should not change if the metadata has not changed.

---

_@ibraheemdev reviewed on 2025-08-20 18:10_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/python_platform.rs`:19 on 2025-08-20 18:10_

Should there be failing tests if the schema changes? This does change the representation.

---

_@MichaReiser reviewed on 2025-08-20 18:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/python_platform.rs`:19 on 2025-08-20 18:13_

Yes, there should be a failing test

---

_@MichaReiser reviewed on 2025-08-20 18:14_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:150 on 2025-08-20 18:14_

https://github.com/astral-sh/ruff/blob/f34b65b7a00dfc61c5a54e4280b421c94151e677/crates/ty_project/src/metadata.rs#L166-L177

---

_@ibraheemdev reviewed on 2025-08-20 18:16_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:150 on 2025-08-20 18:16_

Right, should we serialize the source at the top-level (in `Options`)?

---

_@ibraheemdev reviewed on 2025-08-20 18:31_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/python_platform.rs`:19 on 2025-08-20 18:31_

I rewrote the serde implementations in a way that bincode accepts, but there should be no change in behavior.

---

_@ibraheemdev reviewed on 2025-08-20 18:32_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/metadata.rs`:38 on 2025-08-20 18:32_

@MichaReiser is this one public facing?

---

_@MichaReiser reviewed on 2025-08-20 19:11_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:150 on 2025-08-20 19:11_

We can't. The options are merged together from multiple sources. We'd need to preserve them per value, which might be tricky

---

_@ibraheemdev reviewed on 2025-08-20 19:17_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:150 on 2025-08-20 19:17_

Is there a reason that `RangedValue` ignores the source when serializing/deserializing in the first place?

---

_Comment by @github-actions[bot] on 2025-08-20 19:42_

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

_@ibraheemdev reviewed on 2025-08-20 20:11_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/semantic_index/scope.rs`:23 on 2025-08-20 20:11_

I'm actually seeing a 7% speedup locally?

```bash
$ hyperfine "./target/release/ty-new check --quiet --python ./prefect/.venv" "./target/release/ty-old check --quiet --python ./prefect/.venv" --ignore-failure --warmup 10
Benchmark 1: ./target/release/ty-new check --quiet --python ./prefect/.venv
  Time (mean ± σ):     674.4 ms ±  21.7 ms    [User: 7657.4 ms, System: 1588.2 ms]
  Range (min … max):   644.4 ms … 721.5 ms    10 runs
 
Benchmark 2: ./target/release/ty-old check --quiet --python ./prefect/.venv
  Time (mean ± σ):     722.3 ms ±  13.0 ms    [User: 7699.8 ms, System: 1694.5 ms]
  Range (min … max):   703.2 ms … 744.5 ms    10 runs
 
Summary
  ./target/release/ty-new check --quiet --python ./prefect/.venv ran
    1.07 ± 0.04 times faster than ./target/release/ty-old check --quiet --python ./prefect/.venv
```

Codspeed [reports a similar speedup on the multithreaded benchmark](https://codspeed.io/astral-sh/ruff/runs/compare/68a622dc2992c5c139cf3e1e..68a6249e7162a5372439a629).

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:150 on 2025-08-21 08:56_

It only ignores it when serializing. The deserialization uses the serde_toml's spanned feature to get the span and we get the source from the thread local:

https://github.com/astral-sh/ruff/blob/f34b65b7a00dfc61c5a54e4280b421c94151e677/crates/ty_project/src/metadata/value.rs#L281-L308

We don't serialize the span and source because the range is only for better diagnostics. It isn't actually part of the data itself. Now, the good news is that we only use serialization for tests. So we would consider serializing the ranges (or only if a specific thread local is set) and deserialize the ranges from the data if present (over using the thread local). I guess, we could always serialize/deserialize the source if the thread local isn't set

---

_@MichaReiser reviewed on 2025-08-21 08:56_

---

_@ibraheemdev reviewed on 2025-08-21 17:30_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:150 on 2025-08-21 17:30_

I updated the code to use the new `ProjectMetadata` when resolving the settings (and program settings), so the new value sources are propagated.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:943 on 2025-08-22 08:37_

Should we include the name in the error message or does serde do this for us?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files/file_root.rs`:46 on 2025-08-22 08:38_

We'll want to have some form of garbage collection here to aovid creating file roots for paths that no longer exists but we can defer that to later.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:273 on 2025-08-22 08:40_

It might be worth asserting here that no such file existed and panic otherwise 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:450 on 2025-08-22 08:42_

The `key.key_index` feels a bit magical and we shouldn't have to use an API from `salsa::plumbing`. Could `entries` return a struct that provides an `as_struct()` method (or similar) to encapsulate this logic (I'm fine if this is done as a follow up, it just feels a bit off)

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/value.rs`:62 on 2025-08-22 08:43_

Can we leave this `pub(super)`, now that we load the settings from the "fresh" metadata?

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:149 on 2025-08-22 08:44_

Can we add a comment explaining why it's okay to use `ValueSource::Cli` here

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:155 on 2025-08-22 08:47_

Can we use `DEFAULT_LINT_REGISTRY` instead?  It avoids building the lint registry everytime we lookup a rule. 



---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:177 on 2025-08-22 08:49_

It may help if we run this multi threaded, but that's something we can leave for a separate PR

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:207 on 2025-08-22 08:50_

Nit: Maybe use `encode_into_std_write`

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:166 on 2025-08-22 08:50_

Nit: Maybe use `decode_from_std_read`

Edit: Actually, we can't do that. We need to use `system.read_to_string` and use `system.as_writeable().write_file` to write the changes back

---

_Review comment by @MichaReiser on `crates/ty_project/src/files.rs`:23 on 2025-08-22 08:52_

I think we should skip serializing `IndexedFiles` because we need to reload all files anyway. Unless that isn't easily possible because it then always results in a change?

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:87 on 2025-08-22 08:52_

Let's extend this documentation to also mention why it uses a `OnceLock` instead of an `Option`. Same for the other fields

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:75 on 2025-08-22 08:57_


```suggestion
        if let Ok(path) = system.env_var(EnvVars::TY_PERSIST) {
```

To avoid your local `TY_PERSIST` environment variable leaking into tests

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:141 on 2025-08-22 08:58_


```suggestion
    fn deserialize(&mut self, path: &str, metadata: &ProjectMetadata) -> anyhow::Result<()> {
```

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:177 on 2025-08-22 09:01_

Nit: `resolve_settings` seems to be only used by deserialization but its name doesn't suggest that. Maybe `init_after_deserialization`?

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata.rs`:38 on 2025-08-22 09:02_

No, not yet :) We only use serialization in tests (that's why `cfg_attr(test)`)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/scope.rs`:22 on 2025-08-22 09:05_

Let's add a `TODO` to consider reverting to a tracked struct once supported by the persistent caching backend.

---

_Review comment by @MichaReiser on `crates/ty_static/src/env_vars.rs`:45 on 2025-08-22 09:08_

Let's add a note that this is highly experimental and currently makes ty slower.

---

_@MichaReiser approved on 2025-08-22 09:11_

Thank you. This looks great. I've a few smaller comments that would be great to address before landing but nothing major. 

The only thing we should do before landing is to add a CLI test that exercises the `persist`, `load` to prevent us from accidentially breaking this feature. It would be nice to have more extensive tests that verify that e.g. changes to the files that need checking or settings propagate as expected but this is something we can add when we fully commit to persistent caching.

See the `cli` module for how to add CLI tests 

https://github.com/astral-sh/ruff/blob/4daf59e5e7f4613eeb5b16616d6f1021297fc12e/crates/ty/tests/cli/main.rs#L18 

---

_@MichaReiser reviewed on 2025-08-22 09:30_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/scope.rs`:23 on 2025-08-22 09:30_

It's funny that this is the case and i don't fully understand it. Especially considering that it doubles the metadata size for `ScopeId`

main:

`ScopeId`                                          metadata=27.84MB  fields=11.93MB  count=994370

this branch:

`ScopeId`                                          metadata=51.71MB  fields=11.93MB  count=994346

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:166 on 2025-08-22 23:03_

I'm also not sure if it would be faster to deserialize streaming. serde_json at least has performance issues with this and recommends reading into a temporary buffer first.

---

_@ibraheemdev reviewed on 2025-08-22 23:03_

---

_@ibraheemdev reviewed on 2025-08-22 23:04_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/metadata/value.rs`:62 on 2025-08-22 23:04_

We still need it to set the dummy `ValueSource::Cli` values.

---

_@ibraheemdev reviewed on 2025-08-22 23:05_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/semantic_index/scope.rs`:22 on 2025-08-22 23:05_

Hmm, I'm not sure how this would work without persisting the `semantic_index` query.

---

_@ibraheemdev reviewed on 2025-08-22 23:34_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/files.rs`:23 on 2025-08-22 23:34_

Hmm.. updating the `file_set` field is problematic. The `semantic_index` query has a dependency on `file_set` (for example, `report_semantic_error` checks if the file is in the set), and so `file_set` ends up in the flattened dependencies of `infer_scope_types`.

I think we need to make  `should_check_file` a tracked function so that the `infer_scope_types` queries have finer-grained dependencies on specific files instead of the entire set. I'm going to leave this as a TODO for now as it also requires Salsa changes (to avoid flattening transitive queries if they're marked as persistable).

---

_Comment by @ibraheemdev on 2025-08-22 23:37_

I think I've addressed most of the comments, but this still needs tests. I also want to do some benchmark to see why making `ScopeId` an interned struct is a performance improvement.

---
