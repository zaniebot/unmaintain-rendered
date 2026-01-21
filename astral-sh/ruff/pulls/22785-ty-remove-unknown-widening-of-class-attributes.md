```yaml
number: 22785
title: "[ty] remove Unknown widening of class attributes"
type: pull_request
state: open
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: cjm/remove-place-widening
created_at: 2026-01-21T06:11:53Z
updated_at: 2026-01-21T15:16:51Z
url: https://github.com/astral-sh/ruff/pull/22785
synced_at: 2026-01-21T16:04:59Z
```

# [ty] remove Unknown widening of class attributes

---

_@carljm_

Part of https://github.com/astral-sh/ty/issues/1240

Stop widening un-annotated class attributes with Unknown.

---

_Label `ty` added by @carljm on 2026-01-21 06:11_

---

_Label `ecosystem-analyzer` added by @carljm on 2026-01-21 06:12_

---

_Comment by @astral-sh-bot[bot] on 2026-01-21 06:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.42% to 77.51%. The percentage of expected errors that received a diagnostic held steady at 60.49%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 672 | 672 | +0 |  |
| False Positives | 196 | 195 | -1 | ⏬ (✅) |
| False Negatives | 439 | 439 | +0 |  |
| Total Diagnostics | 868 | 867 | -1 | ⏬ |
| Precision | 77.42% | 77.51% | +0.09% | ⏫ (✅) |
| Recall | 60.49% | 60.49% | +0.00% |  |



### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [dataclasses_match_args.py:49:1](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/dataclasses_match_args.py#L49) | type-assertion-failure | Type `tuple[()]` does not match asserted type `Unknown \| tuple[()]` |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-21 06:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_cmp.py:321:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:321:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `str | None`
- tests/test_cmp.py:329:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:329:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `str | None`
- tests/test_cmp.py:389:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:389:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `str | None`
- tests/test_cmp.py:397:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:397:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `str | None`
- tests/test_cmp.py:407:18: warning[possibly-missing-attribute] Attribute `__lt__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:415:18: warning[possibly-missing-attribute] Attribute `__le__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:425:18: warning[possibly-missing-attribute] Attribute `__gt__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:435:18: warning[possibly-missing-attribute] Attribute `__ge__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:407:18: error[unresolved-attribute] Object of type `type` has no attribute `__lt__`
+ tests/test_cmp.py:415:18: error[unresolved-attribute] Object of type `type` has no attribute `__le__`
+ tests/test_cmp.py:425:18: error[unresolved-attribute] Object of type `type` has no attribute `__gt__`
+ tests/test_cmp.py:435:18: error[unresolved-attribute] Object of type `type` has no attribute `__ge__`
- tests/test_cmp.py:461:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:461:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `str | None`
- tests/test_cmp.py:469:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
+ tests/test_cmp.py:469:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `str | None`
- tests/test_cmp.py:479:18: warning[possibly-missing-attribute] Attribute `__lt__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:487:18: warning[possibly-missing-attribute] Attribute `__le__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:495:18: warning[possibly-missing-attribute] Attribute `__gt__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:503:18: warning[possibly-missing-attribute] Attribute `__ge__` may be missing on object of type `Unknown | type`
+ tests/test_cmp.py:479:18: error[unresolved-attribute] Object of type `type` has no attribute `__lt__`
+ tests/test_cmp.py:487:18: error[unresolved-attribute] Object of type `type` has no attribute `__le__`
+ tests/test_cmp.py:495:18: error[unresolved-attribute] Object of type `type` has no attribute `__gt__`
+ tests/test_cmp.py:503:18: error[unresolved-attribute] Object of type `type` has no attribute `__ge__`
+ tests/test_make.py:2864:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `own_eq_called` of type `Literal[False]`
+ tests/test_make.py:2868:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `own_le_called` of type `Literal[False]`
+ tests/test_make.py:2916:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `called` of type `Literal[False]`
+ tests/test_setattr.py:188:9: error[invalid-assignment] Object of type `Literal["1"]` is not assignable to attribute `x` of type `int`
+ tests/test_setattr.py:195:13: error[invalid-assignment] Object of type `Literal["1"]` is not assignable to attribute `x` of type `int`
- Found 628 diagnostics
+ Found 633 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_asyncio.py:1365:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_closing` of type `Literal[False]`
+ src/anyio/_backends/_asyncio.py:1537:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_closed` of type `Literal[False]`
- Found 92 diagnostics
+ Found 94 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/lock.py:153:13: error[invalid-assignment] Object of type `Unknown | Script` is not assignable to attribute `lua_release` of type `None`
+ aioredis/lock.py:155:13: error[invalid-assignment] Object of type `Unknown | Script` is not assignable to attribute `lua_extend` of type `None`
+ aioredis/lock.py:157:13: error[invalid-assignment] Object of type `Unknown | Script` is not assignable to attribute `lua_reacquire` of type `None`
- Found 29 diagnostics
+ Found 32 diagnostics

nionutils (https://github.com/nion-software/nionutils)
+ nion/utils/StructuredModel.py:187:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__initialized` of type `Literal[False]`
- Found 23 diagnostics
+ Found 24 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/vendor/decorator.py:165:31: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None`
+ pyinstrument/vendor/decorator.py:165:31: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None | LiteralString`

