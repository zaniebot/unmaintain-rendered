```yaml
number: 21166
title: "[ty] don't assume in diagnostic messages that a TypedDict key error is about subscript access"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: cjm/tddiag
created_at: 2025-10-31T14:29:51Z
updated_at: 2025-10-31T14:50:01Z
url: https://github.com/astral-sh/ruff/pull/21166
synced_at: 2026-01-12T15:57:17Z
```

# [ty] don't assume in diagnostic messages that a TypedDict key error is about subscript access

---

_@carljm_

## Summary

Before this PR, we would emit diagnostics like "Invalid key access" for a TypedDict literal with invalid key, which doesn't make sense since there's no "access" in that case. This PR just adjusts the wording to be more general, and adjusts the documentation of the lint rule too.

I noticed this in the playground and thought it would be a quick fix. As usual, it turned out to be a bit more subtle than I expected, but for now I chose to punt on the complexity. We may ultimately want to have different rules for invalid subscript vs invalid TypedDict literal, because an invalid key in a TypedDict literal is low severity: it's a typo detector, but not actually a type error. But then there's another wrinkle there: if the TypedDict is `closed=True`, then it _is_ a type error. So would we want to separate the open and closed cases into separate rules, too? I decided to leave this as a question for future.

If we wanted to use separate rules, or use specific wording for each case instead of the generalized wording I chose here, that would also involve a bit of extra work to distinguish the cases, since we use a generic set of functions for reporting these errors.

## Test Plan

Added and updated mdtests.


---

_Review requested from @AlexWaygood by @carljm on 2025-10-31 14:29_

---

_Review requested from @sharkdp by @carljm on 2025-10-31 14:29_

---

_Review requested from @dcreager by @carljm on 2025-10-31 14:29_

---

_Label `ty` added by @carljm on 2025-10-31 14:29_

---

_Comment by @github-actions[bot] on 2025-10-31 14:31_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-31 14:39:02.675968186 +0000
+++ new-output.txt	2025-10-31 14:39:05.795980852 +0000
@@ -921,11 +921,11 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:22:17: error[invalid-assignment] Invalid assignment to key "name" with declared type `str` on TypedDict `Movie`: value of type `Literal[1982]`
 typeddicts_operations.py:23:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal[""]`
-typeddicts_operations.py:24:7: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "other"
-typeddicts_operations.py:26:13: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "other"
+typeddicts_operations.py:24:7: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "other"
+typeddicts_operations.py:26:13: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:28:9: error[missing-typed-dict-key] Missing required key 'year' in TypedDict `Movie` constructor
 typeddicts_operations.py:29:42: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `Movie`: value of type `float`
-typeddicts_operations.py:32:36: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "other"
+typeddicts_operations.py:32:36: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:62:1: error[unresolved-attribute] Object of type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
@@ -938,13 +938,13 @@
 typeddicts_readonly_inheritance.py:82:14: error[invalid-assignment] Invalid assignment to key "ident" with declared type `str` on TypedDict `User`: value of type `Literal[3]`
 typeddicts_readonly_inheritance.py:83:15: error[invalid-argument-type] Invalid argument to key "ident" with declared type `str` on TypedDict `User`: value of type `Literal[3]`
 typeddicts_readonly_inheritance.py:84:5: error[missing-typed-dict-key] Missing required key 'ident' in TypedDict `User` constructor
-typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
+typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key for TypedDict `A3`: Unknown key "y"
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_type_consistency.py:126:56: error[invalid-argument-type] Invalid argument to key "inner_key" with declared type `str` on TypedDict `Inner1`: value of type `Literal[1]`
-typeddicts_usage.py:23:7: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "director"
+typeddicts_usage.py:23:7: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "director"
 typeddicts_usage.py:24:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal["1982"]`
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
-typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
+typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
 Found 948 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-31 14:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_config.py:155:41: error[invalid-key] TypedDict `ConfigDict` cannot be indexed with a key of type `str`
