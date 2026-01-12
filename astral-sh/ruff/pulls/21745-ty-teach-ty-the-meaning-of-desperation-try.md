```yaml
number: 21745
title: "[ty] Teach `ty` the meaning of desperation (try ancestor `pyproject.toml`s as search-paths if module resolution fails)"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: gankra/workup
created_at: 2025-12-02T01:31:23Z
updated_at: 2025-12-03T20:42:58Z
url: https://github.com/astral-sh/ruff/pull/21745
synced_at: 2026-01-12T15:57:32Z
```

# [ty] Teach `ty` the meaning of desperation (try ancestor `pyproject.toml`s as search-paths if module resolution fails)

---

_@Gankra_

## Summary

This makes an importing file a required argument to module resolution, and if the fast-path cached query fails to resolve the module, take the slow-path uncached (could be cached if we want) `desperately_resolve_module` which will walk up from the importing file until it finds a `pyproject.toml` (arbitrary decision, we could try every ancestor directory), at which point it takes one last desperate attempt to use that directory as a search-path. We do not continue walking up once we've found a `pyproject.toml` (arbitrary decision, we could keep going up).

Running locally, this fixes every broken-for-workspace-reasons import in pyx's workspace!

* Fixes https://github.com/astral-sh/ty/issues/1539
* Improves https://github.com/astral-sh/ty/issues/839

## Test Plan

The workspace tests see a huge improvement on most absolute imports.


---

_Label `ty` added by @Gankra on 2025-12-02 01:31_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 01:33_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @Gankra on 2025-12-02 01:33_

The uses of `resolve_module_old` are one of:

