```yaml
number: 20430
title: "[ty] Add support for `**kwargs`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dhruv/keywords-argument
created_at: 2025-09-16T06:02:20Z
updated_at: 2025-09-19T05:09:33Z
url: https://github.com/astral-sh/ruff/pull/20430
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Add support for `**kwargs`

---

_Pull request opened by @dhruvmanila on 2025-09-16 06:02_

## Summary

This PR adds support for unpacking `**kwargs` argument.

This can be matched against any standard (positional or keyword), keyword-only, or keyword variadic parameter that haven't been matched yet.

This PR also takes care of special casing `TypedDict` because the key names and the corresponding value type is known, so we can be more precise in our matching and type checking step. In the future, this special casing would be extended to include `ParamSpec` as well.

Part of astral-sh/ty#247

## Test Plan

Add test cases for various scenarios.


---

_Label `ty` added by @dhruvmanila on 2025-09-16 06:02_

---

_Comment by @github-actions[bot] on 2025-09-16 06:04_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-19 04:58:24.463113203 +0000
+++ new-output.txt	2025-09-19 04:58:27.522106846 +0000
@@ -109,9 +109,8 @@
 callables_kwargs.py:35:5: error[type-assertion-failure] Argument does not have asserted type `str`
 callables_kwargs.py:41:5: error[type-assertion-failure] Argument does not have asserted type `TD1`
 callables_kwargs.py:52:11: error[too-many-positional-arguments] Too many positional arguments to function `func1`: expected 0, got 3
-callables_kwargs.py:62:5: error[missing-argument] No argument provided for required parameter `v3` of function `func2`
 callables_kwargs.py:64:11: error[invalid-argument-type] Argument to function `func2` is incorrect: Expected `str`, found `Literal[1]`
-callables_kwargs.py:65:5: error[missing-argument] No argument provided for required parameter `v3` of function `func2`
+callables_kwargs.py:64:14: error[parameter-already-assigned] Multiple values provided for parameter `v3` of function `func2`
 callables_kwargs.py:103:1: error[invalid-assignment] Object of type `def func1(**kwargs: @Todo(`Unpack[]` special form)) -> None` is not assignable to `TDProtocol5`
 callables_kwargs.py:134:1: error[invalid-assignment] Object of type `def func7(*, v1: int, v3: str, v2: str = Literal[""]) -> None` is not assignable to `TDProtocol6`
 callables_protocol.py:35:1: error[invalid-assignment] Object of type `def cb1_bad1(*vals: bytes, *, max_items: int | None) -> list[bytes]` is not assignable to `Proto1`
@@ -374,6 +373,7 @@
 generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
 generics_defaults_specialization.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
+generics_paramspec_components.py:49:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `tuple[Unknown, ...]`
 generics_paramspec_components.py:83:18: error[parameter-already-assigned] Multiple values provided for parameter 1 (`x`) of function `foo`
 generics_paramspec_semantics.py:13:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> str`
 generics_paramspec_semantics.py:17:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-16 06:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- mypy_primer/globals.py:224:11: error[missing-argument] No arguments provided for required parameters `new`, `old`, `repo`, `type_checker`, `mypyc_compile_level`, `custom_typeshed_repo`, `new_typeshed`, `old_typeshed`, `new_prepend_path`, `old_prepend_path`, `additional_flags`, `project_selector`, `known_dependency_selector`, `local_project`, `expected_success`, `project_date`, `shard_index`, `num_shards`, `output`, `old_success`, `show_speed_regression`, `coverage`, `bisect`, `bisect_output`, `validate_expected_success`, `measure_project_runtimes`, `concurrency`, `base_dir`, `debug`, `clear`
- mypy_primer/main.py:58:12: error[missing-argument] No arguments provided for required parameters `repo`, `mypyc_compile_level` of function `setup_mypy`
- mypy_primer/main.py:58:12: error[missing-argument] No argument provided for required parameter `repo` of function `setup_pyright`
- mypy_primer/main.py:58:12: error[missing-argument] No argument provided for required parameter `repo` of function `setup_ty`
- mypy_primer/main.py:58:12: error[missing-argument] No arguments provided for required parameters `repo`, `typeshed_dir` of function `setup_pyrefly`
- Found 8 diagnostics
+ Found 3 diagnostics