+ pydantic/_internal/_config.py:155:41: error[invalid-key] Invalid key for TypedDict `ConfigDict` of type `str`
- pydantic/_internal/_config.py:158:44: error[invalid-key] TypedDict `ConfigDict` cannot be indexed with a key of type `str`
+ pydantic/_internal/_config.py:158:44: error[invalid-key] Invalid key for TypedDict `ConfigDict` of type `str`
- pydantic/deprecated/config.py:22:43: error[invalid-key] TypedDict `ConfigDict` cannot be indexed with a key of type `str`
+ pydantic/deprecated/config.py:22:43: error[invalid-key] Invalid key for TypedDict `ConfigDict` of type `str`
- pydantic/json_schema.py:2125:47: error[invalid-key] Invalid key access on TypedDict `IncExSeqSerSchema`: Unknown key "schema"
+ pydantic/json_schema.py:2125:47: error[invalid-key] Invalid key for TypedDict `IncExSeqSerSchema`: Unknown key "schema"
- pydantic/json_schema.py:2125:47: error[invalid-key] Invalid key access on TypedDict `IncExDictSerSchema`: Unknown key "schema"
+ pydantic/json_schema.py:2125:47: error[invalid-key] Invalid key for TypedDict `IncExDictSerSchema`: Unknown key "schema"
- pydantic/v1/networks.py:232:13: error[invalid-key] Invalid key access on TypedDict `Parts`: Unknown key "host" - did you mean "port"?
+ pydantic/v1/networks.py:232:13: error[invalid-key] Invalid key for TypedDict `Parts`: Unknown key "host" - did you mean "port"?