* We're looking up a KnownModule so this logic is irrelevant (should probably continue to use the original logic)
* We're in unit tests and I really didn't want to update 100 test invocations (could be updated)
* It took me more than 30 seconds to see how to get the importing_file and so I skipped it (should probably be updated)

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 01:35_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- pyodide-build/pyodide_build/_py_compile.py:14:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/_py_compile.py:15:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/build_env.py:16:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/build_env.py:17:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/build_env.py:83:10: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv`
- pyodide-build/pyodide_build/build_env.py:127:10: error[unresolved-import] Cannot resolve imported module `pyodide_build.config`
- pyodide-build/pyodide_build/build_env.py:217:14: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.loader`
- pyodide-build/pyodide_build/cli/build.py:13:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/cli/build.py:18:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/cli/build.py:19:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/cli/build.py:20:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.out_of_tree`
- pyodide-build/pyodide_build/cli/build.py:21:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.out_of_tree.pypi`
- pyodide-build/pyodide_build/cli/build.py:26:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.pypabuild`
- pyodide-build/pyodide_build/cli/build.py:27:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.spec`
- pyodide-build/pyodide_build/cli/build_recipes.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/cli/build_recipes.py:8:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/cli/build_recipes.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/cli/build_recipes.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/cli/build_recipes.py:11:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
- pyodide-build/pyodide_build/cli/build_recipes.py:12:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.builder`
- pyodide-build/pyodide_build/cli/config.py:3:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/cli/config.py:8:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.config`
- pyodide-build/pyodide_build/cli/create_zipfile.py:6:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.pyzip`
- pyodide-build/pyodide_build/cli/py_compile.py:6:6: error[unresolved-import] Cannot resolve imported module `pyodide_build._py_compile`
+ pyodide-build/pyodide_build/cli/skeleton.py:196:17: error[invalid-argument-type] Argument to function `make_package` is incorrect: Expected `Literal["wheel", "sdist"] | None`, found `str`
- pyodide-build/pyodide_build/cli/skeleton.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/cli/skeleton.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/cli/skeleton.py:11:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
- pyodide-build/pyodide_build/cli/skeleton.py:180:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyodide-build/pyodide_build/cli/venv.py:5:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/cli/venv.py:6:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.out_of_tree`
- pyodide-build/pyodide_build/cli/xbuildenv.py:5:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/cli/xbuildenv.py:6:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/cli/xbuildenv.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.views`
- pyodide-build/pyodide_build/cli/xbuildenv.py:8:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv`
- pyodide-build/pyodide_build/cli/xbuildenv.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv_releases`
- pyodide-build/pyodide_build/common.py:31:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/common.py:36:14: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/common.py:62:14: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/common.py:315:10: error[unresolved-import] Cannot resolve imported module `pyodide_build.vendor.loky`
- pyodide-build/pyodide_build/config.py:8:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/config.py:13:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.constants`
- pyodide-build/pyodide_build/config.py:14:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/io.py:2:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.spec`
- pyodide-build/pyodide_build/out_of_tree/build.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/out_of_tree/build.py:8:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/out_of_tree/build.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.spec`
- pyodide-build/pyodide_build/out_of_tree/pypi.py:30:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/out_of_tree/pypi.py:31:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/out_of_tree/pypi.py:32:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/out_of_tree/pypi.py:33:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.out_of_tree`
- pyodide-build/pyodide_build/out_of_tree/pypi.py:34:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.spec`
- pyodide-build/pyodide_build/out_of_tree/venv.py:8:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/out_of_tree/venv.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/out_of_tree/venv.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
+ pyodide-build/pyodide_build/pypabuild.py:52:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `_DefaultIsolatedEnv`
+ pyodide-build/pyodide_build/pypabuild.py:266:5: error[no-matching-overload] No overload of function `copy2` matches arguments
+ pyodide-build/pyodide_build/pypabuild.py:271:5: error[no-matching-overload] No overload of function `copy2` matches arguments
- pyodide-build/pyodide_build/pypabuild.py:17:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/pypabuild.py:18:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/pypabuild.py:26:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.spec`
- pyodide-build/pyodide_build/pypabuild.py:27:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.vendor._pypabuild`
- pyodide-build/pyodide_build/pypabuild.py:95:10: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/pypabuild.py:317:10: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/pyzip.py:6:6: error[unresolved-import] Cannot resolve imported module `pyodide_build._py_compile`
- pyodide-build/pyodide_build/pyzip.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/recipe/bash_runner.py:12:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/recipe/bash_runner.py:16:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/recipe/bash_runner.py:17:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/recipe/builder.py:21:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/recipe/builder.py:22:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/recipe/builder.py:35:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/recipe/builder.py:46:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/recipe/builder.py:47:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.bash_runner`
- pyodide-build/pyodide_build/recipe/builder.py:51:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.spec`
+ pyodide-build/pyodide_build/recipe/builder.py:801:53: error[invalid-argument-type] Argument to function `find_matching_wheel` is incorrect: Expected `str`, found `str | None`
- pyodide-build/pyodide_build/recipe/graph_builder.py:30:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/recipe/graph_builder.py:31:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/recipe/graph_builder.py:32:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/recipe/graph_builder.py:40:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/recipe/graph_builder.py:41:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
- pyodide-build/pyodide_build/recipe/graph_builder.py:42:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.builder`
- pyodide-build/pyodide_build/recipe/graph_builder.py:43:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.spec`
- pyodide-build/pyodide_build/recipe/loader.py:5:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/recipe/loader.py:6:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.spec`
- pyodide-build/pyodide_build/recipe/loader.py:86:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyodide-build/pyodide_build/recipe/skeleton.py:18:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/recipe/skeleton.py:19:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/recipe/spec.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.spec`
- pyodide-build/pyodide_build/recipe/unvendor.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/tests/conftest.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/conftest.py:8:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/tests/conftest.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv`
- pyodide-build/pyodide_build/tests/conftest.py:106:10: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/tests/recipe/test_bash_runner.py:4:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
+ pyodide-build/pyodide_build/tests/recipe/test_builder.py:242:9: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `str`
- pyodide-build/pyodide_build/tests/recipe/test_builder.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/recipe/test_builder.py:11:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/tests/recipe/test_builder.py:12:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
- pyodide-build/pyodide_build/tests/recipe/test_builder.py:13:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.builder`
- pyodide-build/pyodide_build/tests/recipe/test_builder.py:20:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.spec`
- pyodide-build/pyodide_build/tests/recipe/test_graph_builder.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/recipe/test_graph_builder.py:11:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.build_env`
- pyodide-build/pyodide_build/tests/recipe/test_graph_builder.py:12:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
- pyodide-build/pyodide_build/tests/recipe/test_loader.py:5:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
- pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
- pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:11:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.skeleton`
- pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:17:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.spec`
+ pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:108:9: error[invalid-argument-type] Argument is incorrect: Expected `_PackageSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:109:9: error[invalid-argument-type] Argument is incorrect: Expected `_SourceSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:113:9: error[invalid-argument-type] Argument is incorrect: Expected `_TestSpec`, found `dict[Unknown | str, Unknown | list[Unknown | str]]`
+ pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:216:9: error[invalid-argument-type] Argument is incorrect: Expected `_PackageSpec`, found `dict[Unknown | str, Unknown | str | bool]`
+ pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:217:9: error[invalid-argument-type] Argument is incorrect: Expected `_SourceSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_skeleton.py:221:9: error[invalid-argument-type] Argument is incorrect: Expected `_TestSpec`, found `dict[Unknown | str, Unknown | list[Unknown | str]]`
- pyodide-build/pyodide_build/tests/recipe/test_spec.py:4:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe.spec`
- pyodide-build/pyodide_build/tests/recipe/test_unvendor.py:4:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.recipe`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:18:13: error[invalid-argument-type] Argument is incorrect: Expected `_PackageSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:19:13: error[invalid-argument-type] Argument is incorrect: Expected `_SourceSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:20:13: error[invalid-argument-type] Argument is incorrect: Expected `_RequirementsSpec`, found `dict[Unknown | str, Unknown | list[Unknown | str]]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:29:30: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `Literal["b"]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:37:21: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `Literal["b"]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:54:9: error[invalid-argument-type] Argument is incorrect: Expected `_PackageSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:55:9: error[invalid-argument-type] Argument is incorrect: Expected `_SourceSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:56:9: error[invalid-argument-type] Argument is incorrect: Expected `_RequirementsSpec`, found `dict[Unknown | str, Unknown | list[Unknown]]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:71:9: error[invalid-argument-type] Argument is incorrect: Expected `_PackageSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_spec.py:72:9: error[invalid-argument-type] Argument is incorrect: Expected `_SourceSpec`, found `dict[Unknown | str, Unknown | str]`
+ pyodide-build/pyodide_build/tests/recipe/test_unvendor.py:17:12: warning[possibly-missing-attribute] Attribute `exists` may be missing on object of type `Path | None`
+ pyodide-build/pyodide_build/tests/recipe/test_unvendor.py:18:12: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Path | None`
+ pyodide-build/pyodide_build/tests/recipe/test_unvendor.py:21:27: error[invalid-argument-type] Argument to function `unpack_archive` is incorrect: Expected `str | PathLike[str]`, found `Path | None`
+ pyodide-build/pyodide_build/tests/recipe/test_unvendor.py:54:12: warning[possibly-missing-attribute] Attribute `exists` may be missing on object of type `Path | None`
+ pyodide-build/pyodide_build/tests/recipe/test_unvendor.py:55:12: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Path | None`
+ pyodide-build/pyodide_build/tests/recipe/test_unvendor.py:58:27: error[invalid-argument-type] Argument to function `unpack_archive` is incorrect: Expected `str | PathLike[str]`, found `Path | None`
- pyodide-build/pyodide_build/tests/test_build_env.py:5:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_build_env.py:6:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.config`
- pyodide-build/pyodide_build/tests/test_build_env.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv`
- pyodide-build/pyodide_build/tests/test_cli.py:10:8: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_cli.py:11:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_cli.py:12:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.cli`
- pyodide-build/pyodide_build/tests/test_cli.py:20:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.config`
+ pyodide-build/pyodide_build/tests/test_cli.py:453:25: warning[possibly-missing-attribute] Submodule `build` may not be available as an attribute on module `pyodide_build.cli`
+ pyodide-build/pyodide_build/tests/test_cli.py:454:25: warning[possibly-missing-attribute] Submodule `build` may not be available as an attribute on module `pyodide_build.cli`
+ pyodide-build/pyodide_build/tests/test_cli.py:456:25: warning[possibly-missing-attribute] Submodule `out_of_tree` may not be available as an attribute on module `pyodide_build`
+ pyodide-build/pyodide_build/tests/test_cli.py:520:25: warning[possibly-missing-attribute] Submodule `build` may not be available as an attribute on module `pyodide_build.cli`
+ pyodide-build/pyodide_build/tests/test_cli.py:521:25: warning[possibly-missing-attribute] Submodule `out_of_tree` may not be available as an attribute on module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_cli.py:357:10: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_cli.py:389:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyodide-build/pyodide_build/tests/test_cli.py:422:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyodide-build/pyodide_build/tests/test_cli.py:695:10: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_cli.py:729:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyodide-build/pyodide_build/tests/test_cli.py:761:10: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_cli.py:795:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyodide-build/pyodide_build/tests/test_cli.py:828:10: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_cli.py:862:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyodide-build/pyodide_build/tests/test_cli_xbuildenv.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.cli`
- pyodide-build/pyodide_build/tests/test_cli_xbuildenv.py:13:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/tests/test_cli_xbuildenv.py:14:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv_releases`
- pyodide-build/pyodide_build/tests/test_common.py:6:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/tests/test_config.py:1:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_config.py:2:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.config`
- pyodide-build/pyodide_build/tests/test_config.py:11:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv`
- pyodide-build/pyodide_build/tests/test_f2c_fixes.py:1:6: error[unresolved-import] Cannot resolve imported module `pyodide_build._f2c_fixes`
- pyodide-build/pyodide_build/tests/test_oot_build.py:3:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.out_of_tree`
- pyodide-build/pyodide_build/tests/test_py_compile.py:12:6: error[unresolved-import] Cannot resolve imported module `pyodide_build._py_compile`
- pyodide-build/pyodide_build/tests/test_pypabuild.py:1:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/tests/test_pypabuild.py:2:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.constants`
- pyodide-build/pyodide_build/tests/test_pypi.py:15:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.cli`
- pyodide-build/pyodide_build/tests/test_pypi.py:213:12: error[unresolved-import] Cannot resolve imported module `pyodide_build.out_of_tree.pypi`
- pyodide-build/pyodide_build/tests/test_pywasmcross.py:5:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.pywasmcross`
- pyodide-build/pyodide_build/tests/test_pyzip.py:1:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.pyzip`
- pyodide-build/pyodide_build/tests/test_venv.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.out_of_tree`
- pyodide-build/pyodide_build/tests/test_venv.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv`
- pyodide-build/pyodide_build/tests/test_xbuildenv.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/tests/test_xbuildenv.py:8:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv`
- pyodide-build/pyodide_build/tests/test_xbuildenv_releases.py:7:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv_releases`
+ pyodide-build/pyodide_build/tests/test_xbuildenv_releases.py:86:35: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, CrossBuildEnvReleaseSpec]`, found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
+ pyodide-build/pyodide_build/tests/test_xbuildenv_releases.py:98:35: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, CrossBuildEnvReleaseSpec]`, found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
+ pyodide-build/pyodide_build/tests/test_xbuildenv_releases.py:124:35: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, CrossBuildEnvReleaseSpec]`, found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
+ pyodide-build/pyodide_build/tests/test_xbuildenv_releases.py:133:35: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, CrossBuildEnvReleaseSpec]`, found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
+ pyodide-build/pyodide_build/tests/test_xbuildenv_releases.py:156:35: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, CrossBuildEnvReleaseSpec]`, found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
- pyodide-build/pyodide_build/xbuildenv.py:9:6: error[unresolved-import] Cannot resolve imported module `pyodide_build`
- pyodide-build/pyodide_build/xbuildenv.py:10:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.common`
- pyodide-build/pyodide_build/xbuildenv.py:11:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.create_package_index`
- pyodide-build/pyodide_build/xbuildenv.py:12:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.logger`
- pyodide-build/pyodide_build/xbuildenv.py:13:6: error[unresolved-import] Cannot resolve imported module `pyodide_build.xbuildenv_releases`
- src/py/js.pyi:4:6: error[unresolved-import] Cannot resolve imported module `_pyodide._core_docs`
- src/py/js.pyi:5:6: error[unresolved-import] Cannot resolve imported module `pyodide.ffi`
- src/py/js.pyi:13:6: error[unresolved-import] Cannot resolve imported module `pyodide.webloop`
- src/py/pyodide/_package_loader.py:18:30: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsArray`
- src/py/pyodide/_package_loader.py:18:39: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsBuffer`
- src/py/pyodide/_package_loader.py:18:49: error[unresolved-import] Module `py.pyodide.ffi` has no member `to_js`
- src/py/pyodide/_state.py:6:6: error[unresolved-import] Cannot resolve imported module `_pyodide._importhook`
- src/py/pyodide/_state.py:8:18: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsProxy`
- src/py/pyodide/code.py:6:6: error[unresolved-import] Cannot resolve imported module `_pyodide._base`
- src/py/pyodide/console.py:28:6: error[unresolved-import] Cannot resolve imported module `_pyodide._base`
- src/py/pyodide/console.py:165:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/py/pyodide/console.py:226:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/py/pyodide/console.py:229:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/py/pyodide/ffi/__init__.py:4:8: error[unresolved-import] Cannot resolve imported module `_pyodide._core_docs`
- src/py/pyodide/ffi/__init__.py:5:6: error[unresolved-import] Cannot resolve imported module `_pyodide._core_docs`
- src/py/pyodide/ffi/__init__.py:6:6: error[unresolved-import] Cannot resolve imported module `_pyodide._importhook`
- src/py/pyodide/ffi/wrappers.py:6:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsCallableDoubleProxy`
- src/py/pyodide/ffi/wrappers.py:7:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsDomElement`
- src/py/pyodide/ffi/wrappers.py:8:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsProxy`
- src/py/pyodide/ffi/wrappers.py:9:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsWeakRef`
- src/py/pyodide/ffi/wrappers.py:10:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `create_once_callable`
- src/py/pyodide/ffi/wrappers.py:11:5: error[unresolved-import] Module `py.pyodide.ffi` has no member `create_proxy`
+ src/py/pyodide/ffi/wrappers.py:40:37: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[Any]`?
- src/py/pyodide/http/_exceptions.py:3:19: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsException`
- src/py/pyodide/http/_pyfetch.py:12:31: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsBuffer`
- src/py/pyodide/http/_pyfetch.py:12:41: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsException`
- src/py/pyodide/http/_pyfetch.py:12:54: error[unresolved-import] Module `py.pyodide.ffi` has no member `JsFetchResponse`
- src/py/pyodide/http/_pyfetch.py:12:71: error[unresolved-import] Module `py.pyodide.ffi` has no member `to_js`
- src/py/pyodide/http/pyxhr.py:31:14: error[unresolved-import] Cannot resolve imported module `pyodide.ffi`
- src/py/pyodide/webloop.py:15:30: error[unresolved-import] Module `py.pyodide.ffi` has no member `can_run_sync`
- src/py/pyodide/webloop.py:15:44: error[unresolved-import] Module `py.pyodide.ffi` has no member `create_once_callable`
- src/py/pyodide/webloop.py:15:66: error[unresolved-import] Module `py.pyodide.ffi` has no member `run_sync`
- Found 1001 diagnostics
+ Found 863 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/infra/tests/test_worker_stack.py:4:8: error[unresolved-import] Cannot resolve imported module `..worker.events_stack`
- src/integrations/prefect-aws/infra/tests/test_worker_stack.py:5:8: error[unresolved-import] Cannot resolve imported module `..worker.service_stack`
- src/integrations/prefect-aws/prefect_aws/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
- src/integrations/prefect-aws/prefect_aws/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-aws/prefect_aws/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.client_parameters`
- src/integrations/prefect-aws/prefect_aws/__init__.py:4:7: error[unresolved-import] Cannot resolve imported module `.lambda_function`
- src/integrations/prefect-aws/prefect_aws/__init__.py:5:7: error[unresolved-import] Cannot resolve imported module `.s3`
- src/integrations/prefect-aws/prefect_aws/__init__.py:6:7: error[unresolved-import] Cannot resolve imported module `.secrets_manager`
- src/integrations/prefect-aws/prefect_aws/__init__.py:7:7: error[unresolved-import] Cannot resolve imported module `.workers`
+ src/integrations/prefect-aws/prefect_aws/__init__.py:1:15: error[unresolved-import] Module `prefect_aws` has no member `_version`
- src/integrations/prefect-aws/prefect_aws/_cli/ecs_worker.py:12:7: error[unresolved-import] Cannot resolve imported module `.utils`
- src/integrations/prefect-aws/prefect_aws/_cli/main.py:5:7: error[unresolved-import] Cannot resolve imported module `.ecs_worker`
- src/integrations/prefect-aws/prefect_aws/batch.py:9:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.credentials`
- src/integrations/prefect-aws/prefect_aws/client_parameters.py:10:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.utilities`
- src/integrations/prefect-aws/prefect_aws/client_waiter.py:11:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.credentials`
- src/integrations/prefect-aws/prefect_aws/credentials.py:12:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.client_parameters`
- src/integrations/prefect-aws/prefect_aws/deployments/steps.py:14:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.s3`
- src/integrations/prefect-aws/prefect_aws/experimental/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.decorators`
+ src/integrations/prefect-aws/prefect_aws/deployments/steps.py:100:13: error[unresolved-attribute] Object of type `dict[str, Any]` has no attribute `upload_file`
+ src/integrations/prefect-aws/prefect_aws/deployments/steps.py:156:17: error[unresolved-attribute] Object of type `dict[str, Any]` has no attribute `get_paginator`
+ src/integrations/prefect-aws/prefect_aws/deployments/steps.py:170:13: error[unresolved-attribute] Object of type `dict[str, Any]` has no attribute `download_file`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:15:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.credentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown] | AwsCredentials`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:15:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.credentials`
- src/integrations/prefect-aws/prefect_aws/experimental/decorators.py:13:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.workers`
- src/integrations/prefect-aws/prefect_aws/glue_job.py:12:6: error[unresolved-import] Cannot resolve imported module `prefect_aws`
- src/integrations/prefect-aws/prefect_aws/lambda_function.py:62:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.credentials`
- src/integrations/prefect-aws/prefect_aws/observers/ecs.py:29:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.settings`
- src/integrations/prefect-aws/prefect_aws/s3.py:24:6: error[unresolved-import] Cannot resolve imported module `prefect_aws`
- src/integrations/prefect-aws/prefect_aws/s3.py:25:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.client_parameters`
- src/integrations/prefect-aws/prefect_aws/secrets_manager.py:13:6: error[unresolved-import] Cannot resolve imported module `prefect_aws`
- src/integrations/prefect-aws/prefect_aws/workers/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.ecs_worker`
- src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:85:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.credentials`
- src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:86:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.observers.ecs`
- src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:686:13: warning[possibly-missing-attribute] Attribute `get_client` may be missing on object of type `Unknown | None`
+ src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:686:13: warning[possibly-missing-attribute] Attribute `get_client` may be missing on object of type `AwsCredentials | None`
- src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:709:13: warning[possibly-missing-attribute] Attribute `get_client` may be missing on object of type `Unknown | None`
+ src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:709:13: warning[possibly-missing-attribute] Attribute `get_client` may be missing on object of type `AwsCredentials | None`
- src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:1211:22: warning[possibly-missing-attribute] Attribute `get_client` may be missing on object of type `Unknown | None`
+ src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:1211:22: warning[possibly-missing-attribute] Attribute `get_client` may be missing on object of type `AwsCredentials | None`
- src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:1260:22: warning[possibly-missing-attribute] Attribute `get_client` may be missing on object of type `Unknown | None`
+ src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:1260:22: warning[possibly-missing-attribute] Attribute `get_client` may be missing on object of type `AwsCredentials | None`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:10:6: error[unresolved-import] Cannot resolve imported module `prefect_aws._cli.main`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:11:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.workers`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:407:14: error[unresolved-import] Cannot resolve imported module `prefect_aws._cli.utils`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:422:14: error[unresolved-import] Cannot resolve imported module `prefect_aws._cli.utils`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:439:14: error[unresolved-import] Cannot resolve imported module `prefect_aws._cli.utils`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:475:14: error[unresolved-import] Cannot resolve imported module `prefect_aws._cli.utils`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:514:14: error[unresolved-import] Cannot resolve imported module `prefect_aws._cli.utils`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:561:14: error[unresolved-import] Cannot resolve imported module `prefect_aws._cli.utils`
- src/integrations/prefect-aws/tests/cli/test_ecs_worker.py:582:14: error[unresolved-import] Cannot resolve imported module `prefect_aws._cli.utils`
+ src/integrations/prefect-aws/tests/conftest.py:27:9: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["secret_access_key"]`
- src/integrations/prefect-aws/tests/conftest.py:4:6: error[unresolved-import] Cannot resolve imported module `prefect_aws`
- src/integrations/prefect-aws/tests/conftest.py:5:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.client_parameters`
- src/integrations/prefect-aws/tests/deployments/test_steps.py:9:6: error[unresolved-import] Cannot resolve imported module `prefect_aws`
- src/integrations/prefect-aws/tests/deployments/test_steps.py:10:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.deployments.steps`
+ src/integrations/prefect-aws/tests/deployments/test_steps.py:215:13: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["BlockSecret"]`
+ src/integrations/prefect-aws/tests/deployments/test_steps.py:219:13: error[invalid-argument-type] Argument is incorrect: Expected `AwsClientParameters`, found `dict[Unknown | str, Unknown | str | bool | dict[Unknown | str, Unknown | int]]`
+ src/integrations/prefect-aws/tests/experimental/test_bundles.py:130:13: error[invalid-argument-type] Argument to function `upload_bundle_to_s3` is incorrect: Expected `str`, found `None`
+ src/integrations/prefect-aws/tests/experimental/test_bundles.py:192:9: error[invalid-assignment] Implicit shadowing of function `upload_bundle_to_s3`
+ src/integrations/prefect-aws/tests/experimental/test_bundles.py:243:9: error[invalid-assignment] Implicit shadowing of function `upload_bundle_to_s3`
- src/integrations/prefect-aws/tests/experimental/test_bundles.py:14:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.bundles.execute`
- src/integrations/prefect-aws/tests/experimental/test_bundles.py:18:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.bundles.upload`
- src/integrations/prefect-aws/tests/experimental/test_bundles.py:169:14: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.bundles`
- src/integrations/prefect-aws/tests/experimental/test_bundles.py:220:14: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.bundles`
- src/integrations/prefect-aws/tests/experimental/test_bundles.py:273:14: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.bundles`
- src/integrations/prefect-aws/tests/experimental/test_bundles.py:432:18: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.bundles`
- src/integrations/prefect-aws/tests/experimental/test_bundles.py:449:18: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.bundles`
- src/integrations/prefect-aws/tests/experimental/test_bundles.py:471:14: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.bundles`
- src/integrations/prefect-aws/tests/experimental/test_decorators.py:6:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.experimental.decorators`
- src/integrations/prefect-aws/tests/experimental/test_decorators.py:7:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.workers`
- src/integrations/prefect-aws/tests/observers/test_ecs_observer.py:12:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.observers.ecs`
- src/integrations/prefect-aws/tests/observers/test_ecs_observer.py:26:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.settings`
- src/integrations/prefect-aws/tests/test_batch.py:7:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.batch`
+ src/integrations/prefect-aws/tests/test_batch.py:115:29: error[invalid-argument-type] Argument to function `assert_valid_job_id` is incorrect: Expected `str | None`, found `Unknown | Coroutine[Any, Any, Unknown]`
- src/integrations/prefect-aws/tests/test_client_parameters.py:6:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.client_parameters`
- src/integrations/prefect-aws/tests/test_client_waiter.py:5:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.client_waiter`
- src/integrations/prefect-aws/tests/test_credentials.py:5:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.credentials`
+ src/integrations/prefect-aws/tests/test_credentials.py:32:38: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["root_password"]`
+ src/integrations/prefect-aws/tests/test_credentials.py:43:42: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["root_password"]`
+ src/integrations/prefect-aws/tests/test_credentials.py:59:13: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["root_password"]`
+ src/integrations/prefect-aws/tests/test_credentials.py:129:9: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["root_password"]`
+ src/integrations/prefect-aws/tests/test_credentials.py:186:9: error[invalid-argument-type] Argument is incorrect: Expected `AwsClientParameters`, found `dict[str, dict[str, int | dict[str, int | str]]]`
+ src/integrations/prefect-aws/tests/test_credentials.py:212:9: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["test_password"]`
+ src/integrations/prefect-aws/tests/test_credentials.py:214:9: error[invalid-argument-type] Argument is incorrect: Expected `AwsClientParameters`, found `dict[str, dict[str, str | int | dict[str, int | str]]]`
+ src/integrations/prefect-aws/tests/test_glue_job.py:118:9: error[unknown-argument] Argument `aws_credential` does not match any known parameter
- src/integrations/prefect-aws/tests/test_glue_job.py:5:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.glue_job`
+ src/integrations/prefect-aws/tests/test_glue_job.py:120:5: error[invalid-assignment] Implicit shadowing of function `_get_client`
+ src/integrations/prefect-aws/tests/test_glue_job.py:121:5: error[invalid-assignment] Implicit shadowing of function `_start_job`
- src/integrations/prefect-aws/tests/test_lambda_function.py:11:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.credentials`
- src/integrations/prefect-aws/tests/test_lambda_function.py:12:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.lambda_function`
+ src/integrations/prefect-aws/tests/test_lambda_function.py:230:16: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:231:38: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:257:16: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:258:38: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:265:16: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:266:38: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:268:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["LogResult"]` and `Unknown | Coroutine[Any, Any, Unknown]`
+ src/integrations/prefect-aws/tests/test_lambda_function.py:274:16: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:275:38: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:287:16: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:288:38: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:300:16: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_lambda_function.py:301:38: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_s3.py:482:56: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["testing"]`
+ src/integrations/prefect-aws/tests/test_s3.py:488:39: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["minioadmin"]`
- src/integrations/prefect-aws/tests/test_s3.py:9:6: error[unresolved-import] Cannot resolve imported module `prefect_aws`
- src/integrations/prefect-aws/tests/test_s3.py:10:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.client_parameters`
- src/integrations/prefect-aws/tests/test_s3.py:11:6: error[unresolved-import] Cannot resolve imported module `prefect_aws.s3`
+ src/integrations/prefect-aws/tests/test_s3.py:669:9: error[unknown-argument] Argument `aws_credentials` does not match any known parameter
+ src/integrations/prefect-aws/tests/test_s3.py:670:9: error[unknown-argument] Argument `basepath` does not match any known parameter
+ src/integrations/prefect-aws/tests/test_s3.py:764:54: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["password"]`
+ src/integrations/prefect-aws/tests/test_s3.py:901:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | Coroutine[Any, Any, Unknown]`
+ src/integrations/prefect-aws/tests/test_s3.py:902:16: error[non-subscriptable] Cannot subscript object of type `Coroutine[Any, Any, Unknown]` with no `__getitem__` method
+ src/integrations/prefect-aws/tests/test_s3.py:924:23: error[invalid-assignment] Object of type `Unknown | Coroutine[Any, Any, Unknown]` is not assignable to `bytes`
+ src/integrations/prefect-aws/tests/test_s3.py:1046:17: error[invalid-assignment] Object of type `None` is not assignable to attribute `bucket_folder` of type `str`
+ src/integrations/prefect-aws/tests/test_s3.py:1127:17: error[invalid-assig

... (truncated 2580 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo fields = ~384MB
+     memo fields = ~403MB


```

