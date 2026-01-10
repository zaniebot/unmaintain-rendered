```yaml
number: 21467
title: "[ty] implement `TypedDict` structural assignment"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: jack/typedict_assignment
created_at: 2025-11-15T01:04:45Z
updated_at: 2025-11-20T21:15:30Z
url: https://github.com/astral-sh/ruff/pull/21467
synced_at: 2026-01-10T16:48:02Z
```

# [ty] implement `TypedDict` structural assignment

---

_Pull request opened by @oconnor663 on 2025-11-15 01:04_

Closes https://github.com/astral-sh/ty/issues/1387.

---

_Review requested from @carljm by @oconnor663 on 2025-11-15 01:04_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-11-15 01:04_

---

_Review requested from @sharkdp by @oconnor663 on 2025-11-15 01:04_

---

_Review requested from @dcreager by @oconnor663 on 2025-11-15 01:04_

---

_@oconnor663 reviewed on 2025-11-15 01:05_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:313 on 2025-11-15 01:05_

Is this the right way to do these tests? Do I need to `.normalize()` the `str` class?

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 01:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-20 20:55:37.641020926 +0000
+++ new-output.txt	2025-11-20 20:55:41.165048397 +0000
@@ -954,11 +954,15 @@
 typeddicts_extra_items.py:285:43: error[invalid-key] Unknown key "language" for TypedDict `ExtraMovie`: Unknown key "language"
 typeddicts_extra_items.py:293:44: error[invalid-key] Unknown key "year" for TypedDict `ClosedMovie`: Unknown key "year"
 typeddicts_extra_items.py:299:54: error[invalid-key] Unknown key "summary" for TypedDict `MovieExtraStr`: Unknown key "summary"
+typeddicts_extra_items.py:300:34: error[invalid-assignment] Object of type `MovieExtraStr` is not assignable to `Mapping[str, str]`
 typeddicts_extra_items.py:302:54: error[invalid-key] Unknown key "year" for TypedDict `MovieExtraInt`: Unknown key "year"
+typeddicts_extra_items.py:303:34: error[invalid-assignment] Object of type `MovieExtraInt` is not assignable to `Mapping[str, int]`
+typeddicts_extra_items.py:304:44: error[invalid-assignment] Object of type `MovieExtraInt` is not assignable to `Mapping[str, int | str]`
 typeddicts_extra_items.py:310:5: error[type-assertion-failure] Type `list[tuple[str, int | str]]` does not match asserted type `list[tuple[str, object]]`
 typeddicts_extra_items.py:311:5: error[type-assertion-failure] Type `list[int | str]` does not match asserted type `list[object]`
-typeddicts_extra_items.py:327:5: error[unresolved-attribute] Object of type `IntDict` has no attribute `clear`
+typeddicts_extra_items.py:326:25: error[invalid-assignment] Object of type `IntDict` is not assignable to `dict[str, int]`
 typeddicts_extra_items.py:329:52: error[invalid-key] Unknown key "bar" for TypedDict `IntDictWithNum` - did you mean "num"?
+typeddicts_extra_items.py:330:32: error[invalid-assignment] Object of type `IntDictWithNum` is not assignable to `dict[str, int]`
 typeddicts_extra_items.py:337:1: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `clear`
 typeddicts_extra_items.py:339:1: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `Unknown`
 typeddicts_extra_items.py:339:13: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `popitem`
@@ -980,12 +984,26 @@
 typeddicts_readonly.py:51:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie1`: key is marked read-only
 typeddicts_readonly.py:60:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie2`: key is marked read-only
 typeddicts_readonly.py:61:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie2`: key is marked read-only
+typeddicts_readonly_consistency.py:37:14: error[invalid-assignment] Object of type `A1` is not assignable to `B1`
+typeddicts_readonly_consistency.py:38:14: error[invalid-assignment] Object of type `C1` is not assignable to `B1`
+typeddicts_readonly_consistency.py:40:14: error[invalid-assignment] Object of type `A1` is not assignable to `C1`
+typeddicts_readonly_consistency.py:81:14: error[invalid-assignment] Object of type `A2` is not assignable to `B2`
+typeddicts_readonly_consistency.py:82:14: error[invalid-assignment] Object of type `C2` is not assignable to `B2`
+typeddicts_readonly_consistency.py:84:14: error[invalid-assignment] Object of type `A2` is not assignable to `C2`
+typeddicts_readonly_consistency.py:85:14: error[invalid-assignment] Object of type `B2` is not assignable to `C2`
 typeddicts_readonly_inheritance.py:36:4: error[invalid-assignment] Cannot assign to key "name" on TypedDict `Album2`: key is marked read-only
 typeddicts_readonly_inheritance.py:65:19: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
 typeddicts_readonly_inheritance.py:82:14: error[invalid-assignment] Invalid assignment to key "ident" with declared type `str` on TypedDict `User`: value of type `Literal[3]`
 typeddicts_readonly_inheritance.py:83:15: error[invalid-argument-type] Invalid argument to key "ident" with declared type `str` on TypedDict `User`: value of type `Literal[3]`
 typeddicts_readonly_inheritance.py:84:5: error[missing-typed-dict-key] Missing required key 'ident' in TypedDict `User` constructor
+typeddicts_type_consistency.py:21:10: error[invalid-assignment] Object of type `B1` is not assignable to `A1`
+typeddicts_type_consistency.py:38:10: error[invalid-assignment] Object of type `B2` is not assignable to `A2`
+typeddicts_type_consistency.py:65:6: error[invalid-assignment] Object of type `A3` is not assignable to `B3`
 typeddicts_type_consistency.py:69:21: error[invalid-key] Unknown key "y" for TypedDict `A3`: Unknown key "y"
+typeddicts_type_consistency.py:76:22: error[invalid-assignment] Object of type `B3` is not assignable to `dict[str, int]`
+typeddicts_type_consistency.py:77:25: error[invalid-assignment] Object of type `B3` is not assignable to `dict[str, object]`
+typeddicts_type_consistency.py:78:22: error[invalid-assignment] Object of type `B3` is not assignable to `dict[Any, Any]`
+typeddicts_type_consistency.py:82:25: error[invalid-assignment] Object of type `B3` is not assignable to `Mapping[str, int]`
 typeddicts_type_consistency.py:101:14: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_type_consistency.py:126:56: error[invalid-argument-type] Invalid argument to key "inner_key" with declared type `str` on TypedDict `Inner1`: value of type `Literal[1]`
 typeddicts_usage.py:23:7: error[invalid-key] Unknown key "director" for TypedDict `Movie`: Unknown key "director"
@@ -993,5 +1011,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 995 diagnostics
+Found 1013 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_@oconnor663 reviewed on 2025-11-15 01:07_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/subscript/assignment_diagnostics.md`:98 on 2025-11-15 01:07_

This test wants to look at the diagnostics we print for `TypedDict`s in a union, but previously `Person` was a subtype of `Animal`, so this PR made the union disappear. Adding `phone_number` breaks the subtyping relationship, so the union remains.

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 01:09_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/connection.py:1301:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 25 diagnostics
+ Found 26 diagnostics

