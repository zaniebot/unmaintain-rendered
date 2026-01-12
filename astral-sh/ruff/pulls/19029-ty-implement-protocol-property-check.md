```yaml
number: 19029
title: "[ty] Implement protocol property check"
type: pull_request
state: open
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: protocol-property-check
created_at: 2025-06-29T16:31:03Z
updated_at: 2025-08-26T15:56:53Z
url: https://github.com/astral-sh/ruff/pull/19029
synced_at: 2026-01-12T15:56:30Z
```

# [ty] Implement protocol property check

---

_@mtshiba_

## Summary

This is a follow-up to #18847 and implements checks on protocol property members.

Since the logic implemented in `TypeInferenceBuilder::validate_attribute_assignment` was required to check setters, I extracted the judgment part as a `Type` method. As a side effect, the diagnostics emission part and the judgment part are now separated, which I think has improved readability a little. Additionally, some error messages have been consolidated to make them more consistent.

## Test Plan

Some of the TODOs in `protocols.md` now work properly.


---

_Review requested from @carljm by @mtshiba on 2025-06-29 16:31_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-06-29 16:31_

---

_Review requested from @sharkdp by @mtshiba on 2025-06-29 16:31_

---

_Review requested from @dcreager by @mtshiba on 2025-06-29 16:31_

---

_Comment by @github-actions[bot] on 2025-06-29 16:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_next_gen.py:71:13: error[invalid-assignment] Object of type `Literal["1"]` is not assignable to attribute `x` of type `int`
+ tests/test_next_gen.py:71:13: error[invalid-assignment] Object of type `Literal["1"]` is not assignable to attribute `x` on type `int`
- tests/test_next_gen.py:387:9: error[invalid-assignment] Object of type `Literal["11"]` is not assignable to attribute `x` of type `int`
+ tests/test_next_gen.py:387:9: error[invalid-assignment] Object of type `Literal["11"]` is not assignable to attribute `x` on type `int`
- tests/test_next_gen.py:393:13: error[invalid-assignment] Object of type `Literal["9"]` is not assignable to attribute `x` of type `int`
+ tests/test_next_gen.py:393:13: error[invalid-assignment] Object of type `Literal["9"]` is not assignable to attribute `x` on type `int`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/utils.py:512:9: error[invalid-assignment] Object of type `(int & ~((...) -> object)) | (((str | None, /) -> int | None) & ~((...) -> object)) | (@Todo(Type::Intersection.call()) & ~None)` is not assignable to attribute `max_age` of type `int | None`
+ src/werkzeug/utils.py:512:9: error[invalid-assignment] Object of type `(int & ~((...) -> object)) | (((str | None, /) -> int | None) & ~((...) -> object)) | (@Todo(Type::Intersection.call()) & ~None)` is not assignable to attribute `max_age` on type `int | None`
- tests/test_datastructures.py:1009:9: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `no_cache` of type `str | Literal[True] | None`
+ tests/test_datastructures.py:1009:9: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `no_cache` on type `str | Literal[True] | None`
- tests/test_http.py:141:9: error[invalid-assignment] Object of type `float` is not assignable to attribute `max_age` of type `int | None`
+ tests/test_http.py:141:9: error[invalid-assignment] Object of type `float` is not assignable to attribute `max_age` on type `int | None`
- tests/test_http.py:144:9: error[invalid-assignment] Object of type `float` is not assignable to attribute `s_maxage` of type `int | None`
+ tests/test_http.py:144:9: error[invalid-assignment] Object of type `float` is not assignable to attribute `s_maxage` on type `int | None`
- tests/test_security.py:66:5: error[invalid-assignment] Object of type `Literal["*"]` is not assignable to attribute `_os_alt_seps` of type `list[str]`
+ tests/test_security.py:66:5: error[invalid-assignment] Object of type `Literal["*"]` is not assignable to attribute `_os_alt_seps` on type `list[str]`