</details>




---

_Label `ecosystem-analyzer` added by @Gankra on 2025-12-02 01:35_

---

_Comment by @Gankra on 2025-12-02 01:42_

~~Oh my god I think I didn't have to update the implementation of `file_to_module` to loosen the "module_name <-> file_path must be a bijection" check because all the workspace tests have at most two conflicting copies of a name which means one gets to be `resolve_module` and one gets to be `resolve_real_module`~~ 😂 

Actually I think it Just Works because the module lookup takes the importing file, so the fallback mode necessarily agrees with itself.

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 01:45_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 1,749 | 0 | 0 |
| `unresolved-import` | 0 | 644 | 17 |
| `invalid-parameter-default` | 118 | 0 | 10 |
| `unresolved-reference` | 0 | 72 | 0 |
| `possibly-missing-attribute` | 49 | 1 | 14 |
| `non-subscriptable` | 39 | 0 | 0 |
| `unresolved-attribute` | 39 | 0 | 0 |
| `invalid-return-type` | 24 | 0 | 0 |
| `no-matching-overload` | 19 | 0 | 0 |
| `invalid-assignment` | 15 | 0 | 0 |
| `unused-ignore-comment` | 0 | 10 | 0 |
| `unknown-argument` | 9 | 0 | 0 |
| `possibly-missing-import` | 6 | 0 | 0 |
| `invalid-context-manager` | 4 | 0 | 0 |
| `missing-argument` | 3 | 0 | 0 |
| `unsupported-operator` | 3 | 0 | 0 |
| `deprecated` | 2 | 0 | 0 |
| `unsupported-base` | 0 | 2 | 0 |
| `invalid-await` | 1 | 0 | 0 |
| `invalid-type-form` | 1 | 0 | 0 |
| `not-iterable` | 1 | 0 | 0 |
| **Total** | **2,082** | **729** | **41** |