kornia (https://github.com/kornia/kornia)
+ kornia/feature/adalam/core.py:399:96: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 766 diagnostics
+ Found 767 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ testing/typing_checks.py:47:25: error[invalid-argument-type] Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["x"], Literal[2]]`, found `Foo`
+ testing/typing_checks.py:48:25: error[invalid-argument-type] Argument to bound method `delitem` is incorrect: Expected `Mapping[Literal["y"], Unknown]`, found `Foo`
- Found 445 diagnostics
+ Found 447 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/utils/authenticator.py:224:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 978 diagnostics
+ Found 979 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/rpc/telegram.py:445:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ freqtrade/rpc/telegram.py:543:46: error[invalid-argument-type] Argument to bound method `_format_entry_msg` is incorrect: Expected `RPCEntryMsg`, found `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`
+ freqtrade/rpc/telegram.py:546:45: error[invalid-argument-type] Argument to bound method `_format_exit_msg` is incorrect: Expected `RPCExitMsg`, found `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`
+ freqtrade/rpc/telegram.py:553:62: error[invalid-argument-type] Argument to bound method `_exchange_from_msg` is incorrect: Expected `RPCEntryMsg | RPCExitMsg | RPCExitCancelMsg | RPCCancelMsg`, found `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`
- Found 610 diagnostics
+ Found 612 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/build.py:1274:42: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["link_args"], Unknown]`, found `BuildTargetKeywordArguments`
+ mesonbuild/build.py:1288:35: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["include_directories"], Unknown]`, found `BuildTargetKeywordArguments`
+ mesonbuild/build.py:1291:35: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["dependencies"], Unknown]`, found `BuildTargetKeywordArguments`
+ mesonbuild/build.py:2645:40: error[invalid-argument-type] Argument to bound method `process_vs_module_defs_kw` is incorrect: Expected `ExecutableKeywordArguments`, found `SharedLibraryKeywordArguments`
+ mesonbuild/interpreter/interpreter.py:1303:87: error[invalid-argument-type] Argument to function `machine_from_native_kwarg` is incorrect: Expected `dict[str, Any]`, found `FuncAddLanguages`
- mesonbuild/interpreter/interpreter.py:1874:16: error[invalid-return-type] Return type does not match returned value: expected `SharedModule`, found `SharedLibrary`
+ mesonbuild/interpreter/interpreter.py:1905:20: error[no-matching-overload] No overload of bound method `build_target` matches arguments
+ mesonbuild/interpreter/interpreter.py:1916:16: error[no-matching-overload] No overload of bound method `build_target` matches arguments
+ mesonbuild/interpreter/interpreter.py:2215:35: error[invalid-argument-type] Argument to bound method `add_test` is incorrect: Expected `dict[str, Any]`, found `FuncBenchmark`
+ mesonbuild/interpreter/interpreter.py:2222:35: error[invalid-argument-type] Argument to bound method `add_test` is incorrect: Expected `dict[str, Any]`, found `FuncTest`
+ mesonbuild/interpreter/interpreter.py:2253:37: error[invalid-argument-type] Argument to bound method `unpack_env_kwarg` is incorrect: Expected `EnvironmentVariables | dict[str, @Todo] | list[@Todo] | str`, found `BaseTest`
- mesonbuild/interpreter/interpreter.py:3452:16: error[invalid-key] Unknown key "dependencies" for TypedDict `SharedModule`: Unknown key "dependencies"
+ mesonbuild/interpreter/interpreter.py:3452:50: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["dependencies"], Unknown]`, found `Executable | StaticLibrary | SharedLibrary | Jar`
- mesonbuild/interpreter/interpreter.py:3454:33: error[invalid-assignment] Invalid assignment to key "extra_files" with declared type `list[File | str]` on TypedDict `SharedModule`: value of type `list[File]`
+ mesonbuild/interpreter/interpreter.py:3456:38: error[invalid-argument-type] Argument to bound method `__process_language_args` is incorrect: Expected `dict[str, list[File | str]]`, found `Executable | StaticLibrary | SharedLibrary | Jar`
- mesonbuild/interpreter/interpreter.py:3484:39: error[invalid-assignment] Invalid assignment to key "d_import_dirs" with declared type `list[str | IncludeDirs]` on TypedDict `SharedModule`: value of type `list[IncludeDirs]`
- mesonbuild/interpreter/mesonmain.py:385:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ mesonbuild/modules/python.py:158:45: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["dependencies"], Unknown]`, found `ExtensionModuleKw`
+ mesonbuild/modules/python.py:178:51: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["c_args"], Unknown]`, found `ExtensionModuleKw`
+ mesonbuild/modules/python.py:182:53: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["cpp_args"], Unknown]`, found `ExtensionModuleKw`
+ mesonbuild/modules/python.py:207:58: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[Literal["link_args"], Unknown]`, found `ExtensionModuleKw`
- mesonbuild/modules/python.py:231:16: error[invalid-return-type] Return type does not match returned value: expected `SharedModule`, found `Unknown | SharedLibrary`
+ mesonbuild/modules/simd.py:103:54: error[invalid-argument-type] Argument to function `extract_as_list` is incorrect: Expected `dict[str, Unknown]`, found `StaticLibrary`
- Found 1722 diagnostics
+ Found 1733 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/audit_logs.py:414:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/channel.py:561:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/channel.py:1654:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/channel.py:2010:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/channel.py:2169:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/channel.py:2815:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/channel.py:3234:57: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `PartialUser` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
+ discord/channel.py:3409:63: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `PartialUser` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
+ discord/components.py:1326:27: error[invalid-argument-type] Invalid argument to key "components" with declared type `list[ActionRow | TextComponent | MediaGalleryComponent | ... omitted 5 union elements]` on TypedDict `ContainerComponent`: value of type `list[ButtonComponent | SelectMenu | TextInput | ... omitted 11 union elements]`
- discord/components.py:1382:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/components.py:1464:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/components.py:1458:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ActionRow`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1460:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ButtonComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1462:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TextInput`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1466:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SectionComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1468:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TextComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1470:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ThumbnailComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1472:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MediaGalleryComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1474:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `FileComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1476:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SeparatorComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1478:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ContainerComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1480:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `LabelComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
+ discord/components.py:1482:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `FileUploadComponent`, found `ButtonComponent | SelectMenu | TextInput | ... omitted 10 union elements`
- discord/guild.py:653:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/guild.py:1961:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ForumChannel | MediaChannel`, found `TextChannel | NewsChannel | VoiceChannel | ... omitted 5 union elements`
- discord/interactions.py:243:106: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/interactions.py:256:92: error[invalid-argument-type] Argument to bound method `format_map` is incorrect: Expected `_FormatMapMapping`, found `TextChannel | NewsChannel | VoiceChannel | ... omitted 7 union elements`
+ discord/member.py:319:45: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `User` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
- discord/message.py:535:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/message.py:791:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/message.py:2455:46: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `User` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
+ discord/message.py:2481:47: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `UserWithMember` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
- discord/onboarding.py:280:95: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/scheduled_event.py:150:63: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `User & ~AlwaysFalsy` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
- discord/state.py:676:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/state.py:912:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/state.py:1158:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/state.py:1121:32: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `User` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
+ discord/state.py:1388:36: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `User` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
- discord/state.py:1975:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/sticker.py:420:60: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `User & ~AlwaysFalsy` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
- discord/ui/view.py:1064:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/webhook/async_.py:751:44: error[invalid-argument-type] Argument to function `store_user` is incorrect: Argument type `User | PartialUser` does not satisfy upper bound `ConnectionState[ClientT@ConnectionState]` of type variable `Self`
- Found 478 diagnostics
+ Found 485 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/settings/profiles.py:103:56: error[invalid-argument-type] Argument to bound method `from_exception_data` is incorrect: Expected `list[InitErrorDetails]`, found `list[Unknown | ErrorDetails]`
- Found 3239 diagnostics
+ Found 3240 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/core/lists/model.py:431:13: error[invalid-assignment] Object of type `(Thing & ~str & ~Top[dict[Unknown, Unknown]]) | (AnnotatedSeed & ~str & ~Top[dict[Unknown, Unknown]])` is not assignable to attribute `value` of type `Thing | str`
+ openlibrary/core/lists/model.py:474:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Thing | str | AnnotatedSeed`, found `str | ThingReferenceDict | AnnotatedSeedDict`
- Found 944 diagnostics
+ Found 946 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_utils.py:257:22: error[invalid-key] TypedDict `AllConvert` can only be subscripted with a string literal key, got key of type `str`.
- Found 543 diagnostics
+ Found 544 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/cwlprov/ro.py:357:21: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 151 diagnostics
+ Found 152 diagnostics

altair (https://github.com/vega/altair)
- altair/vegalite/v6/data.py:27:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ altair/vegalite/v6/data.py:25:36: error[invalid-argument-type] Argument to bound method `register` is incorrect: Expected `((...) -> typing.TypeVar) | None`, found `Overload[(data: None = ellipsis, prefix: str = ellipsis, extension: str = ellipsis, filename: str = ellipsis, urlpath: str = ellipsis) -> partial[Unknown], (data: dict[Any, Any] | @Todo | SupportsGeoInterface | DataFrameLike, prefix: str = ellipsis, extension: str = ellipsis, filename: str = ellipsis, urlpath: str = ellipsis) -> _ToFormatReturnUrlDict]`
- tests/vegalite/v6/test_api.py:564:17: error[invalid-key] Unknown key "condition" for TypedDict `_Value`: Unknown key "condition"
- tests/vegalite/v6/test_api.py:610:32: error[invalid-key] Unknown key "condition" for TypedDict `_Value`: Unknown key "condition"
- Found 992 diagnostics
+ Found 990 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_timefuncs.py:1352:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `Timestamp`
- tests/test_timefuncs.py:1353:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `Timestamp`
- Found 5966 diagnostics
+ Found 5964 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/tests/test_user_status.py:23:12: error[invalid-return-type] Return type does not match returned value: expected `UserInfoDict`, found `UserInfoDict | None`
+ zerver/views/auth.py:362:30: error[no-matching-overload] No overload of bound method `__init__` matches arguments

core (https://github.com/home-assistant/core)
- homeassistant/components/auth/login_flow.py:226:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/components/auth/mfa_setup_flow.py:157:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ homeassistant/components/auth/mfa_setup_flow.py:155:16: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ homeassistant/components/conversation/default_agent.py:274:33: error[invalid-key] Unknown key "changes" for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/components/energy/data.py:367:29: error[invalid-assignment] Invalid assignment to key "energy_sources" with declared type `list[SourceType]` on TypedDict `EnergyPreferences`: value of type `list[SourceType] | list[DeviceConsumption]`
+ homeassistant/components/energy/data.py:367:29: error[invalid-assignment] Invalid assignment to key "device_consumption" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[SourceType] | list[DeviceConsumption]`
- homeassistant/components/evohome/storage.py:85:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ homeassistant/components/fronius/config_flow.py:119:53: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ homeassistant/components/geofency/device_tracker.py:71:9: error[invalid-assignment] Object of type `DeviceInfo` is not assignable to attribute `_attr_device_info` of type `None`
+ homeassistant/components/gpslogger/device_tracker.py:83:9: error[invalid-assignment] Object of type `DeviceInfo` is not assignable to attribute `_attr_device_info` of type `None`
- homeassistant/components/homekit/config_flow.py:502:25: error[invalid-assignment] Object of type `Any` is not assignable to `EntityFilterDict`
- homeassistant/components/homekit/config_flow.py:546:43: error[invalid-assignment] Object of type `Any` is not assignable to `EntityFilterDict`
- homeassistant/components/homekit/config_flow.py:649:43: error[invalid-assignment] Object of type `Any` is not assignable to `EntityFilterDict`
+ homeassistant/components/launch_library/__init__.py:56:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Awaitable[dict[str, Any]]) | None`, found `def async_update() -> CoroutineType[Any, Any, LaunchLibraryData]`
+ homeassistant/components/mqtt/config_flow.py:3376:65: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ homeassistant/components/mqtt/config_flow.py:4589:13: error[no-matching-overload] No overload of bound method `update` matches arguments
+ homeassistant/components/mqtt/config_flow.py:4590:13: error[no-matching-overload] No overload of bound method `update` matches arguments
+ homeassistant/components/mqtt/config_flow.py:4626:9: error[no-matching-overload] No overload of bound method `update` matches arguments
+ homeassistant/components/mqtt/config_flow.py:4638:13: error[no-matching-overload] No overload of bound method `update` matches arguments
+ homeassistant/components/mqtt/config_flow.py:4639:13: error[no-matching-overload] No overload of bound method `update` matches arguments
+ homeassistant/components/mqtt/entity.py:382:17: error[no-matching-overload] No overload of bound method `update` matches arguments
+ homeassistant/components/mqtt/entity.py:383:17: error[no-matching-overload] No overload of bound method `update` matches arguments
+ homeassistant/components/traccar/device_tracker.py:129:9: error[invalid-assignment] Object of type `DeviceInfo` is not assignable to attribute `_attr_device_info` of type `None`
+ homeassistant/components/weather/__init__.py:703:46: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ homeassistant/config_entries.py:3911:33: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str]) | (Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str])` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
+ homeassistant/config_entries.py:3912:12: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str]) | (Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str])` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
+ homeassistant/helpers/device_registry.py:1456:13: error[invalid-argument-type] Argument to bound method `async_fire_internal` is incorrect: Expected `EventType[_EventDeviceRegistryUpdatedData_Remove] | str`, found `EventType[EventDeviceRegistryUpdatedData]`
+ homeassistant/helpers/device_registry.py:1861:36: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str]) | (Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str])` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
+ homeassistant/helpers/entity_registry.py:1079:13: error[invalid-argument-type] Argument to bound method `async_fire_internal` is incorrect: Expected `EventType[_EventEntityRegistryUpdatedData_CreateRemove] | str`, found `EventType[EventEntityRegistryUpdatedData]`
+ homeassistant/helpers/entity_registry.py:1126:13: error[invalid-argument-type] Argument to bound method `async_fire_internal` is incorrect: Expected `EventType[_EventEntityRegistryUpdatedData_CreateRemove] | str`, found `EventType[EventEntityRegistryUpdatedData]`
+ homeassistant/helpers/entity_registry.py:1378:43: error[invalid-argument-type] Argument to bound method `async_fire_internal` is incorrect: Expected `EventType[_EventEntityRegistryUpdatedData_Update] | str`, found `EventType[EventEntityRegistryUpdatedData]`
- Found 14498 diagnostics
+ Found 14517 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/_internal/_discriminated_union.py:223:13: error[invalid-argument-type] Argument to function `tagged_union_schema` is incorrect: Expected `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 4 union elements`, found `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 6 union elements`
+ pydantic/_internal/_discriminated_union.py:270:83: error[invalid-argument-type] Argument to bound method `_is_discriminator_shared` is incorrect: Expected `TaggedUnionSchema`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`
+ pydantic/_internal/_discriminated_union.py:342:70: error[invalid-argument-type] Argument to bound method `_infer_discriminator_values_for_model_choice` is incorrect: Expected `ModelFieldsSchema`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`
+ pydantic/_internal/_discriminated_union.py:345:74: error[invalid-argument-type] Argument to bound method `_infer_discriminator_values_for_dataclass_choice` is incorrect: Expected `DataclassArgsSchema`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`
+ pydantic/_internal/_discriminated_union.py:348:75: error[invalid-argument-type] Argument to bound method `_infer_discriminator_values_for_typed_dict_choice` is incorrect: Expected `TypedDictSchema`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`
+ pydantic/_internal/_generate_schema.py:270:34: error[invalid-assignment] Invalid assignment to key "items_schema" with declared type `list[InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements]` on TypedDict `TupleSchema`: value of type `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`
+ pydantic/_internal/_generate_schema.py:2823:77: error[invalid-argument-type] Argument to function `_inlining_behavior` is incorrect: Expected `DefinitionReferenceSchema`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`
+ pydantic/functional_validators.py:238:17: error[invalid-argument-type] Argument to function `with_info_plain_validator_function` is incorrect: Expected `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 4 union elements`, found `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 6 union elements`
+ pydantic/functional_validators.py:245:17: error[invalid-argument-type] Argument to function `no_info_plain_validator_function` is incorrect: Expected `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 4 union elements`, found `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 6 union elements`
+ pydantic/json_schema.py:563:49: error[invalid-argument-type] Argument to function `populate_defs` is incorrect: Expected `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 53 union elements`
+ pydantic/json_schema.py:585:41: error[invalid-argument-type] Argument to function `populate_defs` is incorrect: Expected `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 53 union elements`
- pydantic/json_schema.py:1808:30: error[invalid-assignment] Object of type `Any | None` is not assignable to `ConfigDict`
+ pydantic/json_schema.py:2857:16: error[invalid-return-type] Return type does not match returned value: expected `PlainSerializerFunctionSerSchema | None`, found `(SimpleSerSchema & ~AlwaysFalsy) | (PlainSerializerFunctionSerSchema & ~AlwaysFalsy) | (WrapSerializerFunctionSerSchema & ~AlwaysFalsy) | ... omitted 5 union elements`
+ pydantic/types.py:3135:13: error[invalid-argument-type] Argument to function `tagged_union_schema` is incorrect: Expected `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 4 union elements`, found `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 6 union elements`
- Found 4829 diagnostics
+ Found 4841 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@oconnor663 reviewed on 2025-11-15 01:09_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:207 on 2025-11-15 01:09_

This rule was present in historical versions of the typing spec, but it's missing from the current version. It is mentioned and tested [in the conformance suite](https://github.com/python/typing/blob/89c74e36407cee4bb855de83c19d740ffb782a95/conformance/tests/typeddicts_readonly_consistency.py#L61-L62). After this lands I'll open a PR to the typing docs upstream.

---

_@chatgpt-codex-connector[bot] reviewed on 2025-11-15 01:11_


### 💡 Codex Review

Here are some automated review suggestions for this pull request.
    

<details> <summary>ℹ️ About Codex in GitHub</summary>
<br/>

[Your team has set up Codex to review pull requests in this repo](http://chatgpt.com/codex/settings/general). Reviews are triggered when you
- Open a pull request for review
- Mark a draft as ready
- Comment "@codex review".

If Codex has suggestions, it will comment; otherwise it will react with 👍.




Codex can also answer questions or update the PR. Try commenting "@codex address that feedback".
            
</details>

---

_Review comment by @chatgpt-codex-connector[bot] on `crates/ty_python_semantic/src/types/typed_dict.rs`:178 on 2025-11-15 01:11_

**<sub><sub>![P1 Badge](https://img.shields.io/badge/P1-orange?style=flat)</sub></sub>  Guard reverse TypedDict comparison with cycle detector**

In `has_relation_to_other_typeddict_impl` the mutable‐field branch enforces “relation must hold both ways” by calling `self_item_field.declared_ty.has_relation_to_impl(..)` and then immediately calling `target_item_field.declared_ty.has_relation_to_impl(.., self_item_field.declared_ty, ..)` (lines 174‑178). Only the first call is wrapped in `relation_visitor.visit`, so the second one re‑enters `Type::has_relation_to_impl` for the same `(target, self)` pair without ever registering it with the cycle detector. For mutually recursive `TypedDict`s (e.g. `class A(TypedDict): child: "B"; class B(TypedDict): child: "A"`) this causes unbounded recursion/stack overflow whenever we check assignability/subtyping, because the algorithm bounces between `(A,B)` and `(B,A)` without the visitor ever short‑circuiting. The reverse comparison needs its own `relation_visitor.visit((target_item_field.declared_ty, self_item_field.declared_ty, relation), …)` (likewise in the `NotRequired` mutable branch) so that cyclic `TypedDict`s terminate instead of crashing.

Useful? React with 👍 / 👎.

---

_Label `ty` added by @AlexWaygood on 2025-11-15 01:12_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-11-15 01:15_

---

_@oconnor663 reviewed on 2025-11-15 01:18_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:178 on 2025-11-15 01:18_

This seems blatantly false :-D It's true that the "visit item" is only `(self, target)` and not the reverse, but I don't think that's a problem.

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 01:23_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 94 | 0 | 1 |
| `no-matching-overload` | 23 | 0 | 0 |
| `unused-ignore-comment` | 0 | 23 | 0 |
| `invalid-assignment` | 7 | 6 | 1 |
| `invalid-return-type` | 1 | 3 | 1 |
| `invalid-key` | 0 | 4 | 0 |
| `type-assertion-failure` | 0 | 2 | 0 |
| `unsupported-operator` | 2 | 0 | 0 |
| **Total** | **127** | **38** | **3** |

**[Full report with detailed diff](https://jack-typedict-assignment.ecosystem-663.pages.dev/diff)** ([timing results](https://jack-typedict-assignment.ecosystem-663.pages.dev/timing))




---

_Comment by @oconnor663 on 2025-11-15 01:32_

It seems pretty common in the ecosystem report to want to pass a `TypedDict` where a `dict` is required (but you know it won't be modified), like as an argument to `dict.update`. That's kind of a bummer.

---

_Comment by @carljm on 2025-11-15 01:40_

> It seems pretty common in the ecosystem report to want to pass a `TypedDict` where a `dict` is required (but you know it won't be modified), like as an argument to `dict.update`. That's kind of a bummer.

If there are cases like this showing up a lot in the ecosystem, can we just verify quick that we aren't being more strict than other type checkers? If not, then these are just true positives (at least as far as the specified type system is concerned) in the ecosystem that we are catching, great. If so, maybe other type checkers are implementing some kind of pragmatic compromise that we should at least consider.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:296 on 2025-11-15 20:53_

So, it's definitely true that a `TypedDict` isn't assignable to `dict[str, object]` or even `dict[Any, Any]`! However, it's not true that the _only_ non-`TypedDict` a `TypedDict` is assignable to is `Mapping[str, object]`. There are actually lots of non-`TypedDict` types that you can assign `TypedDict`s to!

For a start, _all_ objects in Python are instances of `object`, so we shouldn't complain if a user does something like this (I don't think we will on your branch, because it's dealt with in one of the earlier clauses in the big `match` in `Type::has_relation_to_impl`, but it's illustrative of the point):

```py
from typing import TypedDict