comtypes (https://github.com/enthought/comtypes)
- comtypes/client/_code_cache.py:121:13: error[invalid-assignment] Object of type `ModuleType` is not assignable to attribute `gen` of type `<module 'comtypes.gen'>`
+ comtypes/client/_code_cache.py:121:13: error[invalid-assignment] Object of type `ModuleType` is not assignable to attribute `gen` on type `<module 'comtypes.gen'>`

python-htmlgen (https://github.com/srittau/python-htmlgen)
- test_htmlgen/form.py:51:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `autocomplete` of type `Autocomplete | None`
+ test_htmlgen/form.py:51:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `autocomplete` on type `Autocomplete | None`
- test_htmlgen/video.py:38:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `preload` of type `Preload | None`
+ test_htmlgen/video.py:38:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `preload` on type `Preload | None`

rich (https://github.com/Textualize/rich)
- tests/test_pretty.py:184:5: error[invalid-assignment] Object of type `ExampleDataclass` is not assignable to attribute `bar` of type `str`
+ tests/test_pretty.py:184:5: error[invalid-assignment] Object of type `ExampleDataclass` is not assignable to attribute `bar` on type `str`

optuna (https://github.com/optuna/optuna)
- optuna/_gp/search_space.py:181:37: error[invalid-argument-type] Argument to function `_round_one_normalized_param` is incorrect: Expected `_ScaleType`, found `Unknown | ndarray[Unknown, Unknown]`
+ optuna/_gp/search_space.py:181:37: error[invalid-argument-type] Argument to function `_round_one_normalized_param` is incorrect: Expected `_ScaleType`, found `Unknown | ndarray[Unknown, <class 'signedinteger[_64Bit]'>]`
- tests/gp_tests/test_search_space.py:108:13: error[invalid-argument-type] Argument to function `_unnormalize_one_param` is incorrect: Expected `_ScaleType`, found `Unknown | ndarray[Unknown, Unknown]`
+ tests/gp_tests/test_search_space.py:108:13: error[invalid-argument-type] Argument to function `_unnormalize_one_param` is incorrect: Expected `_ScaleType`, found `Unknown | ndarray[Unknown, <class 'signedinteger[_64Bit]'>]`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `validator` on type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `serializer` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `serializer` on type `SchemaSerializer`
- pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_validator__` on type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_serializer__` on type `SchemaSerializer`

cloud-init (https://github.com/canonical/cloud-init)
- conftest.py:213:5: error[invalid-assignment] Object of type `def my_system_info() -> Unknown` is not assignable to attribute `system_info` of type `_lru_cache_wrapper[Unknown]`
+ conftest.py:213:5: error[invalid-assignment] Object of type `def my_system_info() -> Unknown` is not assignable to attribute `system_info` on type `_lru_cache_wrapper[Unknown]`
- tests/unittests/sources/test_init.py:759:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `network_json` of type `str | None`
+ tests/unittests/sources/test_init.py:759:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `network_json` on type `str | None`
- tests/unittests/sources/test_init.py:778:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `network_json` of type `str | None`
+ tests/unittests/sources/test_init.py:778:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `network_json` on type `str | None`

asynq (https://github.com/quora/asynq)
- asynq/debug.py:324:5: error[invalid-assignment] Object of type `Unknown | None` is not assignable to attribute `excepthook` of type `(type[BaseException], BaseException, TracebackType | None, /) -> Any`
+ asynq/debug.py:324:5: error[invalid-assignment] Object of type `Unknown | None` is not assignable to attribute `excepthook` on type `(type[BaseException], BaseException, TracebackType | None, /) -> Any`

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/structure/page_tests.py:66:9: error[invalid-assignment] Object of type `Literal["foo"]` is not assignable to attribute `parent` of type `Section | None`
+ mkdocs/tests/structure/page_tests.py:66:9: error[invalid-assignment] Object of type `Literal["foo"]` is not assignable to attribute `parent` on type `Section | None`
- mkdocs/tests/structure/page_tests.py:138:9: error[invalid-assignment] Object of type `Literal["foo"]` is not assignable to attribute `parent` of type `Section | None`
+ mkdocs/tests/structure/page_tests.py:138:9: error[invalid-assignment] Object of type `Literal["foo"]` is not assignable to attribute `parent` on type `Section | None`

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/lib/interfaces/storage_utest.py:43:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `storage` of type `<module 'dragonchain.lib.interfaces.aws.s3'> | <module 'dragonchain.lib.interfaces.local.disk'>`
+ dragonchain/lib/interfaces/storage_utest.py:43:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `storage` on type `<module 'dragonchain.lib.interfaces.aws.s3'> | <module 'dragonchain.lib.interfaces.local.disk'>`

pylox (https://github.com/sco1/pylox)
- pylox/builtins/py_builtins.py:21:5: error[invalid-assignment] Object of type `deque[Unknown]` is not assignable to attribute `fields` of type `dict[Unknown, Unknown]`
+ pylox/builtins/py_builtins.py:21:5: error[invalid-assignment] Object of type `deque[Unknown]` is not assignable to attribute `fields` on type `dict[Unknown, Unknown]`
- pylox/containers/array.py:133:9: error[invalid-assignment] Object of type `deque[Unknown]` is not assignable to attribute `fields` of type `dict[Unknown, Unknown]`
+ pylox/containers/array.py:133:9: error[invalid-assignment] Object of type `deque[Unknown]` is not assignable to attribute `fields` on type `dict[Unknown, Unknown]`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/util/ssl_.py:301:9: error[invalid-assignment] Object of type `int | (Unknown & ~None)` is not assignable to attribute `minimum_version` of type `TLSVersion`
+ src/urllib3/util/ssl_.py:301:9: error[invalid-assignment] Object of type `int | (Unknown & ~None)` is not assignable to attribute `minimum_version` on type `TLSVersion`
- src/urllib3/util/ssl_.py:306:9: error[invalid-assignment] Object of type `int | (Unknown & ~None)` is not assignable to attribute `maximum_version` of type `TLSVersion`
+ src/urllib3/util/ssl_.py:306:9: error[invalid-assignment] Object of type `int | (Unknown & ~None)` is not assignable to attribute `maximum_version` on type `TLSVersion`
- src/urllib3/util/ssl_.py:356:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `verify_mode` of type `VerifyMode`
+ src/urllib3/util/ssl_.py:356:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `verify_mode` on type `VerifyMode`
- src/urllib3/util/ssl_.py:360:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `verify_mode` of type `VerifyMode`
+ src/urllib3/util/ssl_.py:360:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `verify_mode` on type `VerifyMode`
- test/conftest.py:387:13: error[invalid-assignment] Object of type `None | <class 'HTTPConnection'>` is not assignable to attribute `ConnectionCls` of type `type[@Todo(type[T] for protocols)]`
+ test/conftest.py:387:13: error[invalid-assignment] Object of type `None | <class 'HTTPConnection'>` is not assignable to attribute `ConnectionCls` on type `type[@Todo(type[T] for protocols)]`

PyGithub (https://github.com/PyGithub/PyGithub)
- github/GithubObject.py:309:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/GithubObject.py:316:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ github/GithubObject.py:319:24: error[invalid-return-type] Return type does not match returned value: expected `Attribute[K@__makeTransformedAttribute]`, found `_ValuedAttribute[K@__makeTransformedAttribute]`
- github/GithubObject.py:376:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/GithubObject.py:390:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/GithubObject.py:402:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 308 diagnostics
+ Found 304 diagnostics

jinja (https://github.com/pallets/jinja)
- tests/test_bytecode_cache.py:52:9: error[invalid-assignment] Object of type `Literal["code"]` is not assignable to attribute `code` of type `CodeType | None`
+ tests/test_bytecode_cache.py:52:9: error[invalid-assignment] Object of type `Literal["code"]` is not assignable to attribute `code` on type `CodeType | None`
- tests/test_bytecode_cache.py:66:9: error[invalid-assignment] Object of type `Literal["code"]` is not assignable to attribute `code` of type `CodeType | None`
+ tests/test_bytecode_cache.py:66:9: error[invalid-assignment] Object of type `Literal["code"]` is not assignable to attribute `code` on type `CodeType | None`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_downloadermiddleware_httpproxy.py:29:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `environ` of type `_Environ[str]`
+ tests/test_downloadermiddleware_httpproxy.py:29:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `environ` on type `_Environ[str]`

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/tests/test_ref_resolver.py:132:9: error[invalid-assignment] Object of type `Literal["linux2"]` is not assignable to attribute `platform` of type `Literal["linux"]`
+ schema_salad/tests/test_ref_resolver.py:132:9: error[invalid-assignment] Object of type `Literal["linux2"]` is not assignable to attribute `platform` on type `Literal["linux"]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/optimize/analysis/lookahead.py:127:9: error[invalid-assignment] Object of type `dict[str, DataFrame]` is not assignable to attribute `data` of type `DataFrame`
+ freqtrade/optimize/analysis/lookahead.py:127:9: error[invalid-assignment] Object of type `dict[str, DataFrame]` is not assignable to attribute `data` on type `DataFrame`
- freqtrade/optimize/analysis/recursive.py:151:9: error[invalid-assignment] Object of type `dict[str, DataFrame]` is not assignable to attribute `data` of type `DataFrame`
+ freqtrade/optimize/analysis/recursive.py:151:9: error[invalid-assignment] Object of type `dict[str, DataFrame]` is not assignable to attribute `data` on type `DataFrame`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/server/contexts.py:233:13: error[invalid-assignment] Object of type `ReferenceType[BokehSessionContext]` is not assignable to attribute `_session_context` of type `ReferenceType[SessionContext] | None`
+ src/bokeh/server/contexts.py:233:13: error[invalid-assignment] Object of type `ReferenceType[BokehSessionContext]` is not assignable to attribute `_session_context` on type `ReferenceType[SessionContext] | None`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/addons/contentview-interactive.py:20:18: error[invalid-argument-type] Argument to function `add` is incorrect: Expected `Contentview`, found `<class 'InteractiveSwapCase'>`
- examples/addons/contentview.py:15:18: error[invalid-argument-type] Argument to function `add` is incorrect: Expected `Contentview`, found `<class 'SwapCase'>`
- examples/contrib/search.py:58:17: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `marked` of type `str`
+ examples/contrib/search.py:58:17: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `marked` on type `str`
- examples/contrib/webscanner_helper/test_watchdog.py:56:9: error[invalid-assignment] Object of type `Literal["Test Error"]` is not assignable to attribute `error` of type `Error | None`
+ examples/contrib/webscanner_helper/test_watchdog.py:56:9: error[invalid-assignment] Object of type `Literal["Test Error"]` is not assignable to attribute `error` on type `Error | None`
- test/mitmproxy/addons/test_clientplayback.py:182:5: error[invalid-assignment] Object of type `None` is not assignable to attribute `request` of type `Request`
+ test/mitmproxy/addons/test_clientplayback.py:182:5: error[invalid-assignment] Object of type `None` is not assignable to attribute `request` on type `Request`
- test/mitmproxy/addons/test_proxyauth.py:243:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `is_replay` of type `str | None`
+ test/mitmproxy/addons/test_proxyauth.py:243:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `is_replay` on type `str | None`
- test/mitmproxy/contentviews/test__registry.py:67:23: error[invalid-argument-type] Argument to bound method `register` is incorrect: Expected `Contentview`, found `<class 'ExampleContentview'>`
- test/mitmproxy/proxy/layers/http/test_http3.py:564:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `stream` of type `((bytes, /) -> Iterable[bytes]) | bool`
+ test/mitmproxy/proxy/layers/http/test_http3.py:564:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `stream` on type `((bytes, /) -> Iterable[bytes]) | bool`
- test/mitmproxy/test_flow.py:39:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `marked` of type `str`
+ test/mitmproxy/test_flow.py:39:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `marked` on type `str`
- Found 1825 diagnostics
+ Found 1822 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/api/api.py:252:9: error[invalid-assignment] Object of type `str | None` is not assignable to attribute `cluster` of type `str`
+ paasta_tools/api/api.py:252:9: error[invalid-assignment] Object of type `str | None` is not assignable to attribute `cluster` on type `str`
- paasta_tools/cli/cmds/mark_for_deployment.py:1925:21: error[invalid-assignment] Object of type `None` is not assignable to attribute `waiting_on` of type `Mapping[str, Collection[str]]`
+ paasta_tools/cli/cmds/mark_for_deployment.py:1925:21: error[invalid-assignment] Object of type `None` is not assignable to attribute `waiting_on` on type `Mapping[str, Collection[str]]`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/transforms/i18n.py:88:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `current_source` of type `str`
+ sphinx/transforms/i18n.py:88:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `current_source` on type `str`
- sphinx/transforms/i18n.py:88:30: error[invalid-assignment] Object of type `None` is not assignable to attribute `current_line` of type `str`
+ sphinx/transforms/i18n.py:88:30: error[invalid-assignment] Object of type `None` is not assignable to attribute `current_line` on type `str`
- sphinx/util/docutils.py:891:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `current_source` of type `str`
+ sphinx/util/docutils.py:891:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `current_source` on type `str`
- sphinx/util/docutils.py:891:35: error[invalid-assignment] Object of type `None` is not assignable to attribute `current_line` of type `str`
+ sphinx/util/docutils.py:891:35: error[invalid-assignment] Object of type `None` is not assignable to attribute `current_line` on type `str`

meson (https://github.com/mesonbuild/meson)
- run_project_tests.py:557:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to attribute `class_cmakeinfo` of type `PerMachine[CMakeInfo | None]`
+ run_project_tests.py:557:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to attribute `class_cmakeinfo` on type `PerMachine[CMakeInfo | None]`
- run_project_tests.py:558:5: error[invalid-assignment] Object of type `PerMachine[bool]` is not assignable to attribute `class_impl` of type `PerMachine[Literal[False] | PkgConfigInterface | None]`
+ run_project_tests.py:558:5: error[invalid-assignment] Object of type `PerMachine[bool]` is not assignable to attribute `class_impl` on type `PerMachine[Literal[False] | PkgConfigInterface | None]`
- run_project_tests.py:559:5: error[invalid-assignment] Object of type `PerMachine[bool]` is not assignable to attribute `class_cli_impl` of type `PerMachine[Literal[False] | PkgConfigCLI | None]`
+ run_project_tests.py:559:5: error[invalid-assignment] Object of type `PerMachine[bool]` is not assignable to attribute `class_cli_impl` on type `PerMachine[Literal[False] | PkgConfigCLI | None]`
- run_project_tests.py:560:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to attribute `pkg_bin_per_machine` of type `PerMachine[ExternalProgram | None]`
+ run_project_tests.py:560:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to attribute `pkg_bin_per_machine` on type `PerMachine[ExternalProgram | None]`
- run_tests.py:141:5: error[invalid-assignment] Object of type `None` is not assignable to attribute `cross_file` of type `list[str]`
+ run_tests.py:141:5: error[invalid-assignment] Object of type `None` is not assignable to attribute `cross_file` on type `list[str]`
- unittests/internaltests.py:461:9: error[invalid-assignment] Object of type `tuple[str]` is not assignable to attribute `cross_file` of type `list[str]`
+ unittests/internaltests.py:461:9: error[invalid-assignment] Object of type `tuple[str]` is not assignable to attribute `cross_file` on type `list[str]`
- unittests/internaltests.py:476:9: error[invalid-assignment] Object of type `tuple[str]` is not assignable to attribute `cross_file` of type `list[str]`
+ unittests/internaltests.py:476:9: error[invalid-assignment] Object of type `tuple[str]` is not assignable to attribute `cross_file` on type `list[str]`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/server/api/deployments.py:247:13: error[invalid-assignment] Object of type `None` is not assignable to attribute `schedules` of type `list[DeploymentScheduleUpdate]`
+ src/prefect/server/api/deployments.py:247:13: error[invalid-assignment] Object of type `None` is not assignable to attribute `schedules` on type `list[DeploymentScheduleUpdate]`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/settings/errortracking.py:44:5: error[invalid-assignment] Object of type `Unknown | EnvVariable[list[Unknown]]` is not assignable to attribute `_configured_modules` of type `list[str]`
+ ddtrace/settings/errortracking.py:44:5: error[invalid-assignment] Object of type `Unknown | EnvVariable[list[Unknown]]` is not assignable to attribute `_configured_modules` on type `list[str]`
- tests/smoke_test.py:58:13: error[invalid-assignment] Object of type `dict[str, str]` is not assignable to attribute `environ` of type `_Environ[str]`
+ tests/smoke_test.py:58:13: error[invalid-assignment] Object of type `dict[str, str]` is not assignable to attribute `environ` on type `_Environ[str]`

zulip (https://github.com/zulip/zulip)
- zerver/decorator.py:886:17: error[invalid-assignment] Object of type `QueryDict` is not assignable to attribute `POST` of type `_ImmutableQueryDict`
+ zerver/decorator.py:886:17: error[invalid-assignment] Object of type `QueryDict` is not assignable to attribute `POST` on type `_ImmutableQueryDict`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/tests/fixtures/messages.py:65:9: error[invalid-assignment] Object of type `MockRotkiNotifier` is not assignable to attribute `rotki_notifier` of type `RotkiNotifier | None`
+ rotkehlchen/tests/fixtures/messages.py:65:9: error[invalid-assignment] Object of type `MockRotkiNotifier` is not assignable to attribute `rotki_notifier` on type `RotkiNotifier | None`
- rotkehlchen/tests/fixtures/messages.py:73:9: error[invalid-assignment] Object of type `MockRotkiNotifier` is not assignable to attribute `rotki_notifier` of type `RotkiNotifier | None`
+ rotkehlchen/tests/fixtures/messages.py:73:9: error[invalid-assignment] Object of type `MockRotkiNotifier` is not assignable to attribute `rotki_notifier` on type `RotkiNotifier | None`

manticore (https://github.com/trailofbits/manticore)
- tests/native/test_memory.py:1875:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `sys` of type `<module 'sys'>`
+ tests/native/test_memory.py:1875:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `sys` on type `<module 'sys'>`

sympy (https://github.com/sympy/sympy)
+ sympy/polys/series/ring.py:54:9: error[invalid-assignment] Object of type `PythonPowerSeriesRingZZ` is not assignable to `PowerSeriesRingProto[@Todo(unknown type subscript), MPZ]`
+ sympy/polys/series/ring.py:59:9: error[invalid-assignment] Object of type `FlintPowerSeriesRingZZ` is not assignable to `PowerSeriesRingProto[Unknown, MPZ]`
+ sympy/polys/series/ring.py:65:9: error[invalid-assignment] Object of type `PythonPowerSeriesRingQQ` is not assignable to `PowerSeriesRingProto[@Todo(unknown type subscript), MPQ]`
+ sympy/polys/series/ring.py:70:9: error[invalid-assignment] Object of type `FlintPowerSeriesRingQQ` is not assignable to `PowerSeriesRingProto[Unknown, MPQ]`
- Found 13020 diagnostics
+ Found 13024 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/stats/_mstats_extras.py:88:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `flat` on type `ndarray[tuple[int, int], Unknown]` with custom `__set__` method
+ scipy/stats/_mstats_extras.py:88:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `flat` on type `ndarray[tuple[int, int], <class 'float64'>]` with custom `__set__` method
- scipy/stats/_mstats_extras.py:181:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `flat` on type `ndarray[tuple[int], Unknown]` with custom `__set__` method
+ scipy/stats/_mstats_extras.py:181:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `flat` on type `ndarray[tuple[int], <class 'float64'>]` with custom `__set__` method

```
</details>
No memory usage changes detected ✅


---

_@AlexWaygood reviewed on 2025-06-29 17:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:384 on 2025-06-29 17:07_

Hmm, I'm not sure whether I prefer "on" or "of" here. But either way -- could we push the decision on whether to make the change to a followup PR, rather than this one? It makes it hard to evaluate the mypy_primer diff, since this changes so many error messages are changing as a result of this change

---

_@AlexWaygood reviewed on 2025-06-29 17:09_

Wow, thank you! I'll probably take a full look tomorrow :-D

---

_Label `ty` added by @AlexWaygood on 2025-06-29 17:09_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-06-29 17:22_

---

_@AlexWaygood reviewed on 2025-06-29 17:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:384 on 2025-06-29 17:39_

Oh, but looking at the rich diagnostic diff (downloadable at https://github.com/astral-sh/ruff/actions/runs/15957614004/artifacts/3427588959), I see this PR doesn't actually add or remove a single diagnostic from the ecosystem -- it just changes the error message for 319 diagnostics! So I guess it _is_ okay for this change to be included in this PR. Though I'm still not sure which I actually prefer 😄

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-06-30 11:02_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-06-30 11:02_

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-01 07:23_

---

_Review request for @carljm removed by @carljm on 2025-07-03 01:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-09 06:22_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:384 on 2025-07-09 09:58_

Here is the true diff:

```diff
psycopg (https://github.com/psycopg/psycopg)
+ error[invalid-argument-type] tests/scripts/pipeline-demo.py:190:38: Argument to function `_pipeline_communicate` is incorrect: Expected `PGconn`, found `LoggingPGconn`
+ error[invalid-argument-type] tests/scripts/pipeline-demo.py:212:38: Argument to function `_pipeline_communicate` is incorrect: Expected `PGconn`, found `LoggingPGconn`
- Found 669 diagnostics
+ Found 671 diagnostics

optuna (https://github.com/optuna/optuna)
- error[invalid-argument-type] optuna/_gp/search_space.py:152:17: Argument to function `normalize_one_param` is incorrect: Expected `ScaleType`, found `ndarray[Unknown, Unknown]`
+ error[invalid-argument-type] optuna/_gp/search_space.py:152:17: Argument to function `normalize_one_param` is incorrect: Expected `ScaleType`, found `ndarray[Unknown, <class 'signedinteger[_64Bit]'>]`
- error[invalid-argument-type] optuna/_gp/search_space.py:154:17: Argument to function `normalize_one_param` is incorrect: Expected `int | float`, found `ndarray[Unknown, Unknown]`
+ error[invalid-argument-type] optuna/_gp/search_space.py:154:17: Argument to function `normalize_one_param` is incorrect: Expected `int | float`, found `ndarray[Unknown, <class 'float64'>]`
```

We can branch the handling to avoid the subtle difference between "on" and "of" in this PR, but I personally think that would make the code less consistent.

---

_@mtshiba reviewed on 2025-07-09 09:58_

---

_Comment by @github-actions[bot] on 2025-08-04 14:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-25 10:20:11.122932016 +0000
+++ new-output.txt	2025-08-25 10:20:13.657941955 +0000
@@ -252,17 +252,17 @@
 dataclasses_transform_converter.py:108:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
 dataclasses_transform_converter.py:109:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
 dataclasses_transform_converter.py:112:11: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
-dataclasses_transform_converter.py:114:1: error[invalid-assignment] Object of type `Literal["f1"]` is not assignable to attribute `field0` of type `int`
-dataclasses_transform_converter.py:115:1: error[invalid-assignment] Object of type `Literal["f6"]` is not assignable to attribute `field3` of type `ConverterClass`
-dataclasses_transform_converter.py:116:1: error[invalid-assignment] Object of type `Literal[b"f6"]` is not assignable to attribute `field3` of type `ConverterClass`
-dataclasses_transform_converter.py:119:1: error[invalid-assignment] Object of type `Literal[1]` is not assignable to attribute `field3` of type `ConverterClass`
+dataclasses_transform_converter.py:114:1: error[invalid-assignment] Object of type `Literal["f1"]` is not assignable to attribute `field0` on type `int`
+dataclasses_transform_converter.py:115:1: error[invalid-assignment] Object of type `Literal["f6"]` is not assignable to attribute `field3` on type `ConverterClass`
+dataclasses_transform_converter.py:116:1: error[invalid-assignment] Object of type `Literal[b"f6"]` is not assignable to attribute `field3` on type `ConverterClass`
+dataclasses_transform_converter.py:119:1: error[invalid-assignment] Object of type `Literal[1]` is not assignable to attribute `field3` on type `ConverterClass`
 dataclasses_transform_converter.py:121:11: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 7
 dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> Unknown`, found `def converter_simple(s: str) -> int`
 dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> @Todo(unsupported type[X] special form)`
 dataclasses_transform_field.py:75:16: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 dataclasses_transform_func.py:53:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_func.py:53:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
-dataclasses_transform_func.py:57:1: error[invalid-assignment] Object of type `Literal[3]` is not assignable to attribute `name` of type `str`
+dataclasses_transform_func.py:57:1: error[invalid-assignment] Object of type `Literal[3]` is not assignable to attribute `name` on type `str`
 dataclasses_transform_func.py:61:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
 dataclasses_transform_func.py:65:36: error[unknown-argument] Argument `salary` does not match any known parameter
 dataclasses_transform_func.py:71:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
@@ -428,8 +428,8 @@
 generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
 generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `typing.Self`
 generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `LinkedList[int]`
-generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
-generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
+generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` on type `typing.Self | None`
+generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` on type `typing.Self | None`
 generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@set_scale`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
@@ -728,13 +728,19 @@
 overloads_evaluation.py:322:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 protocols_class_objects.py:58:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
 protocols_class_objects.py:59:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
+protocols_class_objects.py:74:1: error[invalid-assignment] Object of type `<class 'ConcreteB'>` is not assignable to `ProtoB1`
 protocols_definition.py:114:1: error[invalid-assignment] Object of type `Concrete2_Bad1` is not assignable to `Template2`
 protocols_definition.py:115:1: error[invalid-assignment] Object of type `Concrete2_Bad2` is not assignable to `Template2`
 protocols_definition.py:116:1: error[invalid-assignment] Object of type `Concrete2_Bad3` is not assignable to `Template2`
 protocols_definition.py:156:1: error[invalid-assignment] Object of type `Concrete3_Bad1` is not assignable to `Template3`
 protocols_definition.py:159:1: error[invalid-assignment] Object of type `Concrete3_Bad4` is not assignable to `Template3`
 protocols_definition.py:160:1: error[invalid-assignment] Object of type `Concrete3_Bad5` is not assignable to `Template3`
+protocols_definition.py:218:1: error[invalid-assignment] Object of type `Concrete4_Bad1` is not assignable to `Template4`
 protocols_definition.py:219:1: error[invalid-assignment] Object of type `Concrete4_Bad2` is not assignable to `Template4`
+protocols_definition.py:339:1: error[invalid-assignment] Object of type `Concrete6_Bad1` is not assignable to `Template6`
+protocols_definition.py:340:1: error[invalid-assignment] Object of type `Concrete6_Bad2` is not assignable to `Template6`
+protocols_definition.py:341:1: error[invalid-assignment] Object of type `Concrete6_Bad3` is not assignable to `Template6`
+protocols_generic.py:146:1: error[invalid-assignment] Object of type `ConcreteHasProperty3` is not assignable to `HasPropertyProto`
 protocols_merging.py:52:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable1`
 protocols_merging.py:53:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable2`
 protocols_merging.py:54:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable3`
@@ -850,5 +856,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 851 diagnostics
+Found 857 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:384 on 2025-08-15 11:56_

I do prefer "on" here quite a bit -- sorry! Can we change it back to that wording?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:513 on 2025-08-15 12:10_

I think we can optimise this a little bit: if we already know that the read-type is not satisfied, there's no need to also check the write-type:

```suggestion
                };
                if !read_ok {
                    return false;
                }
                if let Some(Type::FunctionLiteral(setter)) = property.setter(db) {
                    let setter_value_type = setter.parameter_type(db, 1).unwrap_or(Type::unknown());
                    other
                        .validate_attribute_assignment(db, self.name, setter_value_type)
                        .is_not_err()
                } else {
                    true
                }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:504 on 2025-08-15 12:53_

this seems fine and I'm happy to go with it for now if it'll unblock your other PR https://github.com/astral-sh/ruff/pull/19064. But I think in the long term we'll want to just store a custom struct like this in the `ProtocolMemberKind::Property()` variant:

```rs
struct PropertyMember<'db> {
    get_type: Option<Type<'db>>,
    set_type: Option<Type<'db>>,
}
```

we'd convert `PropertyInstanceType`s into `PropertyMember`s as part of the logic in `cached_protocol_interface()`. This design would have the advantage that this assertion would pass, whereas it currently doesn't:

```py
from typing import Protocol
from ty_extensions import static_assert, is_equivalent_to

class A(Protocol):
    @property
    def x(self) -> int: ...

class B(Protocol):
    @property
    def x(self) -> int: ...

static_assert(is_equivalent_to(A | int | str, str | int | B))
```

The reason why the assertion currently fails is that the two protocols do not normalize to the same type. They don't normalize to the same type because their `x` members wrap `PropertyInstanceType`s that don't normalize to the same type, because one `PropertyInstanceType` has one `FunctionLiteral` type as its getter and the other `PropertyInstanceType` has a different `FunctionLiteral` type as its getter. But if we transformed both `x` properties into `PropertyMember { get_type: Some(int), set_type: None) }` when we collected the protocol interface, we wouldn't have this problem.

This design would also probably make assignability and subtyping checks more efficient for property members? And I think it would simplify the code you have here a fair bit.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:464 on 2025-08-15 13:00_

similarly here, we can just return early if `read_error` is true; no need to also check whether there was a write error in that case:

```suggestion
                if read_error {
                    return true;
                }
                if let Some(Type::FunctionLiteral(setter)) = property.setter(db) {
                    let setter_value_type = setter.parameter_type(db, 1).unwrap_or(Type::unknown());
                    !object_type
                        .validate_attribute_assignment(db, self.name, setter_value_type)
                        .is_not_err()
                } else {
                    false
                }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1 on 2025-08-15 13:01_

Could you add some short doc-comments to the APIs you're adding here, that describe what they do?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1443 on 2025-08-15 13:08_

huh, good catch!

---

_@AlexWaygood approved on 2025-08-15 13:17_

I'm sorry this has taken me so long to review!

This mostly looks great to me. The only things I'd like changed before merging are:
- I do prefer "on" rather than "of" for the error message you're changing, I'm afraid -- sorry! Can you revert back to the error message we currently have?
- I suggested a few minor optimisations below.

I also outlined a different design that would mean you could simplify some code here, which would also fix some other issues we have with regards to normalisation of protocols with property members. That's not blocking for this PR, however; I'm fine to land this and then I or you can tackle that refactor in a followup.

---

_@AlexWaygood requested changes on 2025-08-15 17:13_

Since approving this PR, I ran this on a large codebase that we have access to internally, and realised that it causes new stack overflows. We'll need to fix that before landing this PR, unfortunately :-(

I managed to reduce the code triggering the stack overflow to this, though you might be able to minimize it further:

```py
from typing import Protocol

class Foo[T]: ...

class A(Protocol):
    @property
    def _(self: "A") -> Foo: ...

class B(Protocol):
    @property
    def b(self) -> Foo[A]: ...

class C(Undefined): ...

class D:
    b: Foo[C]

class E[T: B](Protocol): ...

x: E[D]
```

Two strategies that can often be used to avoid stack overflows with recursive protocols are:
- Adding more calls to `.normalized()` before doing `has_relation_to()` checks
- Improving use of type visitors to guard against recursion: using `.has_relation_to_impl` methods rather than `.has_relation_to` methods

---

_Comment by @carljm on 2025-08-15 17:26_

> * Adding more calls to `.normalized()` before doing `has_relation_to()` checks
> 
> * Improving use of type visitors to guard against recursion: using `.has_relation_to_impl` methods rather than `.has_relation_to` methods

I think we should prefer the latter option (and eventually avoid using the former at all? at least for recursion-avoidance, maybe there are other reasons we need to normalize), but if the former option unblocks the PR more easily, we can always follow up and switch from the former to the latter.

---

_@mtshiba reviewed on 2025-08-16 07:07_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types.rs`:2292 on 2025-08-16 07:07_

Although it's not directly related to this PR, I fixed a subtle bug. `other` was incorrectly shadowed here.

---

_Review requested from @AlexWaygood by @mtshiba on 2025-08-16 07:38_

---

_@AlexWaygood reviewed on 2025-08-26 10:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:430 on 2025-08-26 10:14_

Thank you very much for this PR -- it was really helpful! This still doesn't go _quite_ in the direction I was thinking of, however -- you can see https://github.com/astral-sh/ruff/pull/19936 for the kind of thing I had in mind.

I think what I'd like to do at this point is to first land https://github.com/astral-sh/ruff/pull/20095 -- which includes many of your test improvements, plus some of my own -- and then land a cleaned-up version of #19936 on top of that. I'll list you as a co-author of both PRs, since both build on work you've contributed. I hope that's okay!

---

_@mtshiba reviewed on 2025-08-26 15:56_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/protocol_class.rs`:430 on 2025-08-26 15:56_

OK, let's move on!

---