**[Full report with detailed diff](https://gankra-workup.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-workup.ecosystem-663.pages.dev/timing))




---

_Comment by @astral-sh-bot[bot] on 2025-12-02 01:49_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-12-02 14:06_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-12-02 14:06_

---

_Marked ready for review by @Gankra on 2025-12-02 14:07_

---

_Review requested from @carljm by @Gankra on 2025-12-02 14:07_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-02 14:07_

---

_Review requested from @sharkdp by @Gankra on 2025-12-02 14:07_

---

_Review requested from @dcreager by @Gankra on 2025-12-02 14:07_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-02 14:07_

---

_Comment by @Gankra on 2025-12-02 14:07_

This is now ready for review.

---

_Renamed from "[ty] introduce `desperately_resolve_module`" to "[ty] Teach ty the meaning of desperation (try ancestor `pyproject.toml`s as search-paths if module resolution fails)" by @Gankra on 2025-12-02 14:18_

---

_Renamed from "[ty] Teach ty the meaning of desperation (try ancestor `pyproject.toml`s as search-paths if module resolution fails)" to "[ty] Teach `ty` the meaning of desperation (try ancestor `pyproject.toml`s as search-paths if module resolution fails)" by @Gankra on 2025-12-02 14:19_

---

_Comment by @Gankra on 2025-12-02 14:22_

Hard to be 100% confident because there's so Many changes but the ecosystem-analyzer results all look reasonable.

---

_Comment by @codspeed-hq[bot] on 2025-12-02 14:24_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fworkup?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21745 will **not alter performance**

<sub>Comparing <code>gankra/workup</code> (0258d46) with <code>main</code> (0280949)</sub>



### Summary

`✅ 52` untouched  





---

_Comment by @Gankra on 2025-12-02 14:25_

I think the increased memory usage on mypy-primer is just "we successfully resolved more imports and analyzed more code non-trivially"

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:96 on 2025-12-02 17:34_

It might be worth adding a doc level comment explaining the "normal" and "desperate" resolution modes conceptually (what's the idea behind the separation). 



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:257 on 2025-12-02 17:36_

I think we probably should. The only downside of it is that the query would also need to be invalidated whenever a pyproject.toml file is added or deleted (checking all ancestor directories would be easier in that regard).

Given that `file_to_module` depends on `desperately_resolve_module`, I think we have to figure this out anyway. 

The "easiest" but most noisy way to cache this for now that I can think of is to query the file root for `file.path` and read the `FileRoot`'s revision, similar to what we do in auto imports

https://github.com/astral-sh/ruff/blob/7b1e530a6d424c967588ffdffac89b3d5924c88c/crates/ty_python_semantic/src/module_resolver/list.rs#L70-L75

This does mean that the queries gets invalidated every time a file is added, removed, or changed, which is unfortunate. But I don't we have a way of distinguishing between added and removed files and content changes (Unless I'm mistaken, @BurntSushi?)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:92 on 2025-12-02 17:38_

Can we avoid the desperate code path if `importing_file` is not within the project root (I don't think we want to apply this logic to third party libraries)

---

_@MichaReiser approved on 2025-12-02 17:48_

This is great. 

I think we have to figure out the cache invalidation story before landing this change. 

Probably worth a separate PR but I wonder if that allows us to remove `tests` from our `environment.root` defaults

One caveat of this approach is that, I think, completions won't work for any of those modules. This might be surprising to users but is probably fine

---

_@MichaReiser reviewed on 2025-12-02 17:59_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:374 on 2025-12-02 17:59_

I think we also want to use any folder containing a `ty.toml` and we should probably stop at `db.project_root` (and only return `Some` if the path is a sub directory of `db.project.root()`

---

_@MichaReiser reviewed on 2025-12-02 18:00_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:370 on 2025-12-02 18:00_

Can this be `Option<SearchPath>`. We never seem to return more than one (which I think makes sense)

---

_@Gankra reviewed on 2025-12-02 18:09_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:370 on 2025-12-02 18:09_

I wanted the code to be easy to iterate on if we introduce any "keep searching" logic, but yes right now it could be `Option<Searchpath>`. I'll change it to that.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:1640 on 2025-12-02 19:50_

```suggestion
        // here because there isn't really an importing file. However this `resolve_real_module`
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:257 on 2025-12-02 20:05_

Am I understanding right that the problem you are solving with "query file root revision" is "how to invalidate when a `pyproject.toml` (or `ty.toml`) is added or removed"? And you are proposing that for a first cut we just invalidate on any file change?

It seems pretty unfortunate if we have to invalidate all desperate-resolved modules anytime there's any change to any file, considering that a vanishingly small percent of all file changes will actually be "adding or removing a `{pyproject,ty}.toml` file."

Would it be possible to have a query that just returns the set of `{pyproject,ty}.toml` files currently registered in `Files`, and then have the desperate-paths depend on that query? That query would still have to run often, but if its value didn't change, we wouldn't have to re-run any desperate-resolutions. Not sure how the cost of "iterate all files in Files" compares to the cost of re-doing all desperate-resolutions (with the filesystem access that entails.)

---

_@carljm reviewed on 2025-12-02 20:06_

Nice!

---

_@MichaReiser reviewed on 2025-12-02 20:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:257 on 2025-12-02 20:13_

Iterating the entire project is fairly expensive. The only alternative here is to have a query that takes a file and computes the ancestors bit that doesn't seem that much less expensive than just repeating the desperate lookup.

Ultimately, the fix is to male FileRoot distinguish between add/remove and change

---

_Comment by @Gankra on 2025-12-02 20:57_

I'll probably need to ask Micha for help on the caching/clamping behaviour tomorrow (head full, clocking out for the day, maybe won't think it's so daunting when I wake up).

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-12-03 18:30_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-12-03 18:30_

---

_Comment by @Gankra on 2025-12-03 19:20_

Updates:

* tests updated with new explanations and cleaned up cases
* file_to_module now fully reruns with desperate_search_paths for *any* failure (recovers properly in more cases, notably from `/a/src/a/` shenanigans in the extra-paths case)
* desperate search paths are now clamped to be a subdir of the first_party search path, and we now consider the desperate search path a first-party search path (effects at least one diagnostic that checks for that) 
  * seems to have resolved any performance issues on normal code without affecting behaviour on the cases we care about
* ty.toml is also used as a marker

I just need to hammer out a query/caching

---

_Comment by @Gankra on 2025-12-03 19:43_

Made `desperate_search_paths` a query and add the same blunt caching that autocomplete had.

---

_Comment by @Gankra on 2025-12-03 20:04_

I'm going to land this so we can kick the tires on this and unblock dogfooding, if the caching is wonky we can iterate on it.

---

_Merged by @Gankra on 2025-12-03 20:04_

---

_Closed by @Gankra on 2025-12-03 20:04_

---

_Branch deleted on 2025-12-03 20:04_

---

_@carljm reviewed on 2025-12-03 20:42_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:365 on 2025-12-03 20:42_

Ok this is maybe gonna seem unnecessary, but "wop" _is_ an ethnic pejorative (at least in the US), so despite "doo wop" also being a vocal rhythm style, I think it would be safer at the next convenient opportunity to just go with something else here.

---