class Foo(TypedDict):
    x: int

def needs_object(x: object): ...

def takes_foo(foo: Foo):
    needs_object(foo)
```

Secondly, `Mapping` itself in typeshed [inherits from `Collection`](https://github.com/python/typeshed/blob/63b498d240d5568eb5b2b75f457361605d709fb3/stdlib/typing.pyi#L758), and `Collection` inherits from other classes such as `Container` and `Iterable`. Since `Mapping` is assignable to `Collection`, `Container` and `Iterable`, all `TypedDict` types should be too.

Thirdly, `TypedDict`s aren't the only structural types in town! We already have `Protocol`s implemented on `main`, and they also participate in structural typing. `Bar` should be assignable to `P` in the following example:

```py
from typing import TypedDict, Protocol, Literal

class Bar(TypedDict):
    x: int

class P(Protocol):
    def __getitem__(self, value: Literal["x"], /) -> int: ...
```

Two changes to your branch should fix these issues:
1. Delegate to `Mapping[str, object]` rather than singling it out. This approach is simpler than your current approach, and it's also more generalised. It means that anything `Mapping[str, object]` is assignable to, any `TypedDict` will also be assignable to. This fixes the issues with assignability for the classes that `Mapping` inherits from.
2. Move the `Type::TypedDict` branches below the `Type::ProtocolInstance()` branches in the big `Type::has_relation_to_impl` `match` expression. This fixes the issues with `TypedDict` <-> `Protocol` subtyping.

TL;DR -- here's a patch that fixes these issues

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 7112be56f6..d37bdb888a 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -2078,17 +2078,6 @@ impl<'db> Type<'db> {
                 ConstraintSet::from(false)
             }
 
-            (Type::TypedDict(self_typeddict), _) => self_typeddict.has_relation_to_impl(
-                db,
-                target,
-                inferable,
-                relation,
-                relation_visitor,
-                disjointness_visitor,
-            ),
-            // A non-`TypedDict` cannot subtype a `TypedDict`
-            (_, Type::TypedDict(_)) => ConstraintSet::from(false),
-
             // Note that the definition of `Type::AlwaysFalsy` depends on the return value of `__bool__`.
             // If `__bool__` always returns True or False, it can be treated as a subtype of `AlwaysTruthy` or `AlwaysFalsy`, respectively.
             (left, Type::AlwaysFalsy) => ConstraintSet::from(left.bool(db).is_always_false()),
@@ -2195,6 +2184,34 @@ impl<'db> Type<'db> {
             // A protocol instance can never be a subtype of a nominal type, with the *sole* exception of `object`.
             (Type::ProtocolInstance(_), _) => ConstraintSet::from(false),
 
+            (Type::TypedDict(self_typeddict), Type::TypedDict(other_typeddict)) => self_typeddict
+                .has_relation_to_impl(
+                    db,
+                    other_typeddict,
+                    inferable,
+                    relation,
+                    relation_visitor,
+                    disjointness_visitor,
+                ),
+
+            // TODO: When we support `closed` and/or `extra_items`, we could allow assignments to other
+            // compatible `Mapping`s. `extra_items` could also allow for some assignments to `dict`, as
+            // long as `total=False`. (But then again, does anyone want a non-total `TypedDict` where all
+            // key types are a supertype of the extra items type?)
+            (Type::TypedDict(_), _) => KnownClass::Mapping
+                .to_specialized_instance(db, [KnownClass::Str.to_instance(db), Type::object()])
+                .has_relation_to_impl(
+                    db,
+                    target,
+                    inferable,
+                    relation,
+                    relation_visitor,
+                    disjointness_visitor,
+                ),
+
+            // A non-`TypedDict` cannot subtype a `TypedDict`
+            (_, Type::TypedDict(_)) => ConstraintSet::from(false),
+
             // All `StringLiteral` types are a subtype of `LiteralString`.
             (Type::StringLiteral(_), Type::LiteralString) => ConstraintSet::from(true),
 
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index 7ae03c9ce4..df20b0a49a 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -81,32 +81,9 @@ impl<'db> TypedDictType<'db> {
         }
     }
 
-    pub(crate) fn has_relation_to_impl(
-        self,
-        db: &'db dyn Db,
-        target: Type<'db>,
-        inferable: InferableTypeVars<'_, 'db>,
-        relation: TypeRelation<'db>,
-        relation_visitor: &HasRelationToVisitor<'db>,
-        disjointness_visitor: &IsDisjointVisitor<'db>,
-    ) -> ConstraintSet<'db> {
-        if let Type::TypedDict(target_typed_dict) = target {
-            self.has_relation_to_other_typeddict_impl(
-                db,
-                target_typed_dict,
-                inferable,
-                relation,
-                relation_visitor,
-                disjointness_visitor,
-            )
-        } else {
-            Self::has_relation_to_non_typeddict_impl(db, target)
-        }
-    }
-
     // Subtyping between `TypedDict`s follows the algorithm described at:
     // https://typing.python.org/en/latest/spec/typeddict.html#subtyping-between-typeddict-types
-    fn has_relation_to_other_typeddict_impl(
+    pub(super) fn has_relation_to_impl(
         self,
         db: &'db dyn Db,
         target: TypedDictType<'db>,
@@ -289,34 +266,6 @@ impl<'db> TypedDictType<'db> {
         }
         constraints
     }
-
-    // The only non-`TypedDict` that a `TypedDict` is assignable to is `Mapping[str, object]`.
-    // Although every instance of `TypedDict` is also an instance of `dict[str, object]`, a
-    // `TypedDict` still isn't assignable to `dict`, because mutating the `dict` could break
-    // `TypeDict`'s "structural" invariants.
-    // TODO: When we support `closed` and/or `extra_items`, we could allow assignments to other
-    // compatible `Mapping`s. `extra_items` could also allow for some assignments to `dict`, as
-    // long as `total=False`. (But then again, does anyone want a non-total `TypedDict` where all
-    // key types are a supertype of the extra items type?)
-    fn has_relation_to_non_typeddict_impl(
-        db: &'db dyn Db,
-        target: Type<'db>,
-    ) -> ConstraintSet<'db> {
-        debug_assert!(!matches!(target, Type::TypedDict(_)));
-        if let Some(specialization) = target.known_specialization(db, KnownClass::Mapping)
-            && let &[key_ty, value_ty] = specialization.types(db)
-            // The key type must be exactly `str`.
-            && key_ty == KnownClass::Str.to_instance(db)
-            // The value type must be `object` (or a gradual type that's consistent with `object`).
-            && KnownClass::Object
-                .to_instance(db)
-                .is_assignable_to(db, value_ty)
-        {
-            ConstraintSet::from(true)
-        } else {
-            ConstraintSet::from(false)
-        }
-    }
 }
 
 pub(crate) fn walk_typed_dict_type<'db, V: visitor::TypeVisitor<'db> + ?Sized>(
```

</details>

And here's a test that you could add, which has several failing assertions on your branch currently but which passes with my patch above applied:

<details>
<summary>Test</summary>

```py
from typing import TypedDict, Literal, Protocol, Collection, Iterable, Container
from ty_extensions import static_assert, is_subtype_of

class P(Protocol):
    def __getitem__(self, value: Literal["x"], /) -> int: ...

class Foo(TypedDict):
    x: int

static_assert(is_subtype_of(Foo, object))
static_assert(is_subtype_of(Foo, Collection[str]))
static_assert(is_subtype_of(Foo, Iterable[str]))
static_assert(is_subtype_of(Foo, Container[str]))
static_assert(is_subtype_of(Foo, P))
```

</details>

---

_@AlexWaygood had review dismissed on 2025-11-15 20:54_

Thank you!! I haven't looked through the whole PR yet, but spotted one significant thing

---

_@carljm reviewed on 2025-11-15 21:47_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/typed_dict.rs`:296 on 2025-11-15 21:47_

I think that in principle a TypedDict should be assignable to any `Mapping[str, X]`, where `X` is a supertype of the union of all field types in the `TypedDict` (thus `object` would of course always qualify). Does the wording in the spec prohibit this?

---

_@carljm reviewed on 2025-11-15 21:48_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/typed_dict.rs`:296 on 2025-11-15 21:48_

Oh never mind, this is only true for typed dicts with `closed` or `extra_items`, and it's already covered in a TODO comment above.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:486 on 2025-11-16 16:42_

nit: rather than talking about "target" and "source" here (I can't remember which is which 😄), I think clearer language would be to say something like "In order for one `TypedDict` `X` to be assignable to another `TypedDict` `Y`, all required keys in `Y`'s schema must also be marked as `Required` in `X`'s schema".

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:555 on 2025-11-16 16:48_

nit: all of your tests for assignability appear to be of this form, which are sort-of "integration tests" for assignability -- they test the _effect_ of one `TypedDict` being (not) assignable to another, rather than directly testing that one `TypedDict` is (not) assignable to the other. Integration tests like that are useful, but our "usual style" for this kind of test is to write it more as a "unit test", and directly assert that one type is not assignable to another type using our `ty_extensions` helper functions `static_assert`, `is_assignable_to` and `is_subtype_of`. For example: https://github.com/astral-sh/ruff/blob/665f68036c33301c6d60801b309def48cb3c8894/crates/ty_python_semantic/resources/mdtest/protocols.md?plain=1#L583-L622

As well as being more in line with our "usual style" elsewhere, I'd find it more readable if your tests took this form. Lastly, it would also allow you to explicitly test subtyping as well as assignability (I don't _think_ you have any tests for subtyping yet?).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:140 on 2025-11-16 17:18_