attrs (https://github.com/python-attrs/attrs)
- tests/test_dunders.py:677:15: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ tests/test_dunders.py:677:15: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
- tests/test_make.py:587:13: error[call-non-callable] Object of type `<decorator produced by dataclass-like function>` is not callable
- tests/test_make.py:2169:15: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ tests/test_make.py:2169:15: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
- tests/test_make.py:2191:22: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ tests/test_make.py:2191:22: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
- tests/test_make.py:2192:30: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ tests/test_make.py:2192:30: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
- Found 565 diagnostics
+ Found 564 diagnostics

com2ann (https://github.com/ilevkivskyi/com2ann)
- src/com2ann.py:1039:15: error[missing-argument] No arguments provided for required parameters `drop_none`, `drop_ellipsis`, `silent`
- Found 4 diagnostics
+ Found 3 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/debug/tbtools.py:192:26: error[missing-argument] No arguments provided for required parameters `locals`, `globals` of bound method `__init__`
- src/werkzeug/wsgi.py:66:12: error[missing-argument] No arguments provided for required parameters `scheme`, `host` of function `get_current_url`
- tests/live_apps/run.py:37:1: error[missing-argument] No arguments provided for required parameters `hostname`, `port`, `application` of function `run_simple`
- Found 367 diagnostics
+ Found 364 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
- Found 23 diagnostics
+ Found 71 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/metadata/metacheck.py:202:9: error[missing-argument] No arguments provided for required parameters `func`, `conf` of bound method `reinit`
- beartype/_decor/_nontype/_decordescriptor.py:74:20: error[missing-argument] No argument provided for required parameter `conf` of function `beartype_func`
- beartype/_decor/_nontype/_decordescriptor.py:160:29: error[missing-argument] No argument provided for required parameter `conf` of function `beartype_func`
- beartype/_decor/_nontype/_decordescriptor.py:162:30: error[missing-argument] No argument provided for required parameter `conf` of function `beartype_func`
+ beartype/_decor/_nontype/_decordescriptor.py:152:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_decor/_nontype/_decordescriptor.py:263:80: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_decor/_nontype/decornontype.py:312:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_decor/_nontype/decornontype.py:494:25: error[missing-argument] No argument provided for required parameter `conf` of function `beartype_func`
- beartype/_decor/_nontype/decornontype.py:622:43: error[missing-argument] No argument provided for required parameter `conf` of function `beartype_func`
+ beartype/_decor/decorcore.py:133:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_util/api/external/utilclick.py:104:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/api/standard/utilfunctools.py:295:20: error[missing-argument] No argument provided for required parameter `conf` of function `beartype_func`
- beartype/_util/api/standard/utilfunctools.py:319:12: error[invalid-return-type] Return type does not match returned value: expected `BeartypeableT@beartype_functools_lru_cache`, found `_lru_cache_wrapper[Unknown]`
- beartype/_util/hint/pep/proposal/pep612.py:220:12: error[missing-argument] No arguments provided for required parameters `decor_meta`, `pith_name`, `arg_kind`, `exception_prefix` of function `_reduce_hint_pep612_args_or_kwargs`
- beartype/_util/hint/pep/proposal/pep612.py:253:12: error[missing-argument] No arguments provided for required parameters `decor_meta`, `pith_name`, `arg_kind`, `exception_prefix` of function `_reduce_hint_pep612_args_or_kwargs`
- beartype/bite/collection/infercollectionsabc.py:164:20: error[missing-argument] No arguments provided for required parameters `conf`, `__beartype_obj_ids_seen__` of function `infer_hint_collection_items`
- Found 592 diagnostics
+ Found 586 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/contrib/signaling.py:28:16: error[missing-argument] No arguments provided for required parameters `sdp`, `type`
- Found 93 diagnostics
+ Found 92 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/contrib/models/tiny_vit.py:411:25: error[missing-argument] No arguments provided for required parameters `dim`, `depth` of bound method `__init__`
- kornia/contrib/models/tiny_vit.py:413:25: error[missing-argument] No arguments provided for required parameters `dim`, `depth` of bound method `__init__`
- Found 785 diagnostics
+ Found 783 diagnostics

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/base_checker.py:207:16: error[missing-argument] No argument provided for required parameter `scope` of bound method `__init__`
- Found 178 diagnostics
+ Found 177 diagnostics

alerta (https://github.com/alerta/alerta)
- alerta/plugins/forwarder.py:61:21: error[missing-argument] No arguments provided for required parameters `resource`, `event` of bound method `send_alert`
- Found 513 diagnostics
+ Found 512 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_internal/locations/_sysconfig.py:199:18: error[missing-argument] No arguments provided for required parameters `platlib`, `purelib`, `headers`, `scripts`, `data`
- src/pip/_vendor/requests/cookies.py:489:12: error[missing-argument] No arguments provided for required parameters `version`, `name`, `value`, `port`, `port_specified`, `domain`, `domain_specified`, `domain_initial_dot`, `path`, `path_specified`, `secure`, `expires`, `discard`, `comment`, `comment_url`, `rest` of bound method `__init__`
- Found 436 diagnostics
+ Found 434 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_core/intents/causes.py:272:12: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`
- kopf/_core/intents/causes.py:283:12: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`, `reset`
- kopf/_core/intents/causes.py:315:16: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`, `body`, `initial`
- kopf/_core/intents/causes.py:321:16: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`, `body`, `initial`
- kopf/_core/intents/causes.py:324:16: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`, `body`, `initial`
- kopf/_core/intents/causes.py:331:16: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`, `body`, `initial`
- kopf/_core/intents/causes.py:338:16: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`, `body`, `initial`
- kopf/_core/intents/causes.py:343:16: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`, `body`, `initial`
- kopf/_core/intents/causes.py:346:12: error[missing-argument] No arguments provided for required parameters `logger`, `indices`, `memo`, `resource`, `patch`, `body`, `initial`
- Found 52 diagnostics
+ Found 43 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/contrib/paramiko_vendor.py:207:9: error[missing-argument] No argument provided for required parameter `hostname` of bound method `connect`
- dulwich/porcelain.py:811:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/porcelain.py:2397:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/porcelain.py:2553:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 184 diagnostics
+ Found 180 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ docs/_ext/scrapydocs.py:101:55: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | None`
- scrapy/http/request/rpc.py:27:30: error[missing-argument] No argument provided for required parameter `params` of function `dumps`
+ tests/test_cmdline/__init__.py:26:16: error[unresolved-attribute] Type `str` has no attribute `decode`
+ tests/test_commands.py:108:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Popen[bytes], str, str]`, found `tuple[Popen[str], str, str]`
- tests/test_downloadermiddleware_cookies.py:473:20: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
- tests/test_downloadermiddleware_cookies.py:478:20: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
- tests/test_downloadermiddleware_cookies.py:554:20: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
- tests/test_downloadermiddleware_cookies.py:556:20: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
- tests/test_downloaderslotssettings.py:99:16: error[missing-argument] No arguments provided for required parameters `concurrency`, `delay`, `randomize_delay` of bound method `__init__`
- tests/test_http_request.py:1440:13: error[missing-argument] No argument provided for required parameter `params` of function `dumps`
- tests/test_utils_curl.py:17:13: error[missing-argument] No argument provided for required parameter `url` of bound method `__init__`
- Found 1050 diagnostics
+ Found 1045 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/local_run.py:1007:22: error[missing-argument] No arguments provided for required parameters `memory`, `chosen_port`, `container_port`, `container_name`, `volumes`, `env`, `interactive`, `docker_hash`, `command`, `net`, `docker_params`, `detach` of function `get_docker_run_cmd`
- paasta_tools/cli/cmds/spark_run.py:757:22: error[missing-argument] No arguments provided for required parameters `container_name`, `volumes`, `env`, `docker_img`, `docker_cmd`, `nvidia`, `docker_memory_limit`, `docker_shm_size`, `docker_cpu_limit` of function `get_docker_run_cmd`
- paasta_tools/instance/kubernetes.py:1358:32: error[missing-argument] No arguments provided for required parameters `service`, `instance`, `job_config`, `service_namespace_config`, `pods_task`, `settings` of function `mesh_status`
+ paasta_tools/instance/kubernetes.py:1360:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | @Todo(Inference of subscript on special form) | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1360:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | @Todo(Inference of subscript on special form) | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1360:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `LongRunningServiceConfig`, found `str | @Todo(Inference of subscript on special form) | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1360:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `ServiceNamespaceConfig`, found `str | @Todo(Inference of subscript on special form) | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1360:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `Future[Unknown]`, found `str | @Todo(Inference of subscript on special form) | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1360:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `bool`, found `str | @Todo(Inference of subscript on special form) | ServiceNamespaceConfig | Task[Unknown] | bool`
- paasta_tools/kubernetes/application/controller_wrappers.py:64:32: error[missing-argument] No arguments provided for required parameters `service`, `instance`, `git_sha`, `image_version`, `config_sha`
+ paasta_tools/kubernetes_tools.py:4135:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ paasta_tools/kubernetes_tools.py:4137:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/tron/client.py:45:24: error[missing-argument] No argument provided for required parameter `url` of function `get`
- paasta_tools/tron/client.py:51:24: error[missing-argument] No argument provided for required parameter `url` of function `post`
+ paasta_tools/utils.py:2887:17: error[no-matching-overload] No overload of bound method `write` matches arguments
- paasta_tools/utils.py:2887:17: warning[possibly-unbound-attribute] Attribute `write` on type `IO[bytes] | None` is possibly unbound
+ paasta_tools/utils.py:2887:17: warning[possibly-unbound-attribute] Attribute `write` on type `IO[str] | None` is possibly unbound
- paasta_tools/utils.py:2888:17: warning[possibly-unbound-attribute] Attribute `flush` on type `IO[bytes] | None` is possibly unbound
+ paasta_tools/utils.py:2888:17: warning[possibly-unbound-attribute] Attribute `flush` on type `IO[str] | None` is possibly unbound
- paasta_tools/utils.py:2900:31: warning[possibly-unbound-attribute] Attribute `readline` on type `IO[bytes] | None` is possibly unbound
+ paasta_tools/utils.py:2900:31: warning[possibly-unbound-attribute] Attribute `readline` on type `IO[str] | None` is possibly unbound
- Found 872 diagnostics
+ Found 875 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/utilities/extend_schema.py:306:16: error[missing-argument] No arguments provided for required parameters `name`, `locations` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:343:23: error[missing-argument] No argument provided for required parameter `type_` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:363:16: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/extend_schema.py:363:16: error[missing-argument] No arguments provided for required parameters `name`, `fields` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:378:16: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/extend_schema.py:378:16: error[missing-argument] No arguments provided for required parameters `name`, `values` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:395:16: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/extend_schema.py:395:16: error[missing-argument] No argument provided for required parameter `name` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:430:16: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/extend_schema.py:430:16: error[missing-argument] No arguments provided for required parameters `name`, `fields` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:470:16: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/extend_schema.py:470:16: error[missing-argument] No arguments provided for required parameters `name`, `fields` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:495:16: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/extend_schema.py:495:16: error[missing-argument] No arguments provided for required parameters `name`, `types` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:506:16: error[missing-argument] No argument provided for required parameter `type_` of bound method `__init__`
- src/graphql/utilities/extend_schema.py:516:16: error[missing-argument] No argument provided for required parameter `type_` of bound method `__init__`
- src/graphql/utilities/lexicographic_sort_schema.py:65:16: error[missing-argument] No arguments provided for required parameters `name`, `locations` of bound method `__init__`
- src/graphql/utilities/lexicographic_sort_schema.py:76:26: error[missing-argument] No argument provided for required parameter `type_` of bound method `__init__`
- src/graphql/utilities/lexicographic_sort_schema.py:87:28: error[missing-argument] No argument provided for required parameter `type_` of bound method `__init__`
- src/graphql/utilities/lexicographic_sort_schema.py:121:20: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/lexicographic_sort_schema.py:121:20: error[missing-argument] No arguments provided for required parameters `name`, `fields` of bound method `__init__`
+ src/graphql/utilities/lexicographic_sort_schema.py:121:20: error[missing-argument] No argument provided for required parameter `fields` of bound method `__init__`
- src/graphql/utilities/lexicographic_sort_schema.py:129:20: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/lexicographic_sort_schema.py:129:20: error[missing-argument] No arguments provided for required parameters `name`, `fields` of bound method `__init__`
+ src/graphql/utilities/lexicographic_sort_schema.py:129:20: error[missing-argument] No argument provided for required parameter `fields` of bound method `__init__`
- src/graphql/utilities/lexicographic_sort_schema.py:137:20: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/lexicographic_sort_schema.py:137:20: error[missing-argument] No arguments provided for required parameters `name`, `types` of bound method `__init__`
+ src/graphql/utilities/lexicographic_sort_schema.py:137:20: error[missing-argument] No argument provided for required parameter `types` of bound method `__init__`
- src/graphql/utilities/lexicographic_sort_schema.py:141:20: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/lexicographic_sort_schema.py:141:20: error[missing-argument] No arguments provided for required parameters `name`, `values` of bound method `__init__`
+ src/graphql/utilities/lexicographic_sort_schema.py:141:20: error[missing-argument] No argument provided for required parameter `values` of bound method `__init__`
- src/graphql/utilities/lexicographic_sort_schema.py:156:20: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- src/graphql/utilities/lexicographic_sort_schema.py:156:20: error[missing-argument] No arguments provided for required parameters `name`, `fields` of bound method `__init__`
+ src/graphql/utilities/lexicographic_sort_schema.py:156:20: error[missing-argument] No argument provided for required parameter `fields` of bound method `__init__`
- tests/execution/test_defer.py:209:18: error[missing-argument] No arguments provided for required parameters `id`, `path` of bound method `__init__`
- tests/execution/test_defer.py:210:26: error[missing-argument] No arguments provided for required parameters `id`, `path` of bound method `__init__`
- tests/execution/test_defer.py:211:26: error[missing-argument] No arguments provided for required parameters `id`, `path` of bound method `__init__`
- tests/execution/test_defer.py:212:26: error[missing-argument] No arguments provided for required parameters `id`, `path` of bound method `__init__`
- tests/execution/test_defer.py:213:26: error[missing-argument] No arguments provided for required parameters `id`, `path` of bound method `__init__`
- tests/execution/test_defer.py:234:18: error[missing-argument] No argument provided for required parameter `id` of bound method `__init__`
- tests/execution/test_defer.py:235:26: error[missing-argument] No argument provided for required parameter `id` of bound method `__init__`
- tests/execution/test_defer.py:236:26: error[missing-argument] No argument provided for required parameter `id` of bound method `__init__`
- tests/execution/test_defer.py:237:26: error[missing-argument] No argument provided for required parameter `id` of bound method `__init__`
- tests/execution/test_defer.py:281:18: error[missing-argument] No arguments provided for required parameters `data`, `id` of bound method `__init__`
- tests/execution/test_defer.py:282:26: error[missing-argument] No arguments provided for required parameters `data`, `id` of bound method `__init__`
- tests/execution/test_defer.py:283:26: error[missing-argument] No arguments provided for required parameters `data`, `id` of bound method `__init__`
- tests/execution/test_defer.py:286:26: error[missing-argument] No arguments provided for required parameters `data`, `id` of bound method `__init__`
- tests/execution/test_defer.py:287:26: error[missing-argument] No arguments provided for required parameters `data`, `id` of bound method `__init__`
- tests/execution/test_defer.py:290:26: error[missing-argument] No arguments provided for required parameters `data`, `id` of bound method `__init__`
- tests/execution/test_defer.py:291:26: error[missing-argument] No arguments provided for required parameters `data`, `id` of bound method `__init__`
- tests/execution/test_stream.py:186:18: error[missing-argument] No arguments provided for required parameters `items`, `id` of bound method `__init__`
- tests/execution/test_stream.py:187:26: error[missing-argument] No arguments provided for required parameters `items`, `id` of bound method `__init__`
- tests/execution/test_stream.py:188:26: error[missing-argument] No arguments provided for required parameters `items`, `id` of bound method `__init__`
- tests/execution/test_stream.py:191:26: error[missing-argument] No arguments provided for required parameters `items`, `id` of bound method `__init__`
- tests/execution/test_stream.py:192:26: error[missing-argument] No arguments provided for required parameters `items`, `id` of bound method `__init__`
- tests/execution/test_stream.py:195:26: error[missing-argument] No arguments provided for required parameters `items`, `id` of bound method `__init__`
- tests/execution/test_stream.py:196:26: error[missing-argument] No arguments provided for required parameters `items`, `id` of bound method `__init__`
- tests/test_user_registry.py:70:16: error[missing-argument] No arguments provided for required parameters `firstName`, `lastName`, `tweets`
- tests/type/test_custom_scalars.py:59:12: error[missing-argument] No arguments provided for required parameters `amount`, `currency`
+ tests/test_user_registry.py:288:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_user_registry.py:344:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_definition.py:1288:17: error[missing-argument] No argument provided for required parameter `name` of function `__new__`
- tests/type/test_definition.py:1288:17: error[missing-argument] No argument provided for required parameter `name` of bound method `__init__`
- Found 353 diagnostics
+ Found 304 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/AuthenticatedUser.py:691:17: error[missing-argument] No arguments provided for required parameters `email`, `primary`, `verified`, `visibility`
- github/Requester.py:573:16: error[missing-argument] No arguments provided for required parameters `auth`, `base_url`, `timeout`, `user_agent`, `per_page`, `verify`, `retry`, `pool_size` of bound method `__init__`
- github/Requester.py:594:16: error[missing-argument] No arguments provided for required parameters `auth`, `base_url`, `timeout`, `user_agent`, `per_page`, `verify`, `retry`, `pool_size` of bound method `__init__`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | str | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | str`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Retry | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AppAuth | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | str | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | str`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Retry | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AppAuth | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `AppAuth | str | Unknown | int`
- Found 316 diagnostics
+ Found 345 diagnostics

porcupine (https://github.com/Akuli/porcupine)
+ porcupine/plugins/git_right_click.py:56:22: error[unresolved-attribute] Type `str` has no attribute `decode`
+ porcupine/plugins/git_right_click.py:57:24: error[unresolved-attribute] Type `str` has no attribute `decode`
+ porcupine/plugins/langserver.py:706:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Popen[bytes]`, found `Popen[str]`
- Found 22 diagnostics
+ Found 25 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- aiohttp_devtools/cli.py:47:5: error[missing-argument] No argument provided for required parameter `app` of function `run_app`
- aiohttp_devtools/cli.py:110:9: error[missing-argument] No argument provided for required parameter `app` of function `run_app`
- Found 124 diagnostics
+ Found 122 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/pytester.py:1369:13: error[no-matching-overload] No overload of bound method `write` matches arguments
- src/_pytest/raises.py:283:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/_pytest/reports.py:572:32: error[missing-argument] No argument provided for required parameter `args`
- src/_pytest/reports.py:574:31: error[missing-argument] No arguments provided for required parameters `path`, `lineno`, `message`
- src/_pytest/reports.py:595:16: error[missing-argument] No arguments provided for required parameters `reprentries`, `extraline`, `style`
- src/_pytest/reports.py:599:20: error[missing-argument] No arguments provided for required parameters `path`, `lineno`, `message`
- Found 482 diagnostics
+ Found 478 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/database.py:255:16: error[missing-argument] No arguments provided for required parameters `path`, `installed` of bound method `__init__`
- Found 7529 diagnostics
+ Found 7528 diagnostics

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
- relays/relays.py:94:26: error[missing-argument] No argument provided for required parameter `channels` of bound method `__init__`
- relays/relays.py:97:16: error[missing-argument] No arguments provided for required parameters `source`, `destinations` of bound method `__init__`
- Found 144 diagnostics
+ Found 142 diagnostics

nox (https://github.com/wntrblm/nox)
- nox/_option_set.py:231:21: error[missing-argument] No argument provided for required parameter `group` of bound method `__init__`
- nox/_option_set.py:241:22: error[missing-argument] No argument provided for required parameter `group` of bound method `__init__`
- Found 24 diagnostics
+ Found 22 diagnostics

flake8 (https://github.com/pycqa/flake8)
- tests/unit/test_statistics.py:21:12: error[missing-argument] No arguments provided for required parameters `code`, `filename`, `line_number`, `column_number`, `text`
- Found 39 diagnostics
+ Found 38 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/main.py:1609:16: error[missing-argument] No argument provided for required parameter `deep` of function `_copy_and_set_values`
- pydantic/v1/datetime_parse.py:132:16: error[missing-argument] No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
+ pydantic/v1/datetime_parse.py:208:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 761 diagnostics
+ Found 760 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/distributions/rhel.py:94:19: error[missing-argument] No argument provided for required parameter `gpgurls`
- mkosi/distributions/rhel.py:100:19: error[missing-argument] No argument provided for required parameter `gpgurls`
- mkosi/distributions/rhel.py:106:19: error[missing-argument] No argument provided for required parameter `gpgurls`
+ mkosi/distributions/rhel.py:98:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:98:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:98:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:98:17: error[invalid-argument-type] Argument is incorrect: Expected `int | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:104:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:104:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:104:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:104:17: error[invalid-argument-type] Argument is incorrect: Expected `int | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:110:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:110:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:110:17: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `tuple[str, ...] | Unknown`
+ mkosi/distributions/rhel.py:110:17: error[invalid-argument-type] Argument is incorrect: Expected `int | None`, found `tuple[str, ...] | Unknown`
- Found 100 diagnostics
+ Found 109 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/cli/commands/run/executor.py:60:29: error[missing-argument] No arguments provided for required parameters `include_path`, `include_method`, `include_name`, `include_tag`, `include_operation_id`, `include_path_regex`, `include_method_regex`, `include_name_regex`, `include_tag_regex`, `include_operation_id_regex`, `exclude_path`, `exclude_method`, `exclude_name`, `exclude_tag`, `exclude_operation_id`, `exclude_path_regex`, `exclude_method_regex`, `exclude_name_regex`, `exclude_tag_regex`, `exclude_operation_id_regex`, `include_by`, `exclude_by`, `exclude_deprecated` of bound method `create_filter_set`
- src/schemathesis/generation/overrides.py:39:16: error[missing-argument] No arguments provided for required parameters `query`, `headers`, `cookies`, `path_parameters`
+ src/schemathesis/transport/requests.py:144:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 271 diagnostics
+ Found 270 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/handlers/test_state_param_scheduler.py:336:25: error[missing-argument] No arguments provided for required parameters `initial_value`, `gamma`, `milestones`, `param_name` of bound method `__init__`
- Found 2139 diagnostics
+ Found 2138 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:858:28: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `object`
+ mkdocs/config/config_options.py:858:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `object`
+ mkdocs/config/config_options.py:858:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `object`
+ mkdocs/config/config_options.py:858:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Collection[str]`, found `object`
+ mkdocs/config/config_options.py:858:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `object`
+ mkdocs/tests/structure/page_tests.py:670:39: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)] | str | None`
+ mkdocs/tests/structure/page_tests.py:693:39: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)] | str`
- Found 199 diagnostics
+ Found 206 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/websocket.py:1045:28: error[missing-argument] No arguments provided for required parameters `persistent`, `max_wbits` of bound method `__init__`
- tornado/websocket.py:1048:30: error[missing-argument] No arguments provided for required parameters `persistent`, `max_wbits` of bound method `__init__`
- Found 248 diagnostics
+ Found 246 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/footer.py:48:35: error[missing-argument] No argument provided for required parameter `name`
- tests/test_footer.py:35:27: error[missing-argument] No argument provided for required parameter `name`
- tests/test_footer.py:89:48: error[missing-argument] No argument provided for required parameter `name`
- Found 153 diagnostics
+ Found 150 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/poetry.py:90:13: error[missing-argument] No argument provided for required parameter `name`
- Found 935 diagnostics
+ Found 934 diagnostics

operator (https://github.com/canonical/operator)
+ ops/model.py:3671:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 99 diagnostics
+ Found 100 diagnostics

vision (https://github.com/pytorch/vision)
- test/test_functional_tensor.py:1269:19: error[missing-argument] No argument provided for required parameter `displacement` of function `elastic_transform`
- test/test_transforms_tensor.py:434:17: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:179:9: error[missing-argument] No arguments provided for required parameters `rtol`, `atol` of function `_check_kernel_cuda_vs_cpu`
- test/test_transforms_v2.py:182:9: error[missing-argument] No arguments provided for required parameters `rtol`, `atol` of function `_check_kernel_scripted_vs_eager`
- test/test_transforms_v2.py:185:9: error[missing-argument] No arguments provided for required parameters `rtol`, `atol` of function `_check_kernel_batched_vs_unbatched`
- test/test_transforms_v2.py:412:9: error[missing-argument] No arguments provided for required parameters `rtol`, `atol` of function `_check_transform_v1_compatibility`
- test/test_transforms_v2.py:1518:25: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:1571:21: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:1659:21: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:1714:21: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:1784:13: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:2108:13: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:2145:21: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:2255:21: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:2319:21: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
- test/test_transforms_v2.py:2358:13: error[missing-argument] No argument provided for required parameter `degrees` of bound method `__init__`
+ test/test_transforms_v2.py:3515:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Literal["constant", "edge", "reflect", "symmetric"]`, found `list[@Todo(list comprehension element type)] | Unknown`
- test/test_transforms_v2.py:3473:18: error[missing-argument] No arguments provided for required parameters `top`, `left`, `height`, `width` of function `crop`
- test/test_transforms_v2.py:3474:31: error[missing-argument] No arguments provided for required parameters `top`, `left`, `height`, `width` of function `crop`
- test/test_transforms_v2.py:3515:13: error[missing-argument] No argument provided for required parameter `size` of bound method `__init__`
- test/test_transforms_v2.py:3567:21: error[missing-argument] No argument provided for required parameter `size` of bound method `__init__`
- test/test_transforms_v2.py:3603:18: error[missing-argument] No arguments provided for required parameters `top`, `left`, `height`, `width` of function `crop`
- test/test_transforms_v2.py:3654:18: error[missing-argument] No arguments provided for required parameters `top`, `left`, `height`, `width` of function `crop`
- test/test_transforms_v2.py:3791:18: error[missing-argument] No arguments provided for required parameters `i`, `j`, `h`, `w`, `v` of function `erase`
- test/test_transforms_v2.py:4452:18: error[missing-argument] No arguments provided for required parameters `top`, `left`, `height`, `width` of function `resized_crop`
- test/test_transforms_v2.py:4456:13: error[missing-argument] No arguments provided for required parameters `top`, `left`, `height`, `width` of function `resized_crop`
- test/test_transforms_v2.py:4499:18: error[missing-argument] No arguments provided for required parameters `top`, `left`, `height`, `width` of function `resized_crop`
- test/test_transforms_v2.py:4538:18: error[missing-argument] No arguments provided for required parameters `top`, `left`, `height`, `width` of function `resized_crop`
- Found 1468 diagnostics
+ Found 1442 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/poolmanager.py:147:12: error[missing-argument] No arguments provided for required parameters `key_scheme`, `key_host`, `key_port`, `key_timeout`, `key_retries`, `key_block`, `key_source_address`, `key_key_file`, `key_key_password`, `key_cert_file`, `key_cert_reqs`, `key_ca_certs`, `key_ca_cert_data`, `key_ssl_version`, `key_ssl_minimum_version`, `key_ssl_maximum_version`, `key_ca_cert_dir`, `key_ssl_context`, `key_maxsize`, `key_headers`, `key__proxy`, `key__proxy_headers`, `key__proxy_config`, `key_socket_options`, `key__socks_options`, `key_assert_hostname`, `key_assert_fingerprint`, `key_server_hostname`, `key_blocksize`
- test/test_util.py:586:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 388 diagnostics
+ Found 386 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/pq/pq_ctypes.py:1127:23: error[missing-argument] No arguments provided for required parameters `keyword`, `envvar`, `compiled`, `val`, `label`, `dispchar`, `dispsize`
- tests/test_rows.py:175:12: error[missing-argument] No arguments provided for required parameters `first`, `last` of bound method `__init__`
- Found 701 diagnostics
+ Found 699 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/RL/BaseReinforcementLearningModel.py:505:15: error[missing-argument] No arguments provided for required parameters `reward_kwargs`, `config`, `df_raw` of bound method `__init__`
- Found 386 diagnostics
+ Found 385 diagnostics

mypy (https://github.com/python/mypy)
- mypy/dmypy_server.py:986:18: error[missing-argument] No argument provided for required parameter `json` of bound method `__init__`
- mypy/stubtest.py:81:12: error[missing-argument] No argument provided for required parameter `color` of bound method `style`
- Found 1860 diagnostics
+ Found 1858 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/auth_oidc.py:264:29: error[missing-argument] No argument provided for required parameter `issuer`
- pymongo/asynchronous/collection.py:2721:21: error[missing-argument] No argument provided for required parameter `definition` of bound method `__init__`
- pymongo/synchronous/auth_oidc.py:262:29: error[missing-argument] No argument provided for required parameter `issuer`
- pymongo/synchronous/collection.py:2718:21: error[missing-argument] No argument provided for required parameter `definition` of bound method `__init__`
- Found 514 diagnostics
+ Found 510 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:2771:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_subprocess.py:739:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 675 diagnostics
+ Found 673 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/ext/apidoc/_cli.py:306:12: error[missing-argument] No arguments provided for required parameters `dest_dir`, `module_path`
- Found 519 diagnostics
+ Found 518 diagnostics

arviz (https://github.com/arviz-devs/arviz)
+ arviz/data/io_numpyro.py:56:67: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[str, Any] | None`
- arviz/plots/backends/matplotlib/posteriorplot.py:77:9: error[missing-argument] No argument provided for required parameter `linewidth` of function `_plot_posterior_op`
+ arviz/plots/backends/matplotlib/ppcplot.py:407:47: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | None`
+ arviz/plots/backends/matplotlib/ppcplot.py:418:52: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | None`
- arviz/plots/hdiplot.py:187:18: error[missing-argument] No arguments provided for required parameters `window_length`, `polyorder` of function `savgol_filter`
- arviz/stats/stats.py:578:20: error[missing-argument] No arguments provided for required parameters `hdi_prob`, `skipna`, `max_modes` of function `_hdi_multimodal`
- arviz/stats/stats.py:578:20: error[missing-argument] No arguments provided for required parameters `hdi_prob`, `circular`, `skipna` of function `_hdi`
- arviz/tests/base_tests/test_plots_bokeh.py:1287:10: error[missing-argument] No argument provided for required parameter `y` of function `plot_lm`
- arviz/tests/base_tests/test_plots_matplotlib.py:1897:10: error[missing-argument] No arguments provided for required parameters `point_estimate`, `hdi_prob`, `quartiles`, `linewidth`, `markersize`, `markercolor`, `marker`, `rotated`, `intervalcolor` of function `plot_point_interval`
- arviz/tests/base_tests/test_plots_matplotlib.py:2162:10: error[missing-argument] No argument provided for required parameter `y` of function `plot_lm`
- Found 710 diagnostics
+ Found 706 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/config/_apply_pyprojecttoml.py:271:12: error[missing-argument] No arguments provided for required parameters `name`, `sources` of bound method `__init__`
+ setuptools/tests/test_wheel.py:677:13: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `str | dict[@Todo(dict literal key type), @Todo(dict literal value type)] | dict[str, list[Unknown | str]] | Unknown`

discord.py (https://github.com/Rapptz/discord.py)
- discord/activity.py:879:16: error[missing-argument] No argument provided for required parameter `name` of bound method `__init__`
+ discord/activity.py:891:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/http.py:650:25: error[missing-argument] No arguments provided for required parameters `name`, `value` of bound method `add_field`
- discord/webhook/async_.py:178:25: error[missing-argument] No arguments provided for required parameters `name`, `value` of bound method `add_field`
- Found 514 diagnostics
+ Found 512 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/coverstore/coverlib.py:46:12: error[missing-argument] No arguments provided for required parameters `category`, `olid`, `filename`, `filename_s`, `filename_m`, `filename_l`, `author`, `ip`, `source_url`, `width`, `height` of function `new`
+ openlibrary/data/db.py:89:23: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | None`

apprise (https://github.com/caronc/apprise)
- tests/test_config_file.py:98:10: error[missing-argument] No argument provided for required parameter `path` of bound method `__init__`
- tests/test_plugin_discord.py:333:16: error[missing-argument] No arguments provided for required parameters `webhook_id`, `webhook_token` of bound method `__init__`
- tests/test_plugin_discord.py:380:16: error[missing-argument] No arguments provided for required parameters `webhook_id`, `webhook_token` of bound method `__init__`
- tests/test_plugin_discord.py:809:16: error[missing-argument] No arguments provided for required parameters `webhook_id`, `webhook_token` of bound method `__init__`
- tests/test_plugin_revolt.py:304:16: error[missing-argument] No arguments provided for required parameters `bot_token`, `targets` of bound method `__init__`
- tests/test_plugin_revolt.py:333:16: error[missing-argument] No arguments provided for required parameters `bot_token`, `targets` of bound method `__init__`
- tests/test_plugin_revolt.py:520:16: error[missing-argument] No arguments provided for required parameters `bot_token`, `targets` of bound method `__init__`
- tests/test_plugin_telegram.py:433:11: error[missing-argument] No arguments provided for required parameters `bot_token`, `targets` of bound method `__init__`
- tests/test_plugin_telegram.py:799:16: error[missing-argument] No arguments provided for required parameters `bot_token`, `targets` of bound method `__init__`
- Found 1879 diagnostics
+ Found 1870 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/backends/api.py:1778:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo(Inference of subscript on special form) | None | (@Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy) | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ xarray/backends/api.py:1778:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo(Inference of subscript on special form) | None | (@Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy) | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ xarray/backends/api.py:1778:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `Literal["coordinates", "all"] | bool | None`, found `@Todo(Inference of subscript on special form) | None | (@Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy) | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ xarray/backends/api.py:1778:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo(Inference of subscript on special form) | None | (@Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy) | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ xarray/backends/api.py:1778:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo(Inference of subscript on special form) | None | (@Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy) | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ xarray/backends/api.py:1778:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `str | None`, found `@Todo(Inference of subscript on special form) | None | (@Todo(Support for `typing.TypeAlias`) & ~AlwaysFalsy) | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ xarray/core/common.py:460:37: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `Hashable`
+ xarray/core/dataset.py:1334:30: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Hashable`
- xarray/core/datatree.py:1070:16: error[missing-argument] No arguments provided for required parameters `variables`, `coord_names` of bound method `_construct_direct`
+ xarray/core/groupby.py:1496:53: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `Hashable`
- xarray/core/variable.py:2027:32: error[missing-argument] No argument provided for required parameter `q` of function `quantile`
- xarray/plot/dataarray_plot.py:902:20: error[missing-argument] No argument provided for required par...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2025-09-17 10:36_

---

_Comment by @github-actions[bot] on 2025-09-17 10:41_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 790 | 1 | 0 |
| `missing-argument` | 0 | 600 | 10 |
| `unused-ignore-comment` | 13 | 12 | 0 |
| `no-matching-overload` | 6 | 9 | 0 |
| `unknown-argument` | 14 | 0 | 0 |
| `unresolved-attribute` | 10 | 1 | 0 |
| `possibly-unbound-attribute` | 0 | 0 | 6 |
| `possibly-unbound-implicit-call` | 0 | 0 | 4 |
| `too-many-positional-arguments` | 0 | 0 | 4 |
| `invalid-return-type` | 1 | 2 | 0 |
| `call-non-callable` | 0 | 1 | 0 |
| `type-assertion-failure` | 1 | 0 | 0 |
| **Total** | **835** | **626** | **24** |

**[Full report with detailed diff](https://dhruv-keywords-argument.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @dhruvmanila on 2025-09-18 06:19_

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2025-09-18 06:19_

---

_Comment by @dhruvmanila on 2025-09-18 06:47_

## Ecosystem analysis

In `boostedblob` and `mkosi`, all of the new diagnostics are due to the fact that the assignment has been annotated as `dict[str, Any]` but the inferred type is `dict[str, <class 'int'> | str | Unknown | int]`, so ty will try to check assignment between `<class 'int'> | str | Unknown | int` and the matching parameter type.

In `colour`, all of the new diagnostics are due to:
```python
def optional[T](value: T | None, default: T) -> T: ...

def _(value: dict | None = None):
    # Here, `None` shouldn't be present
    # revealed: dict[Unknown, Unknown] | None 
    reveal_type(optional(value), {})
```

In `home-assistant/core`, there's a case which requires support for narrowing on `TypedDict` (?). The scenario is that the type of a variable is a union of two `TypedDict` and the `if` condition is checking whether a key exists in the union type and narrows based on that. For example:
```python
from typing import TypedDict


class Foo1(TypedDict):
    a: int


class Foo2(TypedDict):
    b: str


def _(f: Foo1 | Foo2):
    if "a" in f:
        # This should reveal `Foo1`
        reveal_type(f)
    else:
        # This should reveal `Foo2`
        reveal_type(f)
```

In `hydra-zen`, all of the new diagnostics are because ty doesn't support `TypeGuard` narrowing.

In `pandas`, the diagnostics are valid, Pyright raises them as well but the specific rule has been disabled in the project config for Pyright.

~_I'm still looking at a few more but this is ready for review._~

---

_Comment by @dhruvmanila on 2025-09-18 06:53_

The benchmark is failing because ty is now raising more number of diagnostics than the current limit for `static-frame`

https://github.com/astral-sh/ruff/blob/fed9d6d02cc97c078bee3992820034f16c3e6f49/crates/ruff_benchmark/benches/ty_walltime.rs#L221-L237

I'll increase the limit to 600.

The new diagnostics seems valid and Pyright raises them too. They're due to the following pattern where the `data` is being splatted to the parameters but the value type that ty infers is going to be a union of all the value types present in the `dict` creation:
```py
data = dict(...)

func(**data)
```

Here's an example in the `static-frame` repository: https://github.com/static-frame/static-frame/blob/9021541074a1b9b3f51fbb4cab545ad2ddbc7d0b/static_frame/core/interface.py#L1525-L1537

Pyright raises these errors as well but it's being silenced using `# pyright: ignore`.

---

_Comment by @sharkdp on 2025-09-18 06:54_

> there's a case which requires support for narrowing on `TypedDict` (?). The scenario is that the type of a variable is a union of two `TypedDict` and the `if` condition is checking whether a key exists in the union type and narrows based on that.

This sounds *somewhat* similar to https://github.com/astral-sh/ty/issues/643. Maybe we could add a comment there that this is another use-case?

---

_Comment by @github-actions[bot] on 2025-09-18 07:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2025-09-18 08:42_

---

_Review requested from @carljm by @dhruvmanila on 2025-09-18 08:42_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-09-18 08:42_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-09-18 08:42_

---

_Review requested from @dcreager by @dhruvmanila on 2025-09-18 08:42_

---

_Comment by @carljm on 2025-09-18 17:37_

Looking at the conformance suite, it seems the issues there are related to still lacking `Unpack` and `ParamSpec` support?


---

_Comment by @carljm on 2025-09-18 17:40_

> In `boostedblob` and `mkosi`, all of the new diagnostics are due to the fact that the assignment has been annotated as `dict[str, Any]` but the inferred type is `dict[str, <class 'int'> | str | Unknown | int]`

So this would be another case to consider in https://github.com/astral-sh/ty/issues/136

> In `colour`, all of the new diagnostics are due to:
> 
> ```python
> def optional[T](value: T | None, default: T) -> T: ...
> 
> def _(value: dict | None = None):
>     # Here, `None` shouldn't be present
>     # revealed: dict[Unknown, Unknown] | None 
>     reveal_type(optional(value), {})
> ```

And this is https://github.com/astral-sh/ty/issues/623

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/function.md`:1051 on 2025-09-18 21:13_

What about if the key type is `Any` or `Unknown`? (That is, something assignable to `str` but not a subtype of `str`.) It looks like this is handled, but not tested.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/function.md`:1101 on 2025-09-18 21:15_

It's not the "key" that needs to be a mapping.

```suggestion
### Not a mapping
```

---

_@carljm approved on 2025-09-18 22:09_

Awesome!

---

_Comment by @dhruvmanila on 2025-09-19 04:53_

> Looking at the conformance suite, it seems the issues there are related to still lacking `Unpack` and `ParamSpec` support?

Yeah, that's correct.

---

_@dhruvmanila reviewed on 2025-09-19 04:55_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/function.md`:1051 on 2025-09-19 04:55_

Added a test case for these cases, thanks!

---

_@dhruvmanila reviewed on 2025-09-19 04:56_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/function.md`:1101 on 2025-09-19 04:56_

Right! Thanks for catching it

---

_Merged by @dhruvmanila on 2025-09-19 05:00_

---

_Closed by @dhruvmanila on 2025-09-19 05:00_

---

_Branch deleted on 2025-09-19 05:00_

---

_Comment by @dhruvmanila on 2025-09-19 05:03_

(Happy to address any other feedback on this PR in a follow-up :))

---
