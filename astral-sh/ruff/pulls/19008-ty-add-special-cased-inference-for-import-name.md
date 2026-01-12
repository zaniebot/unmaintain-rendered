```yaml
number: 19008
title: "[ty] Add special-cased inference for `__import__(name)` and `importlib.import_module(name)`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: ty-dunder-import
created_at: 2025-06-28T05:30:30Z
updated_at: 2025-06-29T17:52:04Z
url: https://github.com/astral-sh/ruff/pull/19008
synced_at: 2026-01-12T15:56:29Z
```

# [ty] Add special-cased inference for `__import__(name)` and `importlib.import_module(name)`

---

_@InSyncWithFoo_

## Summary

Resolves [#718](https://github.com/astral-sh/ty/issues/718).

After this change, calls to `__import__()` and `importlib.import_module()` where the only argument is of a literal string type are resolved to literal module types instead of `types.ModuleType` (or whatever its `typeshed` signature says). The latter is also used as the fallback type when the module cannot be resolved.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-06-28 05:30_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-06-28 05:30_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-06-28 05:30_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-06-28 05:30_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-06-28 05:30_

---

_Comment by @github-actions[bot] on 2025-06-28 05:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
+ error[invalid-argument-type] src/async_utils/corofunc_cache.py:50:26: Argument to class `type` is incorrect: Expected `tuple[type, ...]`, found `tuple[typing.Protocol]`
+ error[invalid-argument-type] src/async_utils/task_cache.py:51:26: Argument to class `type` is incorrect: Expected `tuple[type, ...]`, found `tuple[typing.Protocol]`
- Found 16 diagnostics
+ Found 18 diagnostics

paroxython (https://github.com/laowantong/paroxython)
- error[unresolved-attribute] paroxython/flatten_ast.py:591:12: Type `ModuleType` has no attribute `Path`
- error[unresolved-attribute] paroxython/list_programs.py:123:16: Type `ModuleType` has no attribute `datetime`
- error[unresolved-attribute] paroxython/recommend_programs.py:332:12: Type `ModuleType` has no attribute `loads`
- error[unresolved-attribute] paroxython/recommend_programs.py:335:22: Type `ModuleType` has no attribute `literal_eval`
- Found 11 diagnostics
+ Found 7 diagnostics

rich (https://github.com/Textualize/rich)
- error[unresolved-attribute] rich/pager.py:21:16: Type `ModuleType` has no attribute `pager`
- Found 327 diagnostics
+ Found 326 diagnostics
-     memo fields = ~117MB
+     memo fields = ~106MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo fields = ~97MB
+     memo fields = ~106MB

poetry (https://github.com/python-poetry/poetry)
- error[unresolved-attribute] tests/fixtures/git/github.com/demo/namespace-package-one/namespace_package/__init__.py:4:1: Type `ModuleType` has no attribute `declare_namespace`
- Found 949 diagnostics
+ Found 948 diagnostics

vision (https://github.com/pytorch/vision)
-     memo fields = ~304MB
+     memo fields = ~276MB

altair (https://github.com/vega/altair)
-     memo fields = ~228MB
+     memo fields = ~251MB

discord.py (https://github.com/Rapptz/discord.py)
- error[unresolved-attribute] discord/__init__.py:18:12: Type `ModuleType` has no attribute `extend_path`
- Found 549 diagnostics
+ Found 548 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[unresolved-attribute] scrapy/settings/default_settings.py:358:24: Type `ModuleType` has no attribute `__version__`
- Found 1149 diagnostics
+ Found 1148 diagnostics

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~304MB
+     memo fields = ~334MB

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[unresolved-attribute] tests/appsec/iast/test_loader.py:37:13: Type `ModuleType` has no attribute `add`
- Found 6839 diagnostics
+ Found 6838 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~652MB
+ TOTAL MEMORY USAGE: ~717MB

```
</details>


---

_Comment by @InSyncWithFoo on 2025-06-28 05:38_

The ecosystem changes are all as expected.

---

_Comment by @AlexWaygood on 2025-06-28 07:47_

We may as well do `importlib.import_module` as well, no? It works in exactly the same way

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1 on 2025-06-28 07:50_

I don't think this really needs a new autocompletions test. This PR doesn't change any of the autocompletions machinery at all, it just changes type inference. It's generally true that anything which improves type inference will also improve attribute-access autocompletions, and we already have lots of tests that demonstrate this; adding another just feels duplicative

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:5403 on 2025-06-28 07:51_

Can you add a comment explaining why we exclude dotted module names (because `__import__` returns a `ModuleType` instance representing the top-level module rather than one representing the submodule)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:5391 on 2025-06-28 09:14_

I feel like I'd prefer to add a branch to `KnownFunction::check_call()` , and have that function return an `Option<Type>` like `KnownClass::check_call`, rather than create a closure that's just immediately called here.

Or even better might be to just add a branch here: https://github.com/astral-sh/ruff/blob/90cb0d3a7b0d1b9528854dad64dd17330bfd5f36/crates/ty_python_semantic/src/types/call/bind.rs#L569

---

_@AlexWaygood reviewed on 2025-06-28 09:16_

---

_Label `ty` added by @AlexWaygood on 2025-06-28 13:27_

---

_@InSyncWithFoo reviewed on 2025-06-28 17:10_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/infer.rs`:5391 on 2025-06-28 17:10_

`.evaluate_known_cases()` doesn't have access to `File`. Its call tree is all over the place too, so adding a `file` parameter to every caller is probably not the best choice.

---

_@InSyncWithFoo reviewed on 2025-06-28 17:25_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/infer.rs`:5403 on 2025-06-28 17:25_

That wasn't the intention. I just couldn't figure out how to get `<module 'collections'>` from `__import__('collections.abc')`.

---

_Renamed from "[ty] Special-case `__import__(single_name)`" to "[ty] Special-case `__import__(name)`" by @InSyncWithFoo on 2025-06-28 17:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1201 on 2025-06-28 17:34_

this could fix your dotted-name issue:

```suggestion
                let mut module_name = ModuleName::new(module_name)?;

                // `__import__("collections.abc")` returns the `collections` module;
                // `importlib.import_module("collections.abc")` returns the `collections.abc` module
                if known == KnownFunction::DunderImport {
                    module_name = ModuleName::new(module_name.components().next()?)?;
                }

                let module = resolve_module(db, &module_name)?;

                Some(Type::module_literal(db, file, &module))
```

But note that if you pass the `fromlist` parameter to `__import__`, it changes the behaviour of the function:

```pycon
>>> __import__("collections.abc", fromlist=["abc"])
<module '_collections_abc' (frozen)>
```

You also need to either account for the `level` parameter of `__import__`, or to avoid applying any special-casing when more than one argument is passed to the function. (The latter is obviously simpler.) The same goes for the optional `package` argument of `importlib.import_module`, which changes the behaviour of that function.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:5397 on 2025-06-28 17:34_

```suggestion
                                overload.set_return_type(return_type);
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/dunder_import.md`:1 on 2025-06-28 17:43_

I don't think this test file should be in the `import/` subdirectory. It doesn't have anything to do with our behaviour regarding import resolution; it demonstrates our special-casing of two standard-library functions. I think this folder would be a better fit; it's where most of our tests for our other special-cased functions are (`getatt_static`, etc.)

---

_@AlexWaygood reviewed on 2025-06-28 17:45_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2025-06-28 17:48_

---

_@AlexWaygood reviewed on 2025-06-28 17:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/dunder_import.md`:1 on 2025-06-28 17:56_

er, sorry for the garbled review comment there. I meant to link to this folder as the one I think this test file should go in: https://github.com/astral-sh/ruff/tree/main/crates/ty_python_semantic/resources/mdtest/call

---

_@InSyncWithFoo reviewed on 2025-06-28 18:12_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types/function.rs`:1201 on 2025-06-28 18:12_

Calls with `fromlist`, `level` and `package` are [less common](https://github.com/search?q=lang%3APython+%2F%28__import__%7Cimport_module%29%5C%28%5B%27%22%5D%5Cw%2B%5B%27%22%5D%2C%2F&type=code), so I'd say it's not worth supporting those unless there's a feature request.

As for the suggested code, it imports the top level module but not the submodules (see [this playground](https://play.ty.dev/2a70a4b1-1700-4b4c-b827-ec201b4d8e2e) for an example).

---

_@AlexWaygood reviewed on 2025-06-28 18:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/dunder_import.md`:1 on 2025-06-28 18:21_

Can you add some tests that demonstrate that (and explain why) we do not apply the special-casing if `fromlist`/`level` are passed to `__import__`, or if `package` is passed to `importlib.import_module`?

---

_@AlexWaygood reviewed on 2025-06-28 18:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1201 on 2025-06-28 18:23_

> As for the suggested code, it imports the top level module but not the submodules (see [this playground](https://play.ty.dev/2a70a4b1-1700-4b4c-b827-ec201b4d8e2e) for an example).

Right, we'd have to figure out a way of incorporating it into our model that the submodules are loaded onto the parent module if it's been dynamically imported with `__import__`. Which is probably not worth the bother. For this reason, I'm okay with skipping the special-casing for `__import__` if the name is dotted, as long as we add a comment explaining that this is the reason why.

If the returned object is inferred as `ModuleType`, ideally the `ModuleType.__getattr__` method would prevent false positives if users accessed submodules on the top-level module (though I think we incorrectly ignore `ModuleType.__getattr__` in all cases right now, but that's a separate bug)

---

_Comment by @codspeed-hq[bot] on 2025-06-28 21:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3Aty-dunder-import?runnerMode=WallTime)

### Merging #19008 will **degrade performances by 4.17%**

<sub>Comparing <code>InSyncWithFoo:ty-dunder-import</code> (1548503) with <code>main</code> (9218bf7)</sub>



### Summary

`❌ 1` regressions  
`✅ 7` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3Aty-dunder-import?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` multithreaded[pydantic] `` | 8.6 s | 9 s | -4.17% |


---

_Comment by @AlexWaygood on 2025-06-28 21:40_

That specific benchmark has been flaky, I wouldn't worry about it

---

_@AlexWaygood approved on 2025-06-29 10:48_

---

_Renamed from "[ty] Special-case `__import__(name)`" to "[ty] Add special-cased inference for `__import__(name)` and `importlib.import_module(name)`" by @AlexWaygood on 2025-06-29 10:49_

---

_Merged by @AlexWaygood on 2025-06-29 10:49_

---

_Closed by @AlexWaygood on 2025-06-29 10:49_

---

_Branch deleted on 2025-06-29 14:55_

---