I think you're falling into the trap of "double visitation" here that's called out in the last paragraph of the doc-comment in `cyclic.rs`: https://github.com/astral-sh/ruff/blob/665f68036c33301c6d60801b309def48cb3c8894/crates/ty_python_semantic/src/types/cyclic.rs#L1-L20

I think you probably want to get rid of all the `visitor.visit()` calls in this method, but add a `visitor.visit()` call to the main recursive `Type::has_relation_to_impl` method, as that doc-comment suggests -- e.g. all tests pass if I apply this patch to your branch. It also has the advantage of being slightly simpler than what you currently have :-)

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 7112be56f6..6cd8f7cd7b 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -2078,14 +2078,19 @@ impl<'db> Type<'db> {
                 ConstraintSet::from(false)
             }
 
-            (Type::TypedDict(self_typeddict), _) => self_typeddict.has_relation_to_impl(
-                db,
-                target,
-                inferable,
-                relation,
-                relation_visitor,
-                disjointness_visitor,
-            ),
+            (Type::TypedDict(self_typeddict), _) => {
+                relation_visitor.visit((self, target, relation), || {
+                    self_typeddict.has_relation_to_impl(
+                        db,
+                        target,
+                        inferable,
+                        relation,
+                        relation_visitor,
+                        disjointness_visitor,
+                    )
+                })
+            }
+
             // A non-`TypedDict` cannot subtype a `TypedDict`
             (_, Type::TypedDict(_)) => ConstraintSet::from(false),
 
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index 7ae03c9ce4..d1ba6ecdb7 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -131,60 +131,51 @@ impl<'db> TypedDictType<'db> {
                     // A required field is not required in self.
                     return ConstraintSet::from(false);
                 }