spack (https://github.com/spack/spack)
+ lib/spack/spack/bootstrap/core.py:149:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `config_scope_name` of type `Literal[""]`
+ lib/spack/spack/bootstrap/core.py:264:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `config_scope_name` of type `Literal[""]`
+ lib/spack/spack/cmd/create.py:439:17: error[invalid-assignment] Object of type `Literal["    pypi = \"{url}\""]` is not assignable to attribute `url_line` of type `Literal["    url = \"{url}\""]`
+ lib/spack/spack/cmd/create.py:442:13: error[invalid-assignment] Object of type `str | Unknown` is not assignable to attribute `url_line` of type `Literal["    url = \"{url}\""]`
+ lib/spack/spack/cmd/create.py:483:13: error[invalid-assignment] Object of type `Literal["    cran = \"{url}\""]` is not assignable to attribute `url_line` of type `Literal["    url = \"{url}\""]`
+ lib/spack/spack/environment/depfile.py:116:13: error[invalid-assignment] Object of type `Pattern[str]` is not assignable to attribute `_pattern` of type `None`
+ lib/spack/spack/environment/depfile.py:117:16: error[unresolved-attribute] Object of type `None` has no attribute `sub`
+ lib/spack/spack/installer.py:1244:13: error[invalid-assignment] Object of type `str` is not assignable to attribute `tmpdir` of type `None`
+ lib/spack/spack/installer.py:1245:13: error[invalid-assignment] Object of type `str` is not assignable to attribute `backup_dir` of type `None`
- lib/spack/spack/installer.py:1309:27: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ lib/spack/spack/installer.py:1309:27: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `None | Unknown | str`
- lib/spack/spack/installer.py:1322:23: error[invalid-argument-type] Argument to function `rename` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ lib/spack/spack/installer.py:1322:23: error[invalid-argument-type] Argument to function `rename` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `None | Unknown | str`
+ lib/spack/spack/multimethod.py:44:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `_locals` of type `None`
+ lib/spack/spack/test/build_environment.py:851:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `terminated` of type `Literal[False]`
+ lib/spack/spack/test/build_environment.py:879:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `runtime` of type `Literal[0]`
+ lib/spack/spack/test/stage.py:344:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `performed_search` of type `Literal[False]`
- lib/spack/spack/util/debug.py:71:50: error[invalid-argument-type] Argument to function `fdopen` is incorrect: Expected `int`, found `Unknown | int | None`
+ lib/spack/spack/util/debug.py:71:50: error[invalid-argument-type] Argument to function `fdopen` is incorrect: Expected `int`, found `int | Any | None`
+ lib/spack/spack/vendor/jinja2/environment.py:77:5: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `shared` of type `Literal[False]`
- lib/spack/spack/vendor/jinja2/runtime.py:1036:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ var/spack/test_repos/spack_repo/builtin_mock/packages/cmake_client/package.py:53:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `flipped` of type `Literal[False]`
+ var/spack/test_repos/spack_repo/builtin_mock/packages/cmake_client/package.py:58:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `did_something` of type `Literal[False]`
+ var/spack/test_repos/spack_repo/tutorial/packages/netlib_lapack/package.py:173:13: error[invalid-assignment] Object of type `bool` is not assignable to attribute `_building_shared` of type `Literal[False]`
+ var/spack/test_repos/spack_repo/tutorial/packages/netlib_lapack/package.py:178:13: error[invalid-assignment] Object of type `bool` is not assignable to attribute `_building_shared` of type `Literal[False]`
+ var/spack/test_repos/spack_repo/tutorial/packages/netlib_lapack/package.py:183:13: error[invalid-assignment] Object of type `bool` is not assignable to attribute `_building_shared` of type `Literal[False]`
+ var/spack/test_repos/spack_repo/tutorial/packages/netlib_lapack/package.py:188:13: error[invalid-assignment] Object of type `bool` is not assignable to attribute `_building_shared` of type `Literal[False]`
- Found 4336 diagnostics
+ Found 4355 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ src/bandersnatch/configuration.py:93:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `SHOWN_DEPRECATIONS` of type `Literal[False]`
+ src/bandersnatch/mirror.py:58:9: error[invalid-assignment] Object of type `datetime` is not assignable to attribute `now` of type `None`
+ src/bandersnatch/mirror.py:277:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `need_wrapup` of type `Literal[False]`
+ src/bandersnatch/mirror.py:305:13: error[invalid-assignment] Object of type `bool` is not assignable to attribute `need_index_sync` of type `Literal[True]`
+ src/bandersnatch/mirror.py:360:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `errors` of type `Literal[False]`
+ src/bandersnatch/tests/plugins/test_blocklist_name.py:22:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/plugins/test_blocklist_name.py:23:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/plugins/test_blocklist_name.py:153:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/plugins/test_blocklist_name.py:154:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/plugins/test_filename.py:19:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/plugins/test_filename.py:20:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/plugins/test_latest_release.py:19:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/plugins/test_latest_release.py:20:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/plugins/test_prerelease_name.py:20:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/plugins/test_prerelease_name.py:21:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/plugins/test_regex_name.py:20:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/plugins/test_regex_name.py:21:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/plugins/test_storage_plugins.py:53:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/plugins/test_storage_plugins.py:54:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/test_configuration.py:28:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/test_configuration.py:29:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/test_filter.py:27:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `cwd` of type `None`
+ src/bandersnatch/tests/test_filter.py:28:9: error[invalid-assignment] Object of type `TemporaryDirectory[str]` is not assignable to attribute `tempdir` of type `None`
+ src/bandersnatch/tests/test_mirror.py:370:5: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `errors` of type `Literal[False]`
+ src/bandersnatch/tests/test_mirror.py:419:5: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `errors` of type `Literal[False]`
+ src/bandersnatch_filter_plugins/latest_name.py:30:13: error[invalid-assignment] Object of type `int` is not assignable to attribute `keep` of type `Literal[0]`
+ src/bandersnatch_filter_plugins/latest_name.py:40:17: error[invalid-assignment] Object of type `Unknown | str` is not assignable to attribute `sort_by` of type `Literal["version"]`
+ src/bandersnatch_filter_plugins/metadata_filter.py:50:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `initialized` of type `Literal[False]`
+ src/bandersnatch_filter_plugins/metadata_filter.py:232:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `initialized` of type `Literal[False]`
+ src/bandersnatch_filter_plugins/metadata_filter.py:286:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `initialized` of type `Literal[False]`
- Found 78 diagnostics
+ Found 108 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/distlib/resources.py:274:9: error[invalid-method-override] Invalid override of method `_is_directory`: Definition is incompatible with `ResourceFinder._is_directory`
- src/pip/_vendor/distlib/util.py:1484:36: warning[possibly-missing-attribute] Attribute `getpeercert` may be missing on object of type `socket | Any`
+ src/pip/_vendor/pygments/token.py:40:9: error[invalid-assignment] Object of type `Self@__getattr__` is not assignable to attribute `parent` of type `None`
+ src/pip/_vendor/urllib3/connection.py:474:9: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | bool` is not assignable to attribute `is_verified` of type `Literal[False]`
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:50:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_registered_with_atexit` of type `Literal[False]`
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:59:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_dirty` of type `Literal[False]`
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:153:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_shutdown` of type `Literal[False]`
+ src/pip/_vendor/urllib3/poolmanager.py:493:9: error[invalid-assignment] Object of type `ProxyConfig` is not assignable to attribute `proxy_config` of type `None`
- src/pip/_vendor/urllib3/poolmanager.py:508:13: warning[possibly-missing-attribute] Attribute `host` may be missing on object of type `Unknown | None`
+ src/pip/_vendor/urllib3/poolmanager.py:508:13: warning[possibly-missing-attribute] Attribute `host` may be missing on object of type `None | Unknown`
- src/pip/_vendor/urllib3/poolmanager.py:508:30: warning[possibly-missing-attribute] Attribute `port` may be missing on object of type `Unknown | None`
+ src/pip/_vendor/urllib3/poolmanager.py:508:30: warning[possibly-missing-attribute] Attribute `port` may be missing on object of type `None | Unknown`
- src/pip/_vendor/urllib3/poolmanager.py:508:47: warning[possibly-missing-attribute] Attribute `scheme` may be missing on object of type `Unknown | None`
+ src/pip/_vendor/urllib3/poolmanager.py:508:47: warning[possibly-missing-attribute] Attribute `scheme` may be missing on object of type `None | Unknown`
- Found 595 diagnostics
+ Found 601 diagnostics

asynq (https://github.com/quora/asynq)
+ asynq/tests/test_tools.py:405:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `is_on` of type `Literal[False]`
- Found 217 diagnostics
+ Found 218 diagnostics

stone (https://github.com/dropbox/stone)
- stone/backends/js_client.py:167:27: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
+ stone/backends/js_client.py:167:27: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `None | Unknown`
- stone/backends/js_client.py:170:29: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
+ stone/backends/js_client.py:170:29: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `None | Unknown`
+ stone/backends/python_client.py:176:13: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `supported_auth_types` of type `None`
- stone/backends/python_types.py:210:24: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
+ stone/backends/python_types.py:210:24: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `None | Unknown`
+ stone/backends/tsd_types.py:164:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `split_by_namespace` of type `Literal[False]`
+ test/test_python_gen.py:288:9: error[invalid-assignment] Object of type `Literal["hello"]` is not assignable to attribute `_f_value` of type `NotSet`
+ test/test_python_gen.py:375:9: error[invalid-assignment] Object of type `S2` is not assignable to attribute `_f_value` of type `NotSet`
+ test/test_python_gen.py:377:9: error[invalid-assignment] Object of type `S3` is not assignable to attribute `_i_value` of type `NotSet`
- Found 252 diagnostics
+ Found 257 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/_internal.py:142:13: error[invalid-assignment] Object of type `bool` is not assignable to attribute `read_only` of type `Literal[False]`
+ src/werkzeug/_reloader.py:346:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `name` of type `Literal[""]`
+ src/werkzeug/exceptions.py:231:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `_description` of type `Literal["The browser (or proxy) sent a request that this server could not understand."]`
+ src/werkzeug/routing/converters.py:35:13: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `part_isolating` of type `Literal[True]`
+ src/werkzeug/routing/converters.py:82:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `regex` of type `Literal["[^/]+"]`
+ src/werkzeug/routing/converters.py:102:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `regex` of type `Literal["[^/]+"]`
+ src/werkzeug/routing/converters.py:145:13: error[invalid-assignment] Object of type `str` is not assignable to attribute `regex` of type `Literal["[^/]+"]`
+ tests/test_datastructures.py:561:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `show_exception` of type `Literal[False]`
+ tests/test_datastructures.py:567:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `show_exception` of type `Literal[False]`
+ tests/test_datastructures.py:582:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `(str | None, /) -> Literal[-1]`, found `<class 'int'>`
+ tests/test_datastructures.py:583:41: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `(str | None, /) -> Literal[-1]`, found `<class 'int'>`
- tests/test_datastructures.py:863:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal[42]` and `(Unknown & ~AlwaysFalsy) | (EnvironHeaders & ~AlwaysFalsy)`
+ tests/test_datastructures.py:863:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal[42]` and `EnvironHeaders & ~AlwaysFalsy`
+ tests/test_formparser.py:122:9: error[invalid-assignment] Object of type `Literal[1]` is not assignable to attribute `max_form_parts` of type `Literal[1000]`
+ tests/test_formparser.py:127:9: error[invalid-assignment] Object of type `Literal[1]` is not assignable to attribute `max_form_parts` of type `Literal[1000]`
+ tests/test_routing.py:768:13: error[invalid-assignment] Object of type `Literal["(\\w\\w+)/(\\w\\w+)"]` is not assignable to attribute `regex` of type `Literal["[^/]+"]`
+ tests/test_routing.py:794:13: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `part_isolating` of type `Literal[True]`
+ tests/test_routing.py:1209:5: error[invalid-assignment] Object of type `Literal[307]` is not assignable to attribute `code` of type `Literal[308]`
- tests/test_wrappers.py:729:5: error[invalid-assignment] Invalid assignment to data descriptor attribute `content_length` on type `Response` with custom `__set__` method
+ tests/test_wsgi.py:328:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `got_close` of type `Literal[False]`
+ tests/test_wsgi.py:331:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `got_additional` of type `Literal[False]`
- Found 407 diagnostics
+ Found 424 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/format.py:120:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `ERROR` of type `Literal["ERROR"]`
+ isort/format.py:121:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `SUCCESS` of type `Literal["SUCCESS"]`
- Found 35 diagnostics
+ Found 37 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/environment.py:79:5: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `shared` of type `Literal[False]`
- src/jinja2/runtime.py:1003:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_loader.py:209:17: error[invalid-assignment] Object of type `str` is not assignable to attribute `archive` of type `None`
+ tests/test_loader.py:212:13: error[invalid-assignment] Object of type `str` is not assignable to attribute `archive` of type `None`
+ tests/test_loader.py:214:9: error[invalid-assignment] Object of type `Environment` is not assignable to attribute `mod_env` of type `None`
- tests/test_loader.py:240:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:240:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:244:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:244:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:261:9: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:261:9: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:263:16: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:263:16: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:265:24: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:265:24: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:282:9: error[invalid-assignment] Object of type `ChoiceLoader` is not assignable to attribute `loader` on type `Unknown | None | Environment`
+ tests/test_loader.py:282:9: error[invalid-assignment] Object of type `ChoiceLoader` is not assignable to attribute `loader` on type `None | Unknown | Environment`
- tests/test_loader.py:283:14: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:283:14: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:285:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:285:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:287:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:287:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:292:9: error[invalid-assignment] Object of type `PrefixLoader` is not assignable to attribute `loader` on type `Unknown | None | Environment`
+ tests/test_loader.py:292:9: error[invalid-assignment] Object of type `PrefixLoader` is not assignable to attribute `loader` on type `None | Unknown | Environment`
- tests/test_loader.py:294:24: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:294:24: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:298:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:298:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:300:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:300:17: warning[possibly-missing-attribute] Attribute `get_template` may be missing on object of type `None | Unknown | Environment`
- tests/test_loader.py:306:20: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:306:20: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `None | Unknown | Environment`
+ tests/test_loader.py:308:9: error[invalid-assignment] Object of type `Environment` is not assignable to attribute `mod_env` of type `None`
- tests/test_loader.py:315:20: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `Unknown | None | Environment`
+ tests/test_loader.py:315:20: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `None | Unknown | Environment`
+ tests/test_loader.py:317:9: error[invalid-assignment] Object of type `Environment` is not assignable to attribute `mod_env` of type `None`
- Found 180 diagnostics
+ Found 185 diagnostics

black (https://github.com/psf/black)
+ src/blib2to3/pytree.py:405:13: error[invalid-assignment] Object of type `str` is not assignable to attribute `_prefix` of type `Literal[""]`
+ src/blib2to3/pytree.py:409:13: error[invalid-assignment] Object of type `str` is not assignable to attribute `_prefix` of type `Literal[""]`
+ src/blib2to3/pytree.py:468:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `_prefix` of type `Literal[""]`
- Found 48 diagnostics
+ Found 51 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ src/aiortc/contrib/media.py:227:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `kind` of type `Literal["unknown"]`
+ src/aiortc/rtcrtpreceiver.py:194:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `kind` of type `Literal["unknown"]`
- Found 194 diagnostics
+ Found 196 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ extras/qpsclient.py:32:13: error[invalid-assignment] Object of type `float` is not assignable to attribute `qps` of type `None`
+ extras/qpsclient.py:33:13: error[invalid-assignment] Object of type `int | float` is not assignable to attribute `download_delay` of type `None`
+ extras/qpsclient.py:35:13: error[invalid-assignment] Object of type `float` is not assignable to attribute `download_delay` of type `None`
+ scrapy/commands/parse.py:347:17: error[invalid-assignment] Object of type `Response` is not assignable to attribute `first_response` of type `None`
+ scrapy/core/downloader/webclient.py:220:13: error[invalid-assignment] Object of type `Literal[0]` is not assignable to attribute `waiting` of type `Literal[1]`
+ scrapy/core/downloader/webclient.py:225:13: error[invalid-assignment] Object of type `Literal[0]` is not assignable to attribute `waiting` of type `Literal[1]`
+ scrapy/core/downloader/webclient.py:235:13: error[invalid-assignment] Object of type `Literal[0]` is not assignable to attribute `waiting` of type `Literal[1]`
- tests/spiders.py:370:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `(Unknown & ~Request) | None`
+ tests/spiders.py:370:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `None`
+ tests/test_contracts.py:374:9: error[invalid-assignment] Object of type `Unknown | dict[str, Any]` is not assignable to attribute `meta` of type `None`
+ tests/test_contracts.py:376:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_contracts.py:384:9: error[invalid-assignment] Object of type `Unknown | dict[str, Any]` is not assignable to attribute `meta` of type `None`
+ tests/test_contracts.py:386:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_feedexport.py:610:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `init_with_crawler` of type `Literal[False]`
- tests/test_http_request.py:284:13: error[invalid-assignment] Object of type `Literal["http://example2.com"]` is not assignable to attribute `url` on type `Unknown | Request`
- tests/test_http_request.py:286:13: error[invalid-assignment] Object of type `Literal["xxx"]` is not assignable to attribute `body` on type `Unknown | Request`
+ tests/test_http_request.py:33:13: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
+ tests/test_http_request.py:37:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[123]`
+ tests/test_http_request.py:207:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def somecallback() -> Unknown`
+ tests/test_http_request.py:284:13: error[invalid-assignment] Cannot assign to read-only property `url` on object of type `Request`: Attempted assignment to `Request.url` here
+ tests/test_http_request.py:286:13: error[invalid-assignment] Cannot assign to read-only property `body` on object of type `Request`: Attempted assignment to `Request.body` here
+ tests/test_http_request.py:300:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def a_function() -> Unknown`
+ tests/test_http_request.py:307:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `def a_function() -> Unknown`
+ tests/test_http_request.py:324:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `Literal["a_function"]`
+ tests/test_http_request.py:329:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `Literal["a_function"]`
+ tests/test_http_request.py:430:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `tuple[tuple[Literal["a"], Literal["one"]], tuple[Literal["a"], Literal["two"]], tuple[Literal["b"], Literal["2"]]]`
+ tests/test_http_request.py:432:56: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `tuple[tuple[Literal["a"], Literal["one"]], tuple[Literal["a"], Literal["two"]], tuple[Literal["b"], Literal["2"]]]`
+ tests/test_http_request.py:447:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | bytes, Unknown | bytes]`
+ tests/test_http_request.py:465:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | str | bytes, Unknown | bytes | str]`
+ tests/test_http_request.py:474:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | bytes, Unknown | bytes]`
- tests/test_http_request.py:667:58: error[invalid-argument-type] Argument to bound method `from_response` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | str, Unknown | None]`
+ tests/test_http_request.py:667:58: error[invalid-argument-type] Argument to bound method `from_response` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[str, Iterable[str] | None]`
- tests/test_http_response.py:171:13: error[invalid-assignment] Object of type `Literal["http://example2.com"]` is not assignable to attribute `url` on type `Unknown | Response`
- tests/test_http_response.py:173:13: error[invalid-assignment] Object of type `Literal["xxx"]` is not assignable to attribute `body` on type `Unknown | Response`
+ tests/test_http_response.py:30:13: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
+ tests/test_http_response.py:35:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[b"http://example.com"]`
+ tests/test_http_response.py:37:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `dict[Unknown, Unknown]`
+ tests/test_http_response.py:72:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal["301"]`
+ tests/test_http_response.py:75:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal["lala200"]`
+ tests/test_http_response.py:171:13: error[invalid-assignment] Cannot assign to read-only property `url` on object of type `Response`: Attempted assignment to `Response.url` here
+ tests/test_http_response.py:173:13: error[invalid-assignment] Cannot assign to read-only property `body` on object of type `Response`: Attempted assignment to `Response.body` here
- tests/test_http_response.py:282:35: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Expected `Iterable[str | Link]`, found `list[Unknown | None]`
+ tests/test_http_response.py:282:35: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Expected `Iterable[str | Link]`, found `list[None]`
- tests/test_http_response.py:291:35: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Expected `Iterable[str | Link]`, found `list[Unknown | None]`
+ tests/test_http_response.py:291:35: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Expected `Iterable[str | Link]`, found `list[None]`
+ tests/test_item.py:89:13: error[invalid-assignment] Object of type `Literal["john"]` is not assignable to attribute `name` of type `Field`
+ tests/test_logformatter.py:251:13: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `drop` of type `Literal[True]`
+ tests/test_pipeline_files.py:654:9: error[invalid-assignment] Object of type `Literal["authenticatedRead"]` is not assignable to attribute `POLICY` of type `None`
+ tests/test_pipeline_files.py:770:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_from_crawler_called` of type `Literal[False]`
+ tests/test_pipeline_media.py:435:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_from_crawler_called` of type `Literal[False]`
+ tests/test_pipeline_media.py:454:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_from_crawler_called` of type `Literal[False]`
+ tests/test_scheduler.py:51:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `paused` of type `Literal[False]`
+ tests/test_scheduler.py:174:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `jobdir` of type `None`
- tests/test_scheduler.py:180:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ tests/test_scheduler.py:180:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `None | Unknown | str`
- tests/test_scheduler.py:314:13: error[unresolved-attribute] Object of type `Self@test_logic` has no attribute `close_scheduler`
- tests/test_scheduler.py:315:13: error[unresolved-attribute] Object of type `Self@test_logic` has no attribute `create_scheduler`
- tests/test_spider.py:48:16: warning[possibly-missing-attribute] Attribute `foo` may be missing on object of type `Unknown | Spider`
+ tests/test_spider.py:48:16: error[unresolved-attribute] Object of type `Spider` has no attribute `foo`
+ tests/test_spider.py:78:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `closed_called` of type `Literal[False]`
+ tests/test_spider.py:260:17: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Request | None`
+ tests/test_spider.py:283:17: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Request | None`
+ tests/test_spider.py:307:17: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Request | None`
+ tests/test_spider.py:329:17: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Request | None`
+ tests/test_spider.py:354:17: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Request | None`
+ tests/test_spider.py:382:17: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Request | None`
+ tests/test_spider.py:387:17: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Request | None`
+ tests/test_spider.py:410:17: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Request | None`
+ tests/test_spider.py:439:17: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Request | None`
+ tests/test_spider.py:444:17: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Request | None`
- Found 1789 diagnostics
+ Found 1835 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/contrib/release_robot.py:106:45: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `Unknown | bytes`
+ dulwich/contrib/release_robot.py:106:45: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `bytes`
- dulwich/contrib/swift.py:161:35: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[tuple[Unknown | bytes, None, None, bool]]`
+ dulwich/contrib/swift.py:161:35: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[tuple[bytes, None, None, bool]]`
- dulwich/gc.py:140:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `ObjectID`, found `Unknown | bytes`
+ dulwich/gc.py:140:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `ObjectID`, found `bytes`
- dulwich/gc.py:141:31: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `ObjectID`, found `Unknown | bytes`
+ dulwich/gc.py:141:31: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `ObjectID`, found `bytes`
- dulwich/object_filters.py:523:31: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[ObjectID, str, int]`, found `tuple[Unknown | bytes, Literal[""], Literal[0]]`
+ dulwich/object_filters.py:523:31: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[ObjectID, str, int]`, found `tuple[bytes, Literal[""], Literal[0]]`
- dulwich/object_store.py:2750:63: error[invalid-argument-type] Argument to function `_split_commits_and_tags` is incorrect: Expected `Iterable[ObjectID]`, found `list[Unknown | bytes]`
+ dulwich/object_store.py:2750:63: error[invalid-argument-type] Argument to function `_split_commits_and_tags` is incorrect: Expected `Iterable[ObjectID]`, found `list[bytes]`
- dulwich/object_store.py:2921:31: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[tuple[Unknown | bytes, None, Unknown | int, bool]]`
+ dulwich/object_store.py:2921:31: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[tuple[bytes, None, int, bool]]`
- dulwich/object_store.py:3560:26: error[invalid-assignment] Object of type `Unknown | bytes` is not assignable to `ObjectID | RawObjectID`
+ dulwich/object_store.py:3560:26: error[invalid-assignment] Object of type `bytes` is not assignable to `ObjectID | RawObjectID`
- dulwich/porcelain/__init__.py:4219:44: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `Unknown | bytes`
+ dulwich/porcelain/__init__.py:4219:44: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `bytes`
- dulwich/porcelain/__init__.py:4407:21: error[invalid-assignment] Invalid subscript assignment with key of type `bytes` and value of type `tuple[Unknown, Unknown | list[ObjectID], Unknown]` on object of type `dict[bytes, tuple[int, list[bytes], str]]`
+ dulwich/porcelain/__init__.py:4407:21: error[invalid-assignment] Invalid subscript assignment with key of type `bytes` and value of type `tuple[Unknown, list[ObjectID], Unknown]` on object of type `dict[bytes, tuple[int, list[bytes], str]]`
+ dulwich/porcelain/__init__.py:5739:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `data` on type `Blob` with custom `__set__` method
- dulwich/porcelain/__init__.py:6038:39: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `Unknown | bytes`
+ dulwich/porcelain/__init__.py:6038:39: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `bytes`
- dulwich/repo.py:2396:44: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `Unknown | bytes`
+ dulwich/repo.py:2396:44: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `bytes`
- dulwich/walk.py:193:24: error[invalid-argument-type] Argument to bound method `_push` is incorrect: Expected `ObjectID`, found `Unknown | bytes`
+ dulwich/walk.py:193:24: error[invalid-argument-type] Argument to bound method `_push` is incorrect: Expected `ObjectID`, found `bytes`
+ dulwich/worktree.py:560:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `parents` on type `Commit` with custom `__set__` method
+ dulwich/worktree.py:566:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `parents` on type `Commit` with custom `__set__` method
+ dulwich/worktree.py:657:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `parents` on type `Commit` with custom `__set__` method
- dulwich/worktree.py:716:46: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `Unknown | bytes`
+ dulwich/worktree.py:716:46: error[invalid-argument-type] Argument to bound method `get_object` is incorrect: Expected `ObjectID | RawObjectID`, found `bytes`
- Found 229 diagnostics
+ Found 233 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ tests/language/test_visitor.py:425:21: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `did_visit_added_field` of type `Literal[False]`
- Found 641 diagnostics
+ Found 642 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/capture.py:649:9: error[invalid-assignment] Object of type `Literal["started"]` is not assignable to attribute `_state` of type `None`
+ src/_pytest/capture.py:669:9: error[invalid-assignment] Object of type `Literal["suspended"]` is not assignable to attribute `_state` of type `None`
+ src/_pytest/capture.py:676:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_in_suspended` of type `Literal[False]`
+ src/_pytest/capture.py:679:9: error[invalid-assignment] Object of type `Literal["started"]` is not assignable to attribute `_state` of type `None`
+ src/_pytest/capture.py:693:9: error[invalid-assignment] Object of type `Literal["stopped"]` is not assignable to attribute `_state` of type `None`
+ src/_pytest/debugging.py:187:17: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `_continued` of type `Literal[False]`
+ testing/plugins_integration/pytest_rerunfailures_integration.py:12:13: error[invalid-assignment] Object of type `Literal[False]

... (truncated 4447 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~12MB
+     struct fields = ~11MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~54MB
+     struct fields = ~52MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-21 06:17_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 1,328 | 38 | 43 |
| `unresolved-attribute` | 559 | 168 | 0 |
| `possibly-missing-attribute` | 10 | 542 | 160 |
| `invalid-argument-type` | 22 | 26 | 269 |
| `invalid-method-override` | 202 | 0 | 4 |
| `unsupported-operator` | 0 | 30 | 94 |
| `type-assertion-failure` | 0 | 85 | 4 |
| `unused-ignore-comment` | 29 | 29 | 0 |
| `invalid-context-manager` | 0 | 0 | 57 |
| `invalid-return-type` | 9 | 0 | 33 |
| `not-iterable` | 0 | 1 | 18 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `possibly-unresolved-reference` | 0 | 6 | 0 |
| `call-non-callable` | 0 | 3 | 2 |
| `index-out-of-bounds` | 0 | 5 | 0 |
| `invalid-type-form` | 3 | 0 | 0 |
| `no-matching-overload` | 0 | 3 | 0 |
| `not-subscriptable` | 2 | 1 | 0 |
| `missing-argument` | 2 | 0 | 0 |
| `unresolved-reference` | 1 | 0 | 0 |
| **Total** | **2,167** | **937** | **691** |


**[Full report with detailed diff](https://4af3459f.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://4af3459f.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @codspeed-hq[bot] on 2026-01-21 06:22_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **improve performance by 4.41%**




`⚡ 1` improved benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | Simulation | [`` ty_micro[many_enum_members] ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fremove-place-widening?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members%3A%3Aty_micro%5Bmany_enum_members%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 122.3 ms | 117.1 ms | +4.41% |
---

<sub>Comparing <code>cjm/remove-place-widening</code> (ace792b) with <code>main</code> (745dfa6)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/cjm%2Fremove-place-widening?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fremove-place-widening?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Comment by @MichaReiser on 2026-01-21 07:44_

The very first result in the ecosystem report looks overly pedantic :) 

```
[error] invalid-assignment - [:130:9](https://github.com/Toufool/AutoSplit/blob/2ff11ab4e7a90ed9decf018ba3717d772dc0cb95/src/capture_method/VideoCaptureDeviceCaptureMethod.py#L130) - Object of type `Literal[True]` is not assignable to attribute `is_old_image` of type `Literal[False]`
```

https://github.com/Toufool/AutoSplit/blob/2ff11ab4e7a90ed9decf018ba3717d772dc0cb95/src/capture_method/VideoCaptureDeviceCaptureMethod.py#L130

Do we need to perform literal promotion in more places?

---

_Comment by @carljm on 2026-01-21 14:33_

Yes, I expected we would need literal promotion but wanted to see the ecosystem report; that's why this is in draft mode :)

---

_Comment by @MichaReiser on 2026-01-21 15:16_

Yeah sorry. I was very excited when I saw your PR and couldn't resist taking a look at the ecosystem report.

---