discord.py (https://github.com/Rapptz/discord.py)
- discord/message.py:2404:30: error[invalid-key] TypedDict `Message` cannot be indexed with a key of type `str`
+ discord/message.py:2404:30: error[invalid-key] Invalid key for TypedDict `Message` of type `str`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/build.py:1347:27: error[invalid-key] TypedDict `StaticLibraryKeywordArguments` cannot be indexed with a key of type `Literal["pic", "pie"]`
+ mesonbuild/build.py:1347:27: error[invalid-key] Invalid key for TypedDict `StaticLibraryKeywordArguments` of type `Literal["pic", "pie"]`
- mesonbuild/build.py:1347:27: error[invalid-key] TypedDict `ExecutableKeywordArguments` cannot be indexed with a key of type `Literal["pic", "pie"]`
+ mesonbuild/build.py:1347:27: error[invalid-key] Invalid key for TypedDict `ExecutableKeywordArguments` of type `Literal["pic", "pie"]`
- mesonbuild/interpreter/interpreter.py:1758:98: error[invalid-key] Invalid key access on TypedDict `FindProgram`: Unknown key "version_argument"
+ mesonbuild/interpreter/interpreter.py:1758:98: error[invalid-key] Invalid key for TypedDict `FindProgram`: Unknown key "version_argument"
- mesonbuild/interpreter/interpreter.py:1818:24: error[invalid-key] TypedDict `Executable` cannot be indexed with a key of type `Unknown | str`
+ mesonbuild/interpreter/interpreter.py:1818:24: error[invalid-key] Invalid key for TypedDict `Executable` of type `Unknown | str`
- mesonbuild/interpreter/interpreter.py:2265:29: error[invalid-key] Invalid key access on TypedDict `BaseTest`: Unknown key "args"
+ mesonbuild/interpreter/interpreter.py:2265:29: error[invalid-key] Invalid key for TypedDict `BaseTest`: Unknown key "args"
- mesonbuild/interpreter/interpreter.py:2270:29: error[invalid-key] Invalid key access on TypedDict `BaseTest`: Unknown key "protocol"
+ mesonbuild/interpreter/interpreter.py:2270:29: error[invalid-key] Invalid key for TypedDict `BaseTest`: Unknown key "protocol"
- mesonbuild/interpreter/interpreter.py:2272:29: error[invalid-key] Invalid key access on TypedDict `BaseTest`: Unknown key "verbose"
+ mesonbuild/interpreter/interpreter.py:2272:29: error[invalid-key] Invalid key for TypedDict `BaseTest`: Unknown key "verbose"
- mesonbuild/interpreter/interpreter.py:2315:19: error[invalid-key] Invalid key access on TypedDict `FuncInstallHeaders`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2315:19: error[invalid-key] Invalid key for TypedDict `FuncInstallHeaders`: Unknown key "preserve_path"
- mesonbuild/interpreter/interpreter.py:2505:23: error[invalid-key] Invalid key access on TypedDict `FuncInstallData`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2505:23: error[invalid-key] Invalid key for TypedDict `FuncInstallData`: Unknown key "preserve_path"
- mesonbuild/interpreter/interpreter.py:2511:90: error[invalid-key] Invalid key access on TypedDict `FuncInstallData`: Unknown key "install_tag" - did you mean "install_dir"?
+ mesonbuild/interpreter/interpreter.py:2511:90: error[invalid-key] Invalid key for TypedDict `FuncInstallData`: Unknown key "install_tag" - did you mean "install_dir"?
- mesonbuild/interpreter/interpreter.py:2512:60: error[invalid-key] Invalid key access on TypedDict `FuncInstallData`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2512:60: error[invalid-key] Invalid key for TypedDict `FuncInstallData`: Unknown key "preserve_path"
- mesonbuild/interpreter/interpreter.py:2579:32: error[invalid-key] Invalid key access on TypedDict `FuncInstallSubdir`: Unknown key "install_tag" - did you mean "install_dir"?
+ mesonbuild/interpreter/interpreter.py:2579:32: error[invalid-key] Invalid key for TypedDict `FuncInstallSubdir`: Unknown key "install_tag" - did you mean "install_dir"?
- mesonbuild/interpreter/interpreter.py:2624:36: error[invalid-key] TypedDict `ConfigureFile` cannot be indexed with a key of type `Unknown | str`
+ mesonbuild/interpreter/interpreter.py:2624:36: error[invalid-key] Invalid key for TypedDict `ConfigureFile` of type `Unknown | str`
- mesonbuild/interpreter/interpreter.py:2747:21: error[invalid-key] Invalid key access on TypedDict `ConfigureFile`: Unknown key "copy"
+ mesonbuild/interpreter/interpreter.py:2747:21: error[invalid-key] Invalid key for TypedDict `ConfigureFile`: Unknown key "copy"
- mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key access on TypedDict `Executable`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key for TypedDict `Executable`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key access on TypedDict `StaticLibrary`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key for TypedDict `StaticLibrary`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key access on TypedDict `SharedLibrary`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key for TypedDict `SharedLibrary`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key access on TypedDict `SharedModule`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key for TypedDict `SharedModule`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key access on TypedDict `Jar`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3396:24: error[invalid-key] Invalid key for TypedDict `Jar`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key access on TypedDict `Executable`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key for TypedDict `Executable`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key access on TypedDict `StaticLibrary`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key for TypedDict `StaticLibrary`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key access on TypedDict `SharedLibrary`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key for TypedDict `SharedLibrary`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key access on TypedDict `SharedModule`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key for TypedDict `SharedModule`: Unknown key "language_args"
- mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key access on TypedDict `Jar`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3401:24: error[invalid-key] Invalid key for TypedDict `Jar`: Unknown key "language_args"
- mesonbuild/modules/python.py:154:20: error[invalid-key] Invalid key access on TypedDict `ExtensionModuleKw`: Unknown key "install_dir"
+ mesonbuild/modules/python.py:154:20: error[invalid-key] Invalid key for TypedDict `ExtensionModuleKw`: Unknown key "install_dir"
- mesonbuild/modules/python.py:169:42: error[invalid-key] Invalid key access on TypedDict `ExtensionModuleKw`: Unknown key "limited_api"
+ mesonbuild/modules/python.py:169:42: error[invalid-key] Invalid key for TypedDict `ExtensionModuleKw`: Unknown key "limited_api"
- mesonbuild/modules/python.py:215:24: error[invalid-key] Invalid key access on TypedDict `ExtensionModuleKw`: Unknown key "link_args"
+ mesonbuild/modules/python.py:215:24: error[invalid-key] Invalid key for TypedDict `ExtensionModuleKw`: Unknown key "link_args"
- mesonbuild/modules/python.py:217:16: error[invalid-key] Invalid key access on TypedDict `ExtensionModuleKw`: Unknown key "dependencies"
+ mesonbuild/modules/python.py:217:16: error[invalid-key] Invalid key for TypedDict `ExtensionModuleKw`: Unknown key "dependencies"
- mesonbuild/modules/python.py:262:24: error[invalid-key] Invalid key access on TypedDict `DependencyObjectKWs`: Unknown key "build_config"
+ mesonbuild/modules/python.py:262:24: error[invalid-key] Invalid key for TypedDict `DependencyObjectKWs`: Unknown key "build_config"
- mesonbuild/modules/python.py:309:34: error[invalid-key] Invalid key access on TypedDict `PyInstallKw`: Unknown key "preserve_path"
+ mesonbuild/modules/python.py:309:34: error[invalid-key] Invalid key for TypedDict `PyInstallKw`: Unknown key "preserve_path"
- unittests/cargotests.py:377:68: error[invalid-key] Invalid key access on TypedDict `FromWorkspace`: Unknown key "optional"
+ unittests/cargotests.py:377:68: error[invalid-key] Invalid key for TypedDict `FromWorkspace`: Unknown key "optional"
- unittests/cargotests.py:385:62: error[invalid-key] Invalid key access on TypedDict `FromWorkspace`: Unknown key "features"
+ unittests/cargotests.py:385:62: error[invalid-key] Invalid key for TypedDict `FromWorkspace`: Unknown key "features"

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/DataSourceVMware.py:1000:48: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1000:48: error[invalid-key] Invalid key for TypedDict `Interface` of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1001:26: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1001:26: error[invalid-key] Invalid key for TypedDict `Interface` of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1008:48: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1008:48: error[invalid-key] Invalid key for TypedDict `Interface` of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1009:26: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1009:26: error[invalid-key] Invalid key for TypedDict `Interface` of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1018:53: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1018:53: error[invalid-key] Invalid key for TypedDict `Interface` of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1028:53: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1028:53: error[invalid-key] Invalid key for TypedDict `Interface` of type `Literal[0]`
- tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:24:5: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "name"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:24:5: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "name"
- tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:25:5: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "title"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:25:5: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "title"
- tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:26:5: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "description"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:26:5: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "description"
- tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:30:5: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "examples"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:30:5: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "examples"
- tests/unittests/config/test_modules.py:73:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "name"
+ tests/unittests/config/test_modules.py:73:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "name"
- tests/unittests/config/test_modules.py:75:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "title"
+ tests/unittests/config/test_modules.py:75:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "title"
- tests/unittests/config/test_modules.py:76:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "description"
+ tests/unittests/config/test_modules.py:76:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "description"
- tests/unittests/config/test_modules.py:78:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "examples"
+ tests/unittests/config/test_modules.py:78:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "examples"
- tests/unittests/config/test_modules.py:113:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "name"
+ tests/unittests/config/test_modules.py:113:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "name"
- tests/unittests/config/test_modules.py:115:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "title"
+ tests/unittests/config/test_modules.py:115:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "title"
- tests/unittests/config/test_modules.py:116:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "description"
+ tests/unittests/config/test_modules.py:116:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "description"
- tests/unittests/config/test_modules.py:118:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "examples"
+ tests/unittests/config/test_modules.py:118:13: error[invalid-key] Invalid key for TypedDict `MetaSchema`: Unknown key "examples"

rclip (https://github.com/yurijmikhalevich/rclip)
- rclip/main.py:40:13: error[invalid-key] TypedDict `ImageMeta` cannot be indexed with a key of type `str`
+ rclip/main.py:40:13: error[invalid-key] Invalid key for TypedDict `ImageMeta` of type `str`
- rclip/main.py:40:27: error[invalid-key] TypedDict `Image` cannot be indexed with a key of type `str`
+ rclip/main.py:40:27: error[invalid-key] Invalid key for TypedDict `Image` of type `str`

xarray (https://github.com/pydata/xarray)
- xarray/core/options.py:332:35: error[invalid-key] TypedDict `T_Options` cannot be indexed with a key of type `str`
+ xarray/core/options.py:332:35: error[invalid-key] Invalid key for TypedDict `T_Options` of type `str`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/worksearch/code.py:624:33: error[invalid-key] TypedDict `SolrDocument` cannot be indexed with a key of type `str`
+ openlibrary/plugins/worksearch/code.py:624:33: error[invalid-key] Invalid key for TypedDict `SolrDocument` of type `str`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/llmobs/_llmobs.py:1774:43: error[invalid-key] Invalid key access on TypedDict `LLMObsEvaluationMetricEvent`: Unknown key "metadata"
+ ddtrace/llmobs/_llmobs.py:1774:43: error[invalid-key] Invalid key for TypedDict `LLMObsEvaluationMetricEvent`: Unknown key "metadata"

altair (https://github.com/vega/altair)
- tests/vegalite/v6/test_api.py:558:17: error[invalid-key] Invalid key access on TypedDict `_Value`: Unknown key "condition"
+ tests/vegalite/v6/test_api.py:558:17: error[invalid-key] Invalid key for TypedDict `_Value`: Unknown key "condition"
- tests/vegalite/v6/test_api.py:604:32: error[invalid-key] Invalid key access on TypedDict `_Value`: Unknown key "condition"
+ tests/vegalite/v6/test_api.py:604:32: error[invalid-key] Invalid key for TypedDict `_Value`: Unknown key "condition"
- tests/vegalite/v6/test_theme.py:453:17: error[invalid-key] Invalid key access on TypedDict `StyleConfigIndexKwds`: Unknown key "group-title"
+ tests/vegalite/v6/test_theme.py:453:17: error[invalid-key] Invalid key for TypedDict `StyleConfigIndexKwds`: Unknown key "group-title"
- tests/vegalite/v6/test_theme.py:454:17: error[invalid-key] Invalid key access on TypedDict `StyleConfigIndexKwds`: Unknown key "guide-label"
+ tests/vegalite/v6/test_theme.py:454:17: error[invalid-key] Invalid key for TypedDict `StyleConfigIndexKwds`: Unknown key "guide-label"
- tests/vegalite/v6/test_theme.py:455:17: error[invalid-key] Invalid key access on TypedDict `StyleConfigIndexKwds`: Unknown key "guide-title"
+ tests/vegalite/v6/test_theme.py:455:17: error[invalid-key] Invalid key for TypedDict `StyleConfigIndexKwds`: Unknown key "guide-title"

core (https://github.com/home-assistant/core)
- homeassistant/components/cloud/alexa_config.py:543:28: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/components/cloud/alexa_config.py:543:28: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/components/cloud/alexa_config.py:547:45: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "old_entity_id"
+ homeassistant/components/cloud/alexa_config.py:547:45: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "old_entity_id"
- homeassistant/components/cloud/google_config.py:471:28: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/components/cloud/google_config.py:471:28: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/components/cloud/google_config.py:495:76: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
+ homeassistant/components/cloud/google_config.py:495:76: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
- homeassistant/components/cloud/google_config.py:495:76: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
+ homeassistant/components/cloud/google_config.py:495:76: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
- homeassistant/components/conversation/chat_log.py:424:40: error[invalid-key] Invalid key access on TypedDict `AssistantContentDeltaDict`: Unknown key "tool_call_id" - did you mean "tool_calls"?
+ homeassistant/components/conversation/chat_log.py:424:40: error[invalid-key] Invalid key for TypedDict `AssistantContentDeltaDict`: Unknown key "tool_call_id" - did you mean "tool_calls"?
- homeassistant/components/conversation/chat_log.py:425:37: error[invalid-key] Invalid key access on TypedDict `AssistantContentDeltaDict`: Unknown key "tool_name"
+ homeassistant/components/conversation/chat_log.py:425:37: error[invalid-key] Invalid key for TypedDict `AssistantContentDeltaDict`: Unknown key "tool_name"
- homeassistant/components/conversation/chat_log.py:426:39: error[invalid-key] Invalid key access on TypedDict `AssistantContentDeltaDict`: Unknown key "tool_result"
+ homeassistant/components/conversation/chat_log.py:426:39: error[invalid-key] Invalid key for TypedDict `AssistantContentDeltaDict`: Unknown key "tool_result"
- homeassistant/components/generic_hygrostat/__init__.py:136:36: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/components/generic_hygrostat/__init__.py:136:36: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/components/generic_thermostat/__init__.py:63:36: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/components/generic_thermostat/__init__.py:63:36: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/components/mold_indicator/__init__.py:75:44: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/components/mold_indicator/__init__.py:75:44: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/components/mqtt/entity.py:1733:47: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
+ homeassistant/components/mqtt/entity.py:1733:47: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
- homeassistant/components/mqtt/entity.py:1733:47: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
+ homeassistant/components/mqtt/entity.py:1733:47: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
- homeassistant/components/prometheus/__init__.py:291:34: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/components/prometheus/__init__.py:291:34: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/components/proximity/coordinator.py:125:63: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/components/proximity/coordinator.py:125:63: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/components/proximity/coordinator.py:126:42: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "old_entity_id"
+ homeassistant/components/proximity/coordinator.py:126:42: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "old_entity_id"
- homeassistant/components/recorder/entity_registry.py:29:36: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "old_entity_id"
+ homeassistant/components/recorder/entity_registry.py:29:36: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "old_entity_id"
- homeassistant/components/tts/__init__.py:292:62: error[invalid-key] Invalid key access on TypedDict `ParsedMediaSourceStreamId`: Unknown key "options"
+ homeassistant/components/tts/__init__.py:292:62: error[invalid-key] Invalid key for TypedDict `ParsedMediaSourceStreamId`: Unknown key "options"
- homeassistant/components/tts/__init__.py:293:41: error[invalid-key] Invalid key access on TypedDict `ParsedMediaSourceStreamId`: Unknown key "message"
+ homeassistant/components/tts/__init__.py:293:41: error[invalid-key] Invalid key for TypedDict `ParsedMediaSourceStreamId`: Unknown key "message"
- homeassistant/components/tts/media_source.py:145:70: error[invalid-key] Invalid key access on TypedDict `ParsedMediaSourceStreamId`: Unknown key "options"
+ homeassistant/components/tts/media_source.py:145:70: error[invalid-key] Invalid key for TypedDict `ParsedMediaSourceStreamId`: Unknown key "options"
- homeassistant/components/tts/media_source.py:146:49: error[invalid-key] Invalid key access on TypedDict `ParsedMediaSourceStreamId`: Unknown key "message"
+ homeassistant/components/tts/media_source.py:146:49: error[invalid-key] Invalid key for TypedDict `ParsedMediaSourceStreamId`: Unknown key "message"
- homeassistant/helpers/entity.py:1529:32: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/helpers/entity.py:1529:32: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/helpers/entity.py:1578:31: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
+ homeassistant/helpers/entity.py:1578:31: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
- homeassistant/helpers/entity.py:1578:31: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
+ homeassistant/helpers/entity.py:1578:31: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
- homeassistant/helpers/entity.py:1578:73: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
+ homeassistant/helpers/entity.py:1578:73: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
- homeassistant/helpers/entity.py:1578:73: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
+ homeassistant/helpers/entity.py:1578:73: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
- homeassistant/helpers/entity_registry.py:1149:46: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "device" - did you mean "device_id"?
+ homeassistant/helpers/entity_registry.py:1149:46: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "device" - did you mean "device_id"?
- homeassistant/helpers/entity_registry.py:1149:46: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Update`: Unknown key "device" - did you mean "device_id"?
+ homeassistant/helpers/entity_registry.py:1149:46: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Update`: Unknown key "device" - did you mean "device_id"?
- homeassistant/helpers/entity_registry.py:1179:45: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
+ homeassistant/helpers/entity_registry.py:1179:45: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
- homeassistant/helpers/entity_registry.py:1179:45: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
+ homeassistant/helpers/entity_registry.py:1179:45: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
- homeassistant/helpers/entity_registry.py:1193:56: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
+ homeassistant/helpers/entity_registry.py:1193:56: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Create`: Unknown key "changes"
- homeassistant/helpers/entity_registry.py:1193:56: error[invalid-key] Invalid key access on TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
+ homeassistant/helpers/entity_registry.py:1193:56: error[invalid-key] Invalid key for TypedDict `_EventDeviceRegistryUpdatedData_Remove`: Unknown key "changes"
- homeassistant/helpers/entity_registry.py:1912:40: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "old_entity_id"
+ homeassistant/helpers/entity_registry.py:1912:40: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "old_entity_id"
- homeassistant/helpers/helper_integration.py:73:32: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/helpers/helper_integration.py:73:32: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/helpers/helper_integration.py:81:60: error[invalid-key] Invalid key access on TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/helpers/helper_integration.py:81:60: error[invalid-key] Invalid key for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"

```
</details>
No memory usage changes detected ✅


---

_Review requested from @MichaReiser by @carljm on 2025-10-31 14:37_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-31 14:39_

---

_@AlexWaygood approved on 2025-10-31 14:46_

---

_Merged by @carljm on 2025-10-31 14:49_

---

_Closed by @carljm on 2025-10-31 14:49_

---

_Branch deleted on 2025-10-31 14:50_

---