-                relation_visitor.visit(
-                    (
-                        self_item_field.declared_ty,
+                if target_item_field.is_read_only() {
+                    // For `ReadOnly[]` fields in the target, the corresponding fields in
+                    // self need to have the same assignability/subtyping/etc relation
+                    // individually that we're looking for overall between the
+                    // `TypedDict`s.
+                    self_item_field.declared_ty.has_relation_to_impl(
+                        db,
                         target_item_field.declared_ty,
+                        inferable,
                         relation,
-                    ),
-                    || {
-                        if target_item_field.is_read_only() {
-                            // For `ReadOnly[]` fields in the target, the corresponding fields in
-                            // self need to have the same assignability/subtyping/etc relation
-                            // individually that we're looking for overall between the
-                            // `TypedDict`s.
-                            self_item_field.declared_ty.has_relation_to_impl(
+                        relation_visitor,
+                        disjointness_visitor,
+                    )
+                } else {
+                    if self_item_field.is_read_only() {
+                        // A read-only field can't be assigned to a mutable target.
+                        return ConstraintSet::from(false);
+                    }
+                    // For mutable fields in the target, the relation needs to apply both
+                    // ways, or else mutating the target could violate the structural
+                    // invariants of self. For fully-static types, this is "equivalence".
+                    // For gradual types, it depends on the relation, but mutual
+                    // assignability is "consistency".
+                    self_item_field
+                        .declared_ty
+                        .has_relation_to_impl(
+                            db,
+                            target_item_field.declared_ty,
+                            inferable,
+                            relation,
+                            relation_visitor,
+                            disjointness_visitor,
+                        )
+                        .intersect(
+                            db,
+                            target_item_field.declared_ty.has_relation_to_impl(
                                 db,
-                                target_item_field.declared_ty,
+                                self_item_field.declared_ty,
                                 inferable,
                                 relation,
                                 relation_visitor,
                                 disjointness_visitor,
-                            )
-                        } else {
-                            if self_item_field.is_read_only() {
-                                // A read-only field can't be assigned to a mutable target.
-                                return ConstraintSet::from(false);
-                            }
-                            // For mutable fields in the target, the relation needs to apply both
-                            // ways, or else mutating the target could violate the structural
-                            // invariants of self. For fully-static types, this is "equivalence".
-                            // For gradual types, it depends on the relation, but mutual
-                            // assignability is "consistency".
-                            self_item_field
-                                .declared_ty
-                                .has_relation_to_impl(
-                                    db,
-                                    target_item_field.declared_ty,
-                                    inferable,
-                                    relation,
-                                    relation_visitor,
-                                    disjointness_visitor,
-                                )
-                                .intersect(
-                                    db,
-                                    target_item_field.declared_ty.has_relation_to_impl(
-                                        db,
-                                        self_item_field.declared_ty,
-                                        inferable,
-                                        relation,
-                                        relation_visitor,
-                                        disjointness_visitor,
-                                    ),
-                                )
-                        }
-                    },
-                )
+                            ),
+                        )
+                }
             } else {
                 // `NotRequired[]` target fields
                 if target_item_field.is_read_only() {
@@ -195,22 +186,13 @@ impl<'db> TypedDictType<'db> {
                     // spec are structured like they are), and following the structure of the spec
                     // makes it easier to check the logic here.
                     if let Some(self_item_field) = self_items.get(target_item_name) {
-                        relation_visitor.visit(
-                            (
-                                self_item_field.declared_ty,
-                                target_item_field.declared_ty,
-                                relation,
-                            ),
-                            || {
-                                self_item_field.declared_ty.has_relation_to_impl(
-                                    db,
-                                    target_item_field.declared_ty,
-                                    inferable,
-                                    relation,
-                                    relation_visitor,
-                                    disjointness_visitor,
-                                )
-                            },
+                        self_item_field.declared_ty.has_relation_to_impl(
+                            db,
+                            target_item_field.declared_ty,
+                            inferable,
+                            relation,
+                            relation_visitor,
+                            disjointness_visitor,
                         )
                     } else {
                         // Self is missing this not-required, read-only item. However, since all
@@ -239,38 +221,30 @@ impl<'db> TypedDictType<'db> {
                             // in the target, because `del` is allowed on the target field.
                             return ConstraintSet::from(false);
                         }
-                        relation_visitor.visit(
-                            (
-                                self_item_field.declared_ty,
+
+                        // As above, for mutable fields in the target, the relation needs
+                        // to apply both ways.
+                        self_item_field
+                            .declared_ty
+                            .has_relation_to_impl(
+                                db,
                                 target_item_field.declared_ty,
+                                inferable,
                                 relation,
-                            ),
-                            || {
-                                // As above, for mutable fields in the target, the relation needs
-                                // to apply both ways.
-                                self_item_field
-                                    .declared_ty
-                                    .has_relation_to_impl(
-                                        db,
-                                        target_item_field.declared_ty,
-                                        inferable,
-                                        relation,
-                                        relation_visitor,
-                                        disjointness_visitor,
-                                    )
-                                    .intersect(
-                                        db,
-                                        target_item_field.declared_ty.has_relation_to_impl(
-                                            db,
-                                            self_item_field.declared_ty,
-                                            inferable,
-                                            relation,
-                                            relation_visitor,
-                                            disjointness_visitor,
-                                        ),
-                                    )
-                            },
-                        )
+                                relation_visitor,
+                                disjointness_visitor,
+                            )
+                            .intersect(
+                                db,
+                                target_item_field.declared_ty.has_relation_to_impl(
+                                    db,
+                                    self_item_field.declared_ty,
+                                    inferable,
+                                    relation,
+                                    relation_visitor,
+                                    disjointness_visitor,
+                                ),
+                            )
                     } else {
                         // Self is missing this not-required, mutable field. This isn't ok if self
                         // has read-only extra items, which all `TypedDict`s effectively do until
```

</details>

---

_@AlexWaygood reviewed on 2025-11-16 17:18_

---

_Comment by @AlexWaygood on 2025-11-16 17:28_

> Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

It looks like there are a few new false positives on the typing conformance suite file `typeddicts_extra_items.py` -- places where we're not meant to emit diagnostic, but we now do:
- [this line](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_extra_items.py#L300)
- [this line](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_extra_items.py#L304)
- [this line](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_extra_items.py#L326)
- [this line](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_extra_items.py#L330)

I guess this is just because we don't support `extra_items` at all yet? So maybe this is just all covered by the "PEP 728 support" bullet point in https://github.com/astral-sh/ty/issues/154.

Other than the lines listed above, all other new diagnostics on the conformance suite look like true positives to me -- great job!

---

_Comment by @AlexWaygood on 2025-11-16 17:40_

Unfortunately it looks like there is a huge slowdown on the pydantic benchmark on this branch, which is causing it to timeout in CI currently. On my macbook locally, the benchmark completes in 2.161s on `main`, but 1.017m (I assume `m` here stands for minutes!) on this branch.

I suggest tackling some of the correctness issues I pointed out in https://github.com/astral-sh/ruff/pull/21467#discussion_r2530136276 and https://github.com/astral-sh/ruff/pull/21467#discussion_r2532093692 before looking at this too much. There's always a chance that fixing one/both of those will "miraculously" fix the regression 😆

---

_Comment by @oconnor663 on 2025-11-17 17:44_

> > It seems pretty common in the ecosystem report to want to pass a `TypedDict` where a `dict` is required (but you know it won't be modified), like as an argument to `dict.update`. That's kind of a bummer.
> 
> If there are cases like this showing up a lot in the ecosystem, can we just verify quick that we aren't being more strict than other type checkers? If not, then these are just true positives (at least as far as the specified type system is concerned) in the ecosystem that we are catching, great. If so, maybe other type checkers are implementing some kind of pragmatic compromise that we should at least consider.

Ah, this turned out to be exactly the issue that @AlexWaygood caught above: I was being too strict and making `TypedDict`s assignable _only_ to `Mapping[str, object]`, rather than to anything that `Mapping[str, object]` is assignable to. Alex's patch fixes this.

---

_Comment by @oconnor663 on 2025-11-17 19:28_

> It looks like there are a few new false positives on the typing conformance suite file `typeddicts_extra_items.py` -- places where we're not meant to emit diagnostic, but we now do...I guess this is just because we don't support `extra_items` at all yet?

Yes I think all of the new errors-that-shouldn't-be-there are cases where `extra_items` makes more assignments to `dict` legal. It might not be too hard to add `extra_items` support as part of this (or even to add a blanket "if you use `extra_items` we assume all assignments are legal" workaround), but then again since it's a Python 3.15 feature I'm leaning towards just accepting the false positives for a little while for simplicity?

---

_@oconnor663 reviewed on 2025-11-17 19:33_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:555 on 2025-11-17 19:33_

I've replaced actual assignments with `is_assignable_to` in most but not all cases (and added `is_subclass_of` in some). Let me know how the current balance feels :)

---

_Comment by @oconnor663 on 2025-11-17 20:00_

> There's always a chance that fixing one/both of those will "miraculously" fix the regression

Alas, no. Digging into it.

---

_Comment by @oconnor663 on 2025-11-17 22:43_

I don't have an answer yet, but an example of a specific file that's giving us trouble (which might be infecting a lot of other files, not sure) is [`pydantic-core/python/pydantic_core/core_schema.py`](https://github.com/pydantic/pydantic-core/blob/main/python/pydantic_core/core_schema.py). That file defines a bunch of "*Schema" classes that are all `TypedDict`s, and it seems like most of those have one or more fields of type `CoreSchema`, which is actually a large `Union` of all the "*Schema" classes. So there's a _lot_ of recursion going on.

---

_Comment by @AlexWaygood on 2025-11-17 23:04_

Something you could try would be to add Salsa caching to `Type::is_assignable_to`. We already cache `Type::is_redundant_with`; you'd want to add the `#[salsa::tracked]` attribute to `Type::is_assignable_to` in the same way that `Type::is_redundant_with` already has.

The cost of doing this would be an increased memory usage, but that might be worth it to avoid a 30x execution time increase and benchmarks that no longer complete in CI... worth experimenting with, anyway.

---

_@AlexWaygood reviewed on 2025-11-17 23:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:553 on 2025-11-17 23:06_

Heh, we usually do this to assert that something's false 😄

```suggestion
static_assert(not is_assignable_to(RequiredReadOnlyInt, RequiredMutableInt))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:154 on 2025-11-17 23:12_

I think you can improve performance here by being lazier -- if you use `and()` rather than `intersect()`, you'll avoid eagerly computing the second set of constraints if the first set of constraints is already known to be unsatisfiable

```suggestion
                        .and(db, || {
                            target_item_field.declared_ty.has_relation_to_impl(
                                db,
                                self_item_field.declared_ty,
                                inferable,
                                relation,
                                relation_visitor,
                                disjointness_visitor,
                            )
                        })
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:224 on 2025-11-17 23:14_

same here

```suggestion
                            .and(db, || {
                                target_item_field.declared_ty.has_relation_to_impl(
                                    db,
                                    self_item_field.declared_ty,
                                    inferable,
                                    relation,
                                    relation_visitor,
                                    disjointness_visitor,
                                )
                            })
```

---

_@AlexWaygood reviewed on 2025-11-17 23:14_

---

_@oconnor663 reviewed on 2025-11-17 23:32_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:553 on 2025-11-17 23:32_

oof, you can see where I was coming from tho right :-D

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:154 on 2025-11-17 23:54_

No measureable improvement so far, but it seems like the better way to do it either way.

---

_@oconnor663 reviewed on 2025-11-17 23:54_

---

_@AlexWaygood reviewed on 2025-11-18 14:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:184 on 2025-11-18 14:49_

slightly more efficient, and less verbose:

```suggestion
                        Type::object().when_assignable_to(
                            db,
                            target_item_field.declared_ty,
                            inferable,
                        )
```

also, is really correct that we should use `when_assignable_to` even if `relation == TypeRelation::Subtyping`? Should it be this instead?

```suggestion
                        Type::object().has_relation_to_impl(
                            db,
                            target_item_field.declared_ty,
                            inferable,
                            relation,
                            relation_visitor,
                            disjointness_visitor,
                        )
```

(no tests fail if I make that change, so it looks like we might be missing some test coverage here either way ;)

---

_@oconnor663 reviewed on 2025-11-18 19:20_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:184 on 2025-11-18 19:20_

weird: committing the first suggestion resolves the whole comment, even though the second suggestion is pending

---

_@oconnor663 reviewed on 2025-11-18 19:23_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:184 on 2025-11-18 19:23_

What's an example of something I can do in Python that demands the "subtyping" relation, as opposed to the "assignability" relation? (Besides `ty_extensions.is_subtype_of` I guess.) I get that they're going to be the same for fully-static types, so I know part of the answer is going to involve `Any` or similar, but I'm afraid I've gotten this far without actually knowing what subtyping per se is _for_ :-D

(Edit: I went ahead and asked this in chat.)

---

_Comment by @oconnor663 on 2025-11-18 19:59_

Rebased on top of @AlexWaygood's https://github.com/astral-sh/ruff/pull/21512. Checking Pydantic performs much better for me locally now, so I expect CI to...pass? There's still a substantial slowdown (on the order of 2x?) which we'll need to make a judgment call on. Are we "doing more work" in a way that makes some slowdowns expected? Is there an objective way to know?

---

_Comment by @codspeed-hq[bot] on 2025-11-18 20:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jack%2Ftypedict_assignment?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21467 will **degrade performances by 90.42%**

<sub>Comparing <code>jack/typedict_assignment</code> (9a33cdd) with <code>main</code> (1b28fc1)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/jack%2Ftypedict_assignment?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/jack%2Ftypedict_assignment?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 19.5 s | 203.4 s | -90.42% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/jack%2Ftypedict_assignment?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @AlexWaygood on 2025-11-18 20:21_

Shoot, I only checked how well https://github.com/astral-sh/ruff/pull/21512 did on the benchmarks when it was combined with this PR. I didn't spot that it introduced a panic on pydantic (which is now why mypy_primer is failing in CI) 😞

This is how we'd add cycle handling to `ClassLiteral::fields()`:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 5f8d18908f..91ed2dc330 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -94,6 +94,16 @@ fn implicit_attribute_initial<'db>(
     Member::unbound()
 }
 
+fn fields_initial<'db>(
+    _db: &'db dyn Db,
+    _id: salsa::Id,
+    _self: ClassLiteral<'db>,
+    _specialization: Option<Specialization<'db>>,
+    _field_policy: CodeGeneratorKind<'db>,
+) -> FxIndexMap<Name, Field<'db>> {
+    FxIndexMap::default()
+}
+
 fn try_mro_cycle_initial<'db>(
     db: &'db dyn Db,
     _id: salsa::Id,
@@ -2824,7 +2834,11 @@ impl<'db> ClassLiteral<'db> {
     /// Returns a list of all annotated attributes defined in this class, or any of its superclasses.
     ///
     /// See [`ClassLiteral::own_fields`] for more details.
-    #[salsa::tracked(returns(ref), heap_size=get_size2::GetSize::get_heap_size)]
+    #[salsa::tracked(
+        returns(ref),
+        cycle_initial=fields_initial,
+        heap_size=get_size2::GetSize::get_heap_size
+    )]
     pub(crate) fn fields(
         self,
         db: &'db dyn Db,
```

...but in this case, it just turns the "dependency cycle detected -- please add cycle handling!" panic into a "too many iterations when handling the cycle" panic.

@carljm, I don't suppose you have any suggestions for how to resolve this...?

---

_Comment by @oconnor663 on 2025-11-18 21:12_

The panic seems to minimize to this example:

```py
from __future__ import annotations

from typing import TypedDict, Union

class Foo(TypedDict):
    foo: MyUnion

class Bar(TypedDict):
    bar: MyUnion

MyUnion = Union[
    Foo,
    Bar,
]
```
```
error[panic]: Panicked at /home/jacko/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/fetch.rs:227:21 when checking `/tmp/pydantic/pydantic-core/tests/validators/test_lax_or_strict.py`: `dependency graph cycle when querying ClassLiteral < 'db >::fields_(Id(7400)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_file_impl(Id(c00)),
    infer_scope_types(Id(1001)),
    infer_definition_types(Id(1403)),
    Type < 'db >::is_redundant_with_(Id(7000)),
    ClassLiteral < 'db >::fields_(Id(7400)),
    infer_definition_types(Id(1405)),
    Type < 'db >::is_redundant_with_(Id(7001)),
    ClassLiteral < 'db >::fields_(Id(7401)),
    infer_definition_types(Id(5d98)),
    infer_definition_types(Id(14be)),
    file_to_module(Id(c23)),
    module_type_symbols(Id(5800)),
    place_by_id(Id(3003)),
    infer_definition_types(Id(47d0)),
    infer_definition_types(Id(468a)),
    source_text(Id(c25)),
]`
info: This indicates a bug in ty.
```

---

_Comment by @carljm on 2025-11-18 21:17_

Having not looked too closely yet, my initial take is that this suggests we are querying fields of a typed dict too eagerly and need to make it lazier? There shouldn't be any reason in principle why resolving the type of `Foo.foo` field to the type `Foo | Bar` should require us to fully unpack the fields of the `TypedDict` type `Bar`. (EDIT: Oh, it's because we are trying to simplify the union, of course! We may need some handling in `UnionBuilder` similar to what is already there to support recursive type aliases, so we track "seen typeddicts" and avoid simplifying typeddict types if we are recursing.)

Failing the "more laziness" approach, we'd have to do something hacky with `Type::Divergent` in the cycle recovery function for the `fields` query, or wait for @mtshiba 's PR that does it more generally.

---

_Comment by @AlexWaygood on 2025-11-18 21:30_

Note that if we make this change to the repro:

```diff
  from __future__ import annotations
 
  from typing import TypedDict, Union
  
  class Foo(TypedDict):
      foo: MyUnion
  
  class Bar(TypedDict):
      bar: MyUnion
  
- MyUnion = Union[
+ type MyUnion = Union[
      Foo,
      Bar,
  ]
```

then the panic no longer occurs. I think this is because when we do not attempt to eagerly unpack PEP-695 type aliases when we add them to `UnionBuilder`s (controlled with the `unpack_aliases` flag):

https://github.com/astral-sh/ruff/blob/192c37d5401786b3043aafbfb657b4f5e7df8ac8/crates/ty_python_semantic/src/types/builder.rs#L215

https://github.com/astral-sh/ruff/blob/192c37d5401786b3043aafbfb657b4f5e7df8ac8/crates/ty_python_semantic/src/types/builder.rs#L271-L279

But we do eagerly unpack implicit pre-PEP-695 type aliases (because we don't treat them as first-class opaque types at all)

---

_Comment by @carljm on 2025-11-18 21:35_

I don't think "seen typeddicts" in the union builder is adequate here, the problem is spread across multiple layers of union builder...

---

_Comment by @oconnor663 on 2025-11-18 23:37_

Hmm, that last change fixed my "minimal repro" but not the actual Pydantic check. Time to minimize again?

---

_Comment by @oconnor663 on 2025-11-18 23:56_

New minimal repro. Seems sensitive to the order of appearance of different types within the union too.

```py
from typing import Union, TypedDict

class Foo1(TypedDict):
    x1: MyUnion

class Foo2(TypedDict):
    x2: MyUnion

class Foo3(Foo2):
    x3: MyUnion

class Foo4(TypedDict):
    x4: MyUnion

MyUnion = Foo1 | Foo2 | Foo3 | Foo4
```

Presumably some combinatorial explosion of calls that's dodging cycle detection? Digging into it...

---

_Comment by @carljm on 2025-11-19 01:46_

> Presumably some combinatorial explosion of calls that's dodging cycle detection? Digging into it...

If the error is "too many cycle iterations", then the problem is not dodging cycle detection. That error occurs when Salsa has detected the cycle, attempted to resolve it via fixpoint iteration, but then fixpoint iteration doesn't converge to a stable value. Often this is because we try to eagerly build an ever-more-deeply-recursively-nested type on each iteration, or because iteration flip-flops indefinitely between one value and another.

I haven't looked at this case in detail yet, but my suspicion is that the problem is the union simplification. That is, when we try to build the union type for `MyUnion`, we do a redundancy-relation check between each type in the union, which requires getting the fields of each type, which requires evaluating the annotation on each field, which requires building the `MyUnion` union type, which requires checking redundancy, which requires getting the fields of each type... I suspect that before we added Salsa caching to the fields method, we would have instead hit this cycle in the cached union-redundancy check, which probably converged to an answer more easily in fixpoint iteration since it's just a boolean result. But the fields query, which returns a map of field name to type, is more prone to divergence. In order to tell exactly how it's diverging, you'll need to inspect the values it returns on the problematic example. That may help us come up with ideas for how to fix it.

---

_Comment by @MichaReiser on 2025-11-19 07:34_

The way I try to debug those issues is to add a `tracing::log` to the cycle's recovery function where I log the previous and current value. This way you can see how the query converges (it might be something as simple as that the order of some fields or similar change)

---

_Comment by @AlexWaygood on 2025-11-19 10:20_

> New minimal repro. Seems sensitive to the order of appearance of different types within the union too.
> 
> ```python
> from typing import Union, TypedDict
> 
> class Foo1(TypedDict):
>     x1: MyUnion
> 
> class Foo2(TypedDict):
>     x2: MyUnion
> 
> class Foo3(Foo2):
>     x3: MyUnion
> 
> class Foo4(TypedDict):
>     x4: MyUnion
> 
> MyUnion = Foo1 | Foo2 | Foo3 | Foo4
> ```

I can't reproduce a panic on your branch for this snippet, but I can if I add `from __future__ import annotations` to the top of the file. I'm invoking ty on a file inside the Ruff repo -- possibly you're invoking ty on a file in a slightly different location, which means that it's using our default Python version (3.14, which has deferred annotations by default at runtime) instead of the resolved Python version from Ruff's pyproject.toml file (3.7).

EDIT: ah, and I was explicitly passing `--python-version=3.13` from the CLI to override Ruff's pyproject.toml.

---

_Comment by @AlexWaygood on 2025-11-19 10:34_

I tried out this change locally:

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 17b5fe7625..109a44e159 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -126,16 +126,6 @@ fn try_metaclass_cycle_initial<'db>(
     })
 }
 
-fn fields_cycle_initial<'db>(
-    _db: &'db dyn Db,
-    _id: salsa::Id,
-    _self: ClassLiteral<'db>,
-    _specialization: Option<Specialization<'db>>,
-    _field_policy: CodeGeneratorKind<'db>,
-) -> FxIndexMap<Name, Field<'db>> {
-    FxIndexMap::default()
-}
-
 /// A category of classes with code generation capabilities (with synthesized methods).
 #[derive(Clone, Copy, Debug, PartialEq, Eq, Hash, salsa::Update, get_size2::GetSize)]
 pub(crate) enum CodeGeneratorKind<'db> {
@@ -2339,7 +2329,7 @@ impl<'db> ClassLiteral<'db> {
 
                 // Use the alias name if provided, otherwise use the field name
                 let parameter_name =
-                    Name::new(alias.map(|alias| &**alias).unwrap_or(&**field_name));
+                    Name::new(alias.map(|alias| &**alias).unwrap_or(&*field_name));
 
                 let mut parameter = if is_kw_only {
                     Parameter::keyword_only(parameter_name)
@@ -2603,8 +2593,8 @@ impl<'db> ClassLiteral<'db> {
                 )))
             }
             (CodeGeneratorKind::TypedDict, "get") => {
-                let overloads = self
-                    .fields(db, specialization, field_policy)
+                let fields = self.fields(db, specialization, field_policy);
+                let overloads = fields
                     .iter()
                     .flat_map(|(name, field)| {
                         let key_type =
@@ -2834,7 +2824,6 @@ impl<'db> ClassLiteral<'db> {
     /// Returns a list of all annotated attributes defined in this class, or any of its superclasses.
     ///
     /// See [`ClassLiteral::own_fields`] for more details.
-    #[salsa::tracked(returns(ref), cycle_initial=fields_cycle_initial, heap_size=get_size2::GetSize::get_heap_size)]
     pub(crate) fn fields(
         self,
         db: &'db dyn Db,
diff --git a/crates/ty_python_semantic/src/types/diagnostic.rs b/crates/ty_python_semantic/src/types/diagnostic.rs
index ed8b1be1ab..1542da24ac 100644
--- a/crates/ty_python_semantic/src/types/diagnostic.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic.rs
@@ -35,7 +35,7 @@ use ruff_python_ast::parenthesize::parentheses_iterator;
 use ruff_python_ast::{self as ast, AnyNodeRef, Identifier};
 use ruff_python_trivia::CommentRanges;
 use ruff_text_size::{Ranged, TextRange};
-use rustc_hash::FxHashSet;
+use rustc_hash::{FxHashMap, FxHashSet};
 use std::fmt::Formatter;
 
 /// Registers all known type check lints.
@@ -3190,7 +3190,7 @@ pub(crate) fn report_invalid_key_on_typed_dict<'db>(
     typed_dict_ty: Type<'db>,
     full_object_ty: Option<Type<'db>>,
     key_ty: Type<'db>,
-    items: &FxIndexMap<Name, Field<'db>>,
+    items: &FxHashMap<Name, Field<'db>>,
 ) {
     let db = context.db();
     if let Some(builder) = context.report_lint(&INVALID_KEY, key_node) {
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 85c645d37a..90bbb33ea9 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -919,9 +919,9 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                 CodeGeneratorKind::from_class(self.db(), class, None)
             {
                 let specialization = None;
+                let fields = class.fields(self.db(), specialization, field_policy);
 
-                let kw_only_sentinel_fields: Vec<_> = class
-                    .fields(self.db(), specialization, field_policy)
+                let kw_only_sentinel_fields: Vec<_> = fields
                     .iter()
                     .filter_map(|(name, field)| {
                         field.is_kw_only_sentinel(self.db()).then_some(name)
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index c093dbef25..8e94a4fb1f 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -4,6 +4,7 @@ use ruff_db::parsed::parsed_module;
 use ruff_python_ast::Arguments;
 use ruff_python_ast::{self as ast, AnyNodeRef, StmtClassDef, name::Name};
 use ruff_text_size::Ranged;
+use rustc_hash::FxHashMap;
 
 use super::class::{ClassType, CodeGeneratorKind, Field};
 use super::context::InferContext;
@@ -12,10 +13,10 @@ use super::diagnostic::{
     report_missing_typed_dict_key,
 };
 use super::{ApplyTypeMappingVisitor, Type, TypeMapping, visitor};
+use crate::Db;
 use crate::types::constraints::ConstraintSet;
 use crate::types::generics::InferableTypeVars;
 use crate::types::{HasRelationToVisitor, IsDisjointVisitor, TypeContext, TypeRelation};
-use crate::{Db, FxIndexMap};
 
 use ordermap::OrderSet;
 
@@ -56,9 +57,25 @@ impl<'db> TypedDictType<'db> {
         self.defining_class
     }
 
-    pub(crate) fn items(self, db: &'db dyn Db) -> &'db FxIndexMap<Name, Field<'db>> {
-        let (class_literal, specialization) = self.defining_class.class_literal(db);
-        class_literal.fields(db, specialization, CodeGeneratorKind::TypedDict)
+    pub(crate) fn items(self, db: &'db dyn Db) -> &'db FxHashMap<Name, Field<'db>> {
+        #[salsa::tracked(returns(ref), cycle_initial=items_cycle_initial, heap_size=get_size2::GetSize::get_heap_size)]
+        fn items_inner<'db>(db: &'db dyn Db, class: ClassType<'db>) -> FxHashMap<Name, Field<'db>> {
+            let (class_literal, specialization) = class.class_literal(db);
+            class_literal
+                .fields(db, specialization, CodeGeneratorKind::TypedDict)
+                .into_iter()
+                .collect()
+        }
+
+        fn items_cycle_initial<'db>(
+            _db: &'db dyn Db,
+            _id: salsa::Id,
+            _class: ClassType<'db>,
+        ) -> FxHashMap<Name, Field<'db>> {
+            FxHashMap::default()
+        }
+
+        items_inner(db, self.defining_class)
     }
```

</details>

The idea of the change is to make it easier for the cycle to converge by returning an `FxHashMap` from the cached query rather than an `FxIndexMap`. An `IndexMap` cares about the order of the fields (which can make it hard for the query to converge) but a `HashMap` doesn't. We can't use an order-agnostic map for `ClassLiteral::fields()` because we care about the order of fields for dataclasses and namedtuples -- it allows us to perform certain useful checks -- so the caching is moved up "one level higher" to `TypedDictType::items()`.

With that patch applied, we still panic on the repro from https://github.com/astral-sh/ruff/pull/21467#issuecomment-3549940891 (providing you pass `--python-version=3.14`) -- the panic just moves to `items_inner`:

<details>
<summary>New query stacktrace</summary>

```
error[panic]: Panicked at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:321:21 when checking `/Users/alexw/dev/ruff/foo.py`: `items_inner(Id(4403)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: ruff/0.14.5+57 (4160ea580 2025-11-18)
info: Args: ["target/debug/ty", "check", "foo.py", "--python-version=3.14"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: Type < 'db >::is_redundant_with_(Id(7407))
             at crates/ty_python_semantic/src/types.rs:821
   1: infer_definition_types(Id(1403))
             at crates/ty_python_semantic/src/types/infer.rs:94
             cycle heads: Type < 'db >::is_redundant_with_(Id(7400)) -> iteration = 200
   2: items_inner(Id(4404))
             at crates/ty_python_semantic/src/types/typed_dict.rs:61
   3: Type < 'db >::is_redundant_with_(Id(7400))
             at crates/ty_python_semantic/src/types.rs:821
   4: infer_definition_types(Id(1401))
             at crates/ty_python_semantic/src/types/infer.rs:94
   5: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:70
   6: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535
```

</details>

Note that (with or without my patch above), when invoking `uv run cargo run --manifest-path=../ruff/Cargo.toml --profile=profiling -p ty check pydantic` from the root of my pydantic clone, the panic is due to `Type::is_redundant_with` failing to converge rather than `ClassLiteral::fields()` or `items_inner()`. That also seems to be what we're seeing in the mypy_primer logs:

<details>
<summary>Pydantic query stacktrace</summary>

```
error[panic]: Panicked at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:321:21 when checking `/Users/alexw/dev/pydantic/pydantic/functional_serializers.py`: `Type < 'db >::is_redundant_with_(Id(92ae0)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: ruff/0.14.5+57 (4160ea580 2025-11-18)
info: Args: ["/Users/alexw/dev/ruff/target/profiling/ty", "check", "pydantic"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_definition_types(Id(221d2))
             at crates/ty_python_semantic/src/types/infer.rs:94
             cycle heads: Type < 'db >::is_redundant_with_(Id(63427)) -> iteration = 200
   1: items_inner(Id(15404))
             at crates/ty_python_semantic/src/types/typed_dict.rs:61
   2: Type < 'db >::is_redundant_with_(Id(92ae9))
             at crates/ty_python_semantic/src/types.rs:821
   3: infer_definition_types(Id(22194))
             at crates/ty_python_semantic/src/types/infer.rs:94
             cycle heads: Type < 'db >::is_redundant_with_(Id(63426)) -> iteration = 200
   4: items_inner(Id(2a40b))
             at crates/ty_python_semantic/src/types/typed_dict.rs:61
   5: Type < 'db >::is_redundant_with_(Id(63427))
             at crates/ty_python_semantic/src/types.rs:821
   6: infer_definition_types(Id(221e5))
             at crates/ty_python_semantic/src/types/infer.rs:94
   7: items_inner(Id(15405))
             at crates/ty_python_semantic/src/types/typed_dict.rs:61
   8: Type < 'db >::is_redundant_with_(Id(63426))
             at crates/ty_python_semantic/src/types.rs:821
   9: infer_deferred_types(Id(4c21))
             at crates/ty_python_semantic/src/types/infer.rs:141
  10: infer_scope_types(Id(3401))
             at crates/ty_python_semantic/src/types/infer.rs:70
  11: check_file_impl(Id(1407))
             at crates/ty_project/src/lib.rs:535
```

</details>



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:484 on 2025-11-19 10:50_

```suggestion
static_assert(not is_assignable_to(Person, Employee))
static_assert(not is_assignable_to(Robot, Person))
static_assert(not is_assignable_to(Person, Robot))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:505 on 2025-11-19 10:51_

```suggestion
# invalid because `Spy` might be missing `name`
static_assert(not is_assignable_to(Spy, Person))

# invalid because `Spy` is allowed to delete `name`, while `Person` is not
static_assert(not is_assignable_to(Person, Spy))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:512 on 2025-11-19 10:51_

```suggestion
static_assert(not is_assignable_to(Amnesiac, Person))
```

---

_@AlexWaygood reviewed on 2025-11-19 10:51_

---

_Comment by @AlexWaygood on 2025-11-19 11:33_

This patch gets us passing on both minimal repros so far (https://github.com/astral-sh/ruff/pull/21467#issuecomment-3549529486 and https://github.com/astral-sh/ruff/pull/21467#issuecomment-3549940891), and seems probably worth doing on its own merits, since it'll make cycles rarer and avoid us having to engage in a full structual check for many `TypedDict` assignability checks:

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 17b5fe7625..78beca70d1 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -566,7 +566,9 @@ impl<'db> ClassType<'db> {
                 },
 
                 // Protocol and Generic are not represented by a ClassType.
-                ClassBase::Protocol | ClassBase::Generic => ConstraintSet::from(false),
+                ClassBase::Protocol | ClassBase::Generic | ClassBase::TypedDict => {
+                    ConstraintSet::from(false)
+                }
 
                 ClassBase::Class(base) => match (base, other) {
                     (ClassType::NonGeneric(base), ClassType::NonGeneric(other)) => {
@@ -589,11 +591,6 @@ impl<'db> ClassType<'db> {
                         ConstraintSet::from(false)
                     }
                 },
-
-                ClassBase::TypedDict => {
-                    // TODO: Implement subclassing and assignability for TypedDicts.
-                    ConstraintSet::from(true)
-                }
             }
         })
     }
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 85c645d37a..449e770aff 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -8007,25 +8007,26 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             // the `try_call` path below.
             // TODO: it should be possible to move these special cases into the `try_call_constructor`
             // path instead, or even remove some entirely once we support overloads fully.
-            let has_special_cased_constructor = matches!(
-                class.known(self.db()),
-                Some(
-                    KnownClass::Bool
-                        | KnownClass::Str
-                        | KnownClass::Type
-                        | KnownClass::Object
-                        | KnownClass::Property
-                        | KnownClass::Super
-                        | KnownClass::TypeAliasType
-                        | KnownClass::Deprecated
-                )
-            ) || (
-                // Constructor calls to `tuple` and subclasses of `tuple` are handled in `Type::Bindings`,
-                // but constructor calls to `tuple[int]`, `tuple[int, ...]`, `tuple[int, *tuple[str, ...]]` (etc.)
-                // are handled by the default constructor-call logic (we synthesize a `__new__` method for them
-                // in `ClassType::own_class_member()`).
-                class.is_known(self.db(), KnownClass::Tuple) && !class.is_generic()
-            );
+            let has_special_cased_constructor =
+                matches!(
+                    class.known(self.db()),
+                    Some(
+                        KnownClass::Bool
+                            | KnownClass::Str
+                            | KnownClass::Type
+                            | KnownClass::Object
+                            | KnownClass::Property
+                            | KnownClass::Super
+                            | KnownClass::TypeAliasType
+                            | KnownClass::Deprecated
+                    )
+                ) || (
+                    // Constructor calls to `tuple` and subclasses of `tuple` are handled in `Type::Bindings`,
+                    // but constructor calls to `tuple[int]`, `tuple[int, ...]`, `tuple[int, *tuple[str, ...]]` (etc.)
+                    // are handled by the default constructor-call logic (we synthesize a `__new__` method for them
+                    // in `ClassType::own_class_member()`).
+                    class.is_known(self.db(), KnownClass::Tuple) && !class.is_generic()
+                ) || CodeGeneratorKind::TypedDict.matches(self.db(), class.class_literal(self.db()).0, class.class_literal(self.db()).1);
 
             // temporary special-casing for all subclasses of `enum.Enum`
             // until we support the functional syntax for creating enum classes
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index c093dbef25..bafe798425 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -90,6 +90,16 @@ impl<'db> TypedDictType<'db> {
         relation_visitor: &HasRelationToVisitor<'db>,
         disjointness_visitor: &IsDisjointVisitor<'db>,
     ) -> ConstraintSet<'db> {
+        // First do a quick nominal check that (if it succeeds) means that we can avoid
+        // materializing the full `TypeDict` schema for either `self` or `target`.
+        // This should be cheaper in many cases, and also helps us avoid some cycles.
+        if self
+            .defining_class
+            .is_subclass_of(db, target.defining_class)
+        {
+            return ConstraintSet::from(true);
+        }
+
         let self_items = self.items(db);
         let target_items = target.items(db);
         // Many rules violations short-circuit with "never", but asking whether one field is
```

</details>

We still panic when checking pydantic, even with that patch applied, however, so @oconnor663 will have to find a new minimal repro if he applies the patch ;)

---

_Comment by @oconnor663 on 2025-11-19 17:10_

> I'm invoking ty on a file inside the Ruff repo -- possibly you're invoking ty on a file in a slightly different location, which means that it's using our default Python version (3.14, which has deferred annotations by default at runtime) instead of the resolved Python version from Ruff's pyproject.toml file (3.7).

Yes I should've said I'm just running all of these snippets out of `/tmp`, so I think everything is `ty` defaults.

---

_Comment by @oconnor663 on 2025-11-19 17:48_

Here's a new minimized repro (running as a standalone script out of `/tmp`), which still panics with @AlexWaygood's `has_special_cased_constructor` patch above:

```py
from typing import TypedDict

class Foo1(TypedDict):
    x1: MyUnion

class Foo2(TypedDict):
    x2: MyUnion

class Foo3(Foo2):
    pass

class Foo4(Foo2):
    x3: MyUnion

class Foo5(TypedDict):
    x5: MyUnion

MyUnion = Foo1 | Foo3 | Foo4 | Foo5
```
```
error[panic]: Panicked at /home/jacko/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:321:21 when checking `/tmp/core_schema.py`: `ClassLiteral < 'db >::fields_(Id(8803)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: ruff/0.14.5+62 (a2e803e65 2025-11-19)
info: Args: ["/home/jacko/astral/ruff/target-mold/debug/ty", "check", "core_schema.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: Type < 'db >::is_redundant_with_(Id(8407))
             at crates/ty_python_semantic/src/types.rs:821
   1: infer_definition_types(Id(1403))
             at crates/ty_python_semantic/src/types/infer.rs:94
             cycle heads: Type < 'db >::is_redundant_with_(Id(8400)) -> iteration = 200
   2: ClassLiteral < 'db >::fields_(Id(8800))
             at crates/ty_python_semantic/src/types/class.rs:1370
   3: Type < 'db >::is_redundant_with_(Id(8400))
             at crates/ty_python_semantic/src/types.rs:821
   4: infer_definition_types(Id(1401))
             at crates/ty_python_semantic/src/types/infer.rs:94
   5: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:70
   6: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535
```

It feels "pretty much the same" as the last one. What I'm noticing as I minimize these, is that there are a lot of "paths" I can take on the way down. I'll get a different result if I start ripping things out from the top vs from the bottom vs in the middle. Eventually I rip out something that removes the panic, so I put that thing back and roll the dice again on where to delete more stuff. So the end result is 1) one of many local minima I could wind up at and 2) maximally sensitive to the order of elements within the union. Which means that minor tweaks in how things are evaluated are likely to "fix" one of these minimal examples by coincidence without really fixing the underlying issue.

I'm going to try to follow @carljm and @MichaReiser's advice above and play around with logging this some more, to see if I can get some more intuition about what's going on.

---

_Comment by @oconnor663 on 2025-11-19 19:01_

Making some progress with printouts. (Finally figured out that a "cycle recovery function" is `cycle_fn`. There are only two such examples in `ty`?) Here's what the values of `.fields()` look like right before the crash:

```
...
Foo5 (iteration #195)
    field x5: [missing] --> Foo5
Foo4 (iteration #195)
    field x2: Foo1|Foo3|Foo4|Foo5 --> Foo1|Foo3|Foo5
    field x4: Foo1|Foo3|Foo5 --> Foo5
Foo3 (iteration #195)
    field x2: Foo1|Foo3|Foo5 --> Foo5
Foo5 (iteration #196)
    field x5: Foo5 --> Foo1|Foo3|Foo5
Foo4 (iteration #196)
    field x2: Foo1|Foo3|Foo5 --> Foo5
    field x4: Foo5 --> Foo1|Foo3|Foo4|Foo5
Foo3 (iteration #196)
    field x2: Foo5 --> Foo1|Foo3|Foo4|Foo5
Foo5 (iteration #197)
    field x5: Foo1|Foo3|Foo5 --> Foo1|Foo3|Foo4|Foo5
Foo4 (iteration #197)
    field x2: Foo5 --> Foo1|Foo3|Foo4|Foo5
Foo3 (iteration #197)
    NO CHANGES!
Foo4 (iteration #198)
    field x4: Foo1|Foo3|Foo4|Foo5 --> Foo1|Foo3|Foo5
Foo3 (iteration #198)
    field x2: Foo1|Foo3|Foo4|Foo5 --> Foo1|Foo3|Foo5
Foo5 (iteration #199)
    field x5: [missing] --> Foo5
Foo4 (iteration #199)
    field x2: Foo1|Foo3|Foo4|Foo5 --> Foo1|Foo3|Foo5
    field x4: Foo1|Foo3|Foo5 --> Foo5
Foo3 (iteration #199)
    field x2: Foo1|Foo3|Foo5 --> Foo5
```

Notable that the union keeps collapsing down to a single TypedDict type. More staring required...

---

_Comment by @carljm on 2025-11-19 19:18_

One thing that we could consider here that might help is to just never consider two named `TypedDict` types as redundant in a union, unless they are the exact same type. This would lead to less union simplification, but that might be fine, actually. And I suspect it would solve this problem.

---

_Comment by @oconnor663 on 2025-11-19 19:20_

I'll try that. Separately, I've been refining the printout above, and now I see that sometimes my `cycle_fn` is being called when the previous value and current value are equal. I'm surprised by that, since I thought that was exactly the condition that ends the cycling. Will ask in chat.

---

_Comment by @oconnor663 on 2025-11-19 19:23_

Also Alex mentioned at one point that he hasn't been able to get `Protocol` versions of examples like these to produce a similar panic. Not yet clear what's different between `TypedDict` and `Protocol` in this respect. I haven't looked into it yet.

---

_@oconnor663 reviewed on 2025-11-19 21:18_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/subscript/assignment_diagnostics.md`:98 on 2025-11-19 21:18_

Actually if we end up going with "don't generally simplify `Union`s of `TypedDict`s", I can put this test back the way it was.

---

_@AlexWaygood reviewed on 2025-11-19 21:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/subscript/assignment_diagnostics.md`:98 on 2025-11-19 21:20_

I think it's better for the test to pass whichever way we go on that, so this change seems good to me :-)

---

_Comment by @oconnor663 on 2025-11-19 21:26_

I think I'm seeing CI runs for later commits getting cancelled in favor of earlier commits, which doesn't make sense to me. Known problem? My fault for force-pushing something? In any case, I've manually restarted https://github.com/astral-sh/ruff/actions/runs/19516380284/job/55870895275, and I think that's the one to watch.

---

_Comment by @oconnor663 on 2025-11-19 21:56_

Ok, everything is passing, except that CodSpeed reports a ~10x slowdown on the Pydantic benchmark. I'm going to try to isolate what parts are costing us the most time, but at the same time I need feedback on whether this is "obviously a performance bug" or "maybe reasonable given how convoluted their unions are"?

---

_Comment by @carljm on 2025-11-19 22:04_

The magnitude of the regression is surprising to me. I definitely think we should explore how we can be more efficient here, but given that no other project regresses more than 1%, it's clearly a factor of the size and ubiquity of the typed dicts in pydantic; I wouldn't necessarily be opposed to going ahead and landing this and doing more optimization as a follow-up.

---

_@carljm reviewed on 2025-11-19 22:13_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/subscript/assignment_diagnostics.md`:98 on 2025-11-19 22:13_

Yeah I expect if we land more comprehensive cycle-panic avoidance, we might want to go back to simplifying typed dicts in unions.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:502 on 2025-11-19 22:18_

Should we also explicitly test the `total=False` but mutable field case?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:513 on 2025-11-19 22:19_

"match exactly" isn't a well defined term. "Equivalent" is, but I think that's not actually the right term here either; I think the right term here is "[consistent](https://typing.python.org/en/latest/spec/concepts.html#consistency)". Which is the same thing as "mutually assignable".

The types `int` and `Any`, for example, do not "match exactly" (at least not as I would intuitively define that term; it isn't defined in the typing spec), nor are they equivalent (as defined in the typing spec), but they are mutually assignable, and they are "consistent" as defined in the spec (`Any` can materialize to `int`).

And this code does not have type errors:

```py
from typing import Any, TypedDict

class A(TypedDict):
    x: Any

class B(TypedDict):
    x: int

def _(b: B) -> A:
    return b

def _(a: A) -> B:
    return a
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:632 on 2025-11-19 22:42_

Clearly we do want to support `closed` and `extra_items` at some point, but I don't think there is any TODO right at this spot. A TODO implies that our results on this code should be different, given better support. `Person` does not have `closed=True` or `extra_items`, so nothing we add support for in the future should change this error in this particular case. 

If we added a test using a TypedDict that actually defines `closed` or `extra_items` and tested its assignability to a non-universal `Mapping` type (and got the wrong result for now), that would provide occasion for a TODO comment.

This seems like a good spot for an explanatory comment, but not for a TODO comment.
```suggestion
# `Person` does not have `closed=True` or `extra_items`, so it may have additional keys with values
# of unknown type, therefore it can't be assigned to a `Mapping` with value type smaller than `object`.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:528 on 2025-11-19 22:54_

We import `Any` here, but it doesn't seem to be used in this block of tests.

I think we should add some tests with typeddicts with `Any` typed field(s), showing how that impacts subtyping and assignability. Assignability should be forgiving (as shown in the comment above); subtyping should be strict.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:1118 on 2025-11-19 22:56_

I would probably keep this as a TODO:
```suggestion
    # TODO: Should be `Person`; simplifying TypedDicts in Unions is pending better cycle handling
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:513 on 2025-11-20 00:04_

Looks like you got this all correct (and described well) in the comments in the actual implementation, so I think this is just tightening up the terminology here, and maybe adding some tests as mentioned below.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/typed_dict.rs`:141 on 2025-11-20 00:05_

Excellent comment!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/typed_dict.rs`:84 on 2025-11-20 00:07_

This is a really clear and well-documented implementation of some fairly hairy logic!

---

_@carljm approved on 2025-11-20 00:09_

Excellent work!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:757 on 2025-11-20 11:33_

nit: I'd personally find these more readable if we used the `static_assert(is_assignable_to(...))` pattern here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:795 on 2025-11-20 11:33_

same here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2834 on 2025-11-20 11:35_

nit: quite a long line

```suggestion
    #[salsa::tracked(
        returns(ref),
        cycle_initial=fields_cycle_initial,
        heap_size=get_size2::GetSize::get_heap_size)
    ]
```

---

_@AlexWaygood approved on 2025-11-20 12:03_

Great work!!

Since Carl already reviewed, I haven't done an exhaustive review of the semantics here vis-a-vis the spec -- just a few notes from me.

I think we should definitely look at improving perf here as a (post-beta?) followup. Aside from anything else, it's frustrating that the benchmarks CI job now takes 5 minutes longer than it used to 😆 But I do agree that we should land this now and move on; there's too much else to do before the beta.

I don't think you have any tests currently for generic typeddicts. It looks like everything works correctly on your branch, but it would be great to test it explicitly; something like this?

```py
from typing import TypedDict
from ty_extensions import static_assert, is_assignable_to, is_subtype_of

class F[T](TypedDict):
    x: T

class G[T](TypedDict):
    x: T

static_assert(is_assignable_to(F, G))

def x[T](a: T) -> T:
    static_assert(is_subtype_of(F[T], G[T]))
    return a

static_assert(is_subtype_of(F[int], G[int]))
static_assert(not is_assignable_to(F[int], G[str]))
```

Some tests that use the legacy syntax (multiple-inheriting from `TypedDict` and `Generic[T]`) would be great as well.

I ran the property tests on this branch and didn't see any issues. Though I don't think we include any `TypedDict` types in the property tests right now, so that's not a massive surprise 😆. We should probably open a followup issue for that -- again, that should probably be done post-beta.

---

_@sharkdp reviewed on 2025-11-20 13:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 13:56_

I just talked with Alex about the 10x performance regression on pydantic here, caused by the large union of `TypedDict`s (many of which are probably *not* subtypes of each other). When we build that union, we need to perform O(n²) subtyping checks, so it's understandable that we see a huge regression now that we actually do nontrivial work in those checks. Other than the nominal check here (which is probably not *super* fast, actually), are there any other short circuit paths where we could return `false` early? The most common case might be something like the following where two "normal" `TypedDict`s (no `extra_items` or similar) are simply incompatible because their key names are not compatible:
```py
class A(TypedDict):
    key_a: int

class B(TypedDict):
    key_b: int
```
Is there something we can do by just looking at the names, without ever looking at the value types?

---

_@AlexWaygood reviewed on 2025-11-20 14:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 14:53_

I tried out something like this, and it passes all tests, but doesn't seem to lead to any speedup sadly:

```diff
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index bafe798425..6f638c9817 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -102,6 +102,21 @@ impl<'db> TypedDictType<'db> {
 
         let self_items = self.items(db);
         let target_items = target.items(db);
+
+        // For `self` to be a subtype of `target`,
+        // all `target` fields must be present in `self` unless
+        // the target field specifically has the form `NotRequired[ReadOnly[object]]`.
+        // We therefore do a quick check for missing keys here before doing the full
+        // (expensive) relation checks below.
+        for (name, field) in target_items {
+            if field.is_read_only() && !field.is_required() && field.declared_ty.is_object() {
+                continue;
+            }
+            if !self_items.contains_key(name) {
+                return ConstraintSet::from(false);
+            }
+        }
+
         // Many rules violations short-circuit with "never", but asking whether one field is
         // [relation] to/of another can produce more complicated constraints, and we collect those.
         let mut constraints = ConstraintSet::from(true);
```

---

_@sharkdp reviewed on 2025-11-20 14:58_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 14:58_

Did you verify that this early return actually triggers on the pydantic code? If so, is it possible that the nominal subtyping check above is expensive?

---

_@AlexWaygood reviewed on 2025-11-20 15:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 15:02_

> Did you verify that this early return actually triggers on the pydantic code? 

No, I didn't

> If so, is it possible that the nominal subtyping check above is expensive?

That's certainly possible... even if it is expensive, I think it _may_ still be worth doing to reduce the chance of pathological cycles for recursive typeddicts. But that's just speculation.

---

_@AlexWaygood reviewed on 2025-11-20 15:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 15:09_

I just tried out this patch, which also did not speed things up materially:

```diff
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index bafe798425..c086eb4c11 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -90,18 +90,23 @@ impl<'db> TypedDictType<'db> {
         relation_visitor: &HasRelationToVisitor<'db>,
         disjointness_visitor: &IsDisjointVisitor<'db>,
     ) -> ConstraintSet<'db> {
-        // First do a quick nominal check that (if it succeeds) means that we can avoid
-        // materializing the full `TypeDict` schema for either `self` or `target`.
-        // This should be cheaper in many cases, and also helps us avoid some cycles.
-        if self
-            .defining_class
-            .is_subclass_of(db, target.defining_class)
-        {
-            return ConstraintSet::from(true);
-        }
-
         let self_items = self.items(db);
         let target_items = target.items(db);
+
+        // For `self` to be a subtype of `target`,
+        // all `target` fields must be present in `self` unless
+        // the target field specifically has the form `NotRequired[ReadOnly[object]]`.
+        // We therefore do a quick check for missing keys here before doing the full
+        // (expensive) relation checks below.
+        for (name, field) in target_items {
+            if field.is_read_only() && !field.is_required() && field.declared_ty.is_object() {
+                continue;
+            }
+            if !self_items.contains_key(name) {
+                return ConstraintSet::from(false);
+            }
+        }
+
         // Many rules violations short-circuit with "never", but asking whether one field is
         // [relation] to/of another can produce more complicated constraints, and we collect those.
         let mut constraints = ConstraintSet::from(true);
```

and adding Salsa caching to `Type::is_assignable_to` also didn't help. The benchmark stays stubbornly in the 19s-20s range for me locally whatever I do.

---

_@sharkdp reviewed on 2025-11-20 15:31_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 15:31_

This seems to imply that `ClassType::is_subclass_of` is actually quite expensive:

<img width="1106" height="347" alt="image" src="https://github.com/user-attachments/assets/3e0d40c7-8523-4639-8d85-ad8eac53ba8b" />


---

_@AlexWaygood reviewed on 2025-11-20 15:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 15:54_

I tried out these patches, and they also yielded no significant speedup (runtime still in the 19s-20s range for me locally):

```diff
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index bafe798425..c093dbef25 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -90,16 +90,6 @@ impl<'db> TypedDictType<'db> {
         relation_visitor: &HasRelationToVisitor<'db>,
         disjointness_visitor: &IsDisjointVisitor<'db>,
     ) -> ConstraintSet<'db> {
-        // First do a quick nominal check that (if it succeeds) means that we can avoid
-        // materializing the full `TypeDict` schema for either `self` or `target`.
-        // This should be cheaper in many cases, and also helps us avoid some cycles.
-        if self
-            .defining_class
-            .is_subclass_of(db, target.defining_class)
-        {
-            return ConstraintSet::from(true);
-        }
-
         let self_items = self.items(db);
         let target_items = target.items(db);
         // Many rules violations short-circuit with "never", but asking whether one field is
```

and

```diff
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index bafe798425..c8df7842c9 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -4,6 +4,7 @@ use ruff_db::parsed::parsed_module;
 use ruff_python_ast::Arguments;
 use ruff_python_ast::{self as ast, AnyNodeRef, StmtClassDef, name::Name};
 use ruff_text_size::Ranged;
+use rustc_hash::FxHashSet;
 
 use super::class::{ClassType, CodeGeneratorKind, Field};
 use super::context::InferContext;
@@ -12,9 +13,13 @@ use super::diagnostic::{
     report_missing_typed_dict_key,
 };
 use super::{ApplyTypeMappingVisitor, Type, TypeMapping, visitor};
+use crate::semantic_index::{place_table, use_def_map};
 use crate::types::constraints::ConstraintSet;
 use crate::types::generics::InferableTypeVars;
-use crate::types::{HasRelationToVisitor, IsDisjointVisitor, TypeContext, TypeRelation};
+use crate::types::{
+    ClassBase, ClassLiteral, DefinitionKind, HasRelationToVisitor, IsDisjointVisitor, TypeContext,
+    TypeRelation,
+};
 use crate::{Db, FxIndexMap};
 
 use ordermap::OrderSet;
@@ -79,6 +84,38 @@ impl<'db> TypedDictType<'db> {
         }
     }
 
+    /// Check whether this `TypedDict` has a field with the given name.
+    /// **without inferring the type of that field**
+    fn has_field(self, db: &'db dyn Db, field_name: &str) -> bool {
+        #[salsa::tracked(heap_size=ruff_memory_usage::heap_size)]
+        fn defined_fields<'db>(db: &'db dyn Db, class: ClassLiteral<'db>) -> FxHashSet<Name> {
+            class
+                .iter_mro(db, None)
+                .filter_map(ClassBase::into_class)
+                .flat_map(|class| {
+                    let scope = class.class_literal(db).0.body_scope(db);
+                    use_def_map(db, scope)
+                        .all_end_of_scope_symbol_declarations()
+                        .filter_map(move |(symbol_id, mut declarations)| {
+                            if !declarations.all(|declaration| {
+                                declaration.declaration.is_undefined_or(|declaration| {
+                                    matches!(
+                                        declaration.kind(db),
+                                        DefinitionKind::AnnotatedAssignment(..)
+                                    )
+                                })
+                            }) {
+                                return None;
+                            }
+                            Some(place_table(db, scope).symbol(symbol_id).name().clone())
+                        })
+                })
+                .collect()
+        }
+
+        defined_fields(db, self.defining_class.class_literal(db).0).contains(field_name)
+    }
+
     // Subtyping between `TypedDict`s follows the algorithm described at:
     // https://typing.python.org/en/latest/spec/typeddict.html#subtyping-between-typeddict-types
     pub(super) fn has_relation_to_impl(
@@ -90,18 +127,24 @@ impl<'db> TypedDictType<'db> {
         relation_visitor: &HasRelationToVisitor<'db>,
         disjointness_visitor: &IsDisjointVisitor<'db>,
     ) -> ConstraintSet<'db> {
-        // First do a quick nominal check that (if it succeeds) means that we can avoid
-        // materializing the full `TypeDict` schema for either `self` or `target`.
-        // This should be cheaper in many cases, and also helps us avoid some cycles.
-        if self
-            .defining_class
-            .is_subclass_of(db, target.defining_class)
-        {
-            return ConstraintSet::from(true);
+        let target_items = target.items(db);
+
+        // For `self` to be a subtype of `target`,
+        // all `target` fields must be present in `self` unless
+        // the target field specifically has the form `NotRequired[ReadOnly[object]]`.
+        // We therefore do a quick check for missing keys here before doing the full
+        // (expensive) relation checks below.
+        for (name, field) in target_items {
+            if field.is_read_only() && !field.is_required() && field.declared_ty.is_object() {
+                continue;
+            }
+            if !self.has_field(db, name) {
+                return ConstraintSet::from(false);
+            }
         }
 
         let self_items = self.items(db);
-        let target_items = target.items(db);
+
         // Many rules violations short-circuit with "never", but asking whether one field is
         // [relation] to/of another can produce more complicated constraints, and we collect those.
         let mut constraints = ConstraintSet::from(true);

```

---

_@carljm reviewed on 2025-11-20 18:12_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 18:12_

> When we build that union, we need to perform O(n²) subtyping checks

Except I think the current version of this PR does not attempt to simplify `TypedDict` types in unions at all? (This is how we got around the cycle issues for now.) So it doesn't seem like union simplification can be the explanation for the regression?

Also, since we salsa-cache `is_redundant_with` judgments, I think the overall blowup from this sort of issue is limited by the total number of `TypedDict` types involved in all the unions? That is, with `N` total `TypedDict` types, even if did check them for redundancy in unions, we would do at most N^2 (full, uncached) redundancy comparisons (ever), no matter how many unions of those TypedDicts are built.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 18:13_

Given that the expense in the profile above looks to be inside `is_assignable_to` (not `is_subtype_of` or `is_redundant_with`), it may be worth trying to Salsa cache `is_assignable_to` again?

I know we tried that on an earlier version of this PR and it didn't fix the even-more-massive blowup then -- but that was solved by caching `fields`. It seems like the current smaller blowup might be helped by caching `is_assignable_to`.

---

_@carljm reviewed on 2025-11-20 18:13_

---

_@AlexWaygood reviewed on 2025-11-20 18:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 18:16_

> Given that the expense in the profile above looks to be inside `is_assignable_to` (not `is_subtype_of` or `is_redundant_with`), it may be worth trying to Salsa cache `is_assignable_to` again?

As I said above in https://github.com/astral-sh/ruff/pull/21467#discussion_r2546458561, I tried that _earlier today_ and it didn't yield any speedup

---

_@carljm reviewed on 2025-11-20 18:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 18:19_

Ah sorry, I missed that brief note after the big code diff...

---

_@carljm reviewed on 2025-11-20 18:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 18:24_

Maybe caching `is_subclass_of` directly?

---

_@oconnor663 reviewed on 2025-11-20 18:37_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:757 on 2025-11-20 18:37_

Done and added `is_subtype_of` here as well.

---

_@oconnor663 reviewed on 2025-11-20 20:24_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/typed_dict.rs`:93 on 2025-11-20 20:24_

I'm measuring ~no performance impact from this diff on Pydantic :(

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index fc3b9d4227..93827ae16b 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -524,6 +524,7 @@ impl<'db> ClassType<'db> {
     }
 
     /// Return `true` if `other` is present in this class's MRO.
+    #[salsa::tracked]
     pub(super) fn is_subclass_of(self, db: &'db dyn Db, other: ClassType<'db>) -> bool {
         self.when_subclass_of(db, other, InferableTypeVars::None)
             .is_always_satisfied(db)
@@ -1758,6 +1759,7 @@ impl<'db> ClassLiteral<'db> {
     }
 
     /// Return `true` if `other` is present in this class's MRO.
+    #[salsa::tracked]
     pub(super) fn is_subclass_of(
         self,
         db: &'db dyn Db,
```

I think I've addressed all the non-perf comments, and CI is green except for CodSpeed (which doesn't list any other regressions besides Pydantic), so if there are no objections I'll go ahead and land this as-is and defer the rest of the perf work until after Beta.

---

_@AlexWaygood reviewed on 2025-11-20 20:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:567 on 2025-11-20 20:42_

lol, I sorta prefer Black's formatting 😆

---

_Comment by @AlexWaygood on 2025-11-20 20:43_

> I think I've addressed all the non-perf comments

Could you possibly add some tests for generic typeddicts (I gave some suggestions in https://github.com/astral-sh/ruff/pull/21467#pullrequestreview-3487328376)? But otherwise, yes, I think let's get this in 👍

---

_@oconnor663 reviewed on 2025-11-20 20:46_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:567 on 2025-11-20 20:46_

Black's formatting looks nicer if you're scrolling past the code, but I think this version is better if you're actually trying to read it. (Or more to the point for me, if you're trying to edit it.)

---

_@AlexWaygood reviewed on 2025-11-20 20:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:567 on 2025-11-20 20:48_

Editing -- yes. Reading -- I disagree. But I also don't feel strongly; don't feel you need to change this just to keep me happy :-)

---

_Comment by @oconnor663 on 2025-11-20 20:54_

Woops, thanks for catching. Added structural assignability to the existing mdtest sections for both class-style and legacy generic typeddicts.

---

_Merged by @oconnor663 on 2025-11-20 21:15_

---

_Closed by @oconnor663 on 2025-11-20 21:15_

---

_Branch deleted on 2025-11-20 21:15_

---
