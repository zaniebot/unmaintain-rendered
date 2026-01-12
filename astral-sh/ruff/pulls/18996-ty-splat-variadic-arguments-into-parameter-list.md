```yaml
number: 18996
title: "[ty] Splat variadic arguments into parameter list"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/splat
created_at: 2025-06-27T19:31:20Z
updated_at: 2025-07-22T18:33:10Z
url: https://github.com/astral-sh/ruff/pull/18996
synced_at: 2026-01-12T15:56:29Z
```

# [ty] Splat variadic arguments into parameter list

---

_@dcreager_

This PR updates our call binding logic to handle splatted arguments.

Complicating matters is that we have separated call bind analysis into two phases: parameter matching and type checking. Parameter matching looks at the arity of the function signature and call site, and assigns arguments to parameters. Importantly, we don't yet know the type of each argument! This is needed so that we can decide whether to infer the type of each argument as a type form or value form, depending on the requirements of the parameter that the argument was matched to.

This is an issue when splatting an argument, since we need to know how many elements the splatted argument contains to know how many positional parameters to match it against. And to know how many elements the splatted argument has, we need to know its type.

To get around this, we now make the assumption that splatted arguments can only be used with value-form parameters. (If you end up splatting an argument into a type-form parameter, we will silently pass in its value-form type instead.) That allows us to preemptively infer the (value-form) type of any splatted argument, so that we have its arity available during parameter matching. We defer inference of non-splatted arguments until after parameter matching has finished, as before.

We reuse a lot of the new tuple machinery to make this happen — in particular resizing the tuple spec representing the number of arguments passed in with the tuple length representing the number of parameters the splat was matched with.

This work also shows that we might need to change how we are performing argument expansion during overload resolution. At the moment, when we expand parameters, we assume that each argument will still be matched to the same parameters as before, and only retry the type-checking phase. With splatted arguments, this is no longer the case, since the inferred arity of each union element might be different than the arity of the union as a whole, which can affect how many parameters the splatted argument is matched to. See the regression test case in `mdtest/call/function.md` for more details.

---

_Label `ty` added by @dcreager on 2025-06-27 19:34_

---

_Comment by @codspeed-hq[bot] on 2025-06-27 21:15_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fsplat?runnerMode=WallTime)

### Merging #18996 will **not alter performance**

<sub>Comparing <code>dcreager/splat</code> (b1b6087) with <code>main</code> (9d5ecac)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Comment by @codspeed-hq[bot] on 2025-06-27 21:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fsplat?runnerMode=Instrumentation)

### Merging #18996 will **not alter performance**

<sub>Comparing <code>dcreager/splat</code> (b1b6087) with <code>main</code> (9d5ecac)</sub>



### Summary

`✅ 41` untouched benchmarks  





---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/call/function.md`:237 on 2025-06-27 22:45_

fyi @dhruvmanila 

---

_Comment by @github-actions[bot] on 2025-06-27 22:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- error[no-matching-overload] more_itertools/more.py:1246:15: No overload of function `__new__` matches arguments
- error[no-matching-overload] more_itertools/more.py:1883:16: No overload of function `__new__` matches arguments
- error[no-matching-overload] more_itertools/more.py:1937:24: No overload of function `__new__` matches arguments
- error[no-matching-overload] more_itertools/more.py:1950:29: No overload of function `__new__` matches arguments
- error[no-matching-overload] more_itertools/more.py:2604:49: No overload of function `__new__` matches arguments
- error[no-matching-overload] more_itertools/more.py:3636:31: No overload of function `__new__` matches arguments
- error[no-matching-overload] more_itertools/recipes.py:367:18: No overload of function `__new__` matches arguments
- error[no-matching-overload] more_itertools/recipes.py:421:16: No overload of function `__new__` matches arguments
- error[missing-argument] more_itertools/recipes.py:1122:12: No arguments provided for required parameters `x`, `y`
- Found 40 diagnostics
+ Found 31 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[missing-argument] src/anyio/_backends/_asyncio.py:2561:29: No argument provided for required parameter `program` of function `create_subprocess_exec`
- error[missing-argument] src/anyio/_core/_sockets.py:916:23: No arguments provided for required parameters `host`, `port`
- Found 95 diagnostics
+ Found 93 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- error[missing-argument] src/async_utils/corofunc_cache.py:107:20: No arguments provided for required parameters `args`, `kwds` of function `make_key`
+ error[invalid-argument-type] src/async_utils/corofunc_cache.py:107:29: Argument to function `make_key` is incorrect: Expected `tuple[Any, ...]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
+ error[invalid-argument-type] src/async_utils/corofunc_cache.py:107:29: Argument to function `make_key` is incorrect: Expected `Mapping[Any, object]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- error[missing-argument] src/async_utils/corofunc_cache.py:186:20: No arguments provided for required parameters `args`, `kwds` of function `make_key`
+ error[invalid-argument-type] src/async_utils/corofunc_cache.py:186:29: Argument to function `make_key` is incorrect: Expected `tuple[Any, ...]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
+ error[invalid-argument-type] src/async_utils/corofunc_cache.py:186:29: Argument to function `make_key` is incorrect: Expected `Mapping[Any, object]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- error[missing-argument] src/async_utils/gen_transform.py:176:12: No arguments provided for required parameters `g`, `f` of bound method `__init__`
- error[missing-argument] src/async_utils/task_cache.py:167:20: No arguments provided for required parameters `args`, `kwds` of function `make_key`
+ error[invalid-argument-type] src/async_utils/task_cache.py:167:29: Argument to function `make_key` is incorrect: Expected `tuple[Any, ...]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
+ error[invalid-argument-type] src/async_utils/task_cache.py:167:29: Argument to function `make_key` is incorrect: Expected `Mapping[Any, object]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- error[missing-argument] src/async_utils/task_cache.py:253:20: No arguments provided for required parameters `args`, `kwds` of function `make_key`
+ error[invalid-argument-type] src/async_utils/task_cache.py:253:29: Argument to function `make_key` is incorrect: Expected `tuple[Any, ...]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
+ error[invalid-argument-type] src/async_utils/task_cache.py:253:29: Argument to function `make_key` is incorrect: Expected `Mapping[Any, object]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- Found 16 diagnostics
+ Found 19 diagnostics

parso (https://github.com/davidhalter/parso)
- error[missing-argument] parso/utils.py:132:12: No arguments provided for required parameters `major`, `minor`, `micro`
- Found 75 diagnostics
+ Found 74 diagnostics

paroxython (https://github.com/laowantong/paroxython)
- error[missing-argument] paroxython/parse_program.py:316:19: No arguments provided for required parameters `name`, `spans`
- error[missing-argument] paroxython/parse_program.py:333:31: No arguments provided for required parameters `name`, `spans`
- Found 7 diagnostics
+ Found 5 diagnostics

beartype (https://github.com/beartype/beartype)
- error[missing-argument] beartype/_util/func/arg/utilfuncargget.py:96:21: No argument provided for required parameter `func` of function `iter_func_args`
- error[missing-argument] beartype/_util/func/arg/utilfuncargget.py:163:21: No argument provided for required parameter `func` of function `iter_func_args`
- error[missing-argument] beartype/_util/func/arg/utilfuncargget.py:209:22: No argument provided for required parameter `func` of function `get_func_arg_names`
- error[missing-argument] beartype/_util/func/arg/utilfuncarglen.py:144:9: No argument provided for required parameter `func` of function `get_func_args_lens`
- error[missing-argument] beartype/_util/func/arg/utilfuncarglen.py:438:22: No argument provided for required parameter `func` of function `get_func_args_lens`
- error[missing-argument] beartype/_util/func/arg/utilfuncargtest.py:267:22: No argument provided for required parameter `func` of function `get_func_args_lens`
- error[missing-argument] beartype/_util/func/arg/utilfuncargtest.py:299:22: No argument provided for required parameter `func` of function `get_func_args_lens`
- error[missing-argument] beartype/_util/func/arg/utilfuncargtest.py:328:22: No argument provided for required parameter `func` of function `get_func_args_lens`
- error[missing-argument] beartype/_util/func/arg/utilfuncargtest.py:363:17: No argument provided for required parameter `func` of function `get_func_arg_names`
- warning[unused-ignore-comment] beartype/_util/func/arg/utilfuncargtest.py:418:66: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] beartype/_util/func/arg/utilfuncargtest.py:477:66: Unused blanket `type: ignore` directive
- error[no-matching-overload] beartype/_util/kind/map/utilmapfrozen.py:56:20: No overload of bound method `fromkeys` matches arguments
- error[missing-argument] beartype/_util/module/utilmodimport.py:507:19: No argument provided for required parameter `attr_name` of function `import_module_attr_or_sentinel`
- error[missing-argument] beartype/claw/_importlib/clawimpcache.py:229:12: No argument provided for required parameter `path` of function `cache_from_source`
- Found 577 diagnostics
+ Found 563 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- error[missing-argument] src/pegen/sccutils.py:89:17: No argument provided for required parameter `self` of function `union`
- Found 47 diagnostics
+ Found 46 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[missing-argument] chess/engine.py:1171:22: No argument provided for required parameter `program` of bound method `subprocess_exec`
- Found 17 diagnostics
+ Found 16 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[missing-argument] mypy_primer/utils.py:104:26: No argument provided for required parameter `program` of function `create_subprocess_exec`
- Found 10 diagnostics
+ Found 9 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- error[missing-argument] src/bandersnatch_storage_plugins/swift.py:172:16: No argument provided for required parameter `path` of bound method `mkdir`
- error[missing-argument] src/bandersnatch_storage_plugins/swift.py:178:13: No argument provided for required parameter `path` of bound method `delete_file`
- error[missing-argument] src/bandersnatch_storage_plugins/swift.py:187:16: No arguments provided for required parameters `source`, `dest` of bound method `copy_file`
- error[missing-argument] src/bandersnatch_storage_plugins/swift.py:192:13: No argument provided for required parameter `path` of bound method `rmdir`
- error[missing-argument] src/bandersnatch_storage_plugins/swift.py:199:16: No arguments provided for required parameters `source`, `dest` of bound method `copy_file`
- error[missing-argument] src/bandersnatch_storage_plugins/swift.py:203:16: No arguments provided for required parameters `source`, `dest` of bound method `copy_file`
- Found 129 diagnostics
+ Found 123 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[no-matching-overload] expression/collections/asyncseq.py:38:25: No overload of function `range` matches arguments
- error[no-matching-overload] expression/collections/asyncseq.py:79:18: No overload of function `__new__` matches arguments
- error[no-matching-overload] expression/collections/block.py:347:16: No overload of function `range` matches arguments
- error[no-matching-overload] expression/collections/block.py:866:20: No overload of function `__new__` matches arguments
- error[no-matching-overload] expression/collections/seq.py:260:16: No overload of function `range` matches arguments
- error[no-matching-overload] expression/collections/seq.py:812:16: No overload of function `__new__` matches arguments
- error[missing-argument] tests/test_pipe.py:88:40: No arguments provided for required parameters `a`, `b` of function `gn`
- error[missing-argument] tests/test_pipe.py:92:44: No arguments provided for required parameters `a`, `b` of function `yn`
- error[missing-argument] tests/test_pipe.py:92:48: No arguments provided for required parameters `a`, `b` of function `gn`
+ error[invalid-argument-type] tests/test_pipe.py:88:43: Argument to function `gn` is incorrect: Expected `tuple[Literal[1], Literal[2]]`, found `Literal[1]`
+ error[invalid-argument-type] tests/test_pipe.py:88:43: Argument to function `gn` is incorrect: Expected `tuple[Literal[1], Literal[2]]`, found `Literal[2]`
+ error[invalid-argument-type] tests/test_pipe.py:92:47: Argument to function `yn` is incorrect: Expected `tuple[tuple[Literal[1], Literal[2]], tuple[Literal[1], Literal[2]]]`, found `tuple[Literal[1], Literal[2]]`
+ error[invalid-argument-type] tests/test_pipe.py:92:47: Argument to function `yn` is incorrect: Expected `tuple[tuple[Literal[1], Literal[2]], tuple[Literal[1], Literal[2]]]`, found `tuple[Literal[1], Literal[2]]`
+ error[invalid-argument-type] tests/test_pipe.py:92:51: Argument to function `gn` is incorrect: Expected `tuple[Literal[1], Literal[2]]`, found `Literal[1]`
+ error[invalid-argument-type] tests/test_pipe.py:92:51: Argument to function `gn` is incorrect: Expected `tuple[Literal[1], Literal[2]]`, found `Literal[2]`
- Found 230 diagnostics
+ Found 227 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- error[missing-argument] src/aiortc/rtcrtpreceiver.py:476:29: No arguments provided for required parameters `bitrate`, `ssrcs` of function `pack_remb_fci`
- Found 94 diagnostics
+ Found 93 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[missing-argument] src/graphql/execution/execute.py:1352:30: No arguments provided for required parameters `new_grouped_field_set_details_map`, `new_defer_usages`
+ error[too-many-positional-arguments] src/graphql/execution/execute.py:1352:56: Too many positional arguments: expected 4, got 5
- error[missing-argument] tests/language/test_visitor.py:242:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:247:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:315:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:344:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:419:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:429:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:461:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:471:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:501:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:508:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:541:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:548:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:555:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:693:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:700:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:753:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1393:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1403:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1438:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1448:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1501:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1511:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1544:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1555:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1594:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1601:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1638:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1645:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1704:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1713:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- error[missing-argument] tests/language/test_visitor.py:1774:17: No arguments provided for required parameters `node`, `key`, `parent`, `path`, `ancestors` of function `check_visitor_fn_args`
- Found 357 diagnostics
+ Found 326 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- error[missing-argument] koda_validate/typehints.py:138:24: No argument provided for required parameter `validator_1` of bound method `__init__`
- error[missing-argument] koda_validate/typehints.py:171:20: No argument provided for required parameter `validator_1` of bound method `__init__`
- Found 40 diagnostics
+ Found 38 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- error[missing-argument] dulwich/contrib/release_robot.py:95:21: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- error[missing-argument] dulwich/contrib/release_robot.py:108:17: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- error[missing-argument] dulwich/porcelain.py:3520:19: No argument provided for required parameter `msg` of bound method `__init__`
- error[missing-argument] dulwich/porcelain.py:3747:17: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- error[missing-argument] dulwich/protocol.py:157:13: No arguments provided for required parameters `from_ref`, `to_ref` of function `capability_symref`
- error[missing-argument] dulwich/server.py:1137:9: No arguments provided for required parameters `request`, `client_address`, `server` of function `__init__`
- error[missing-argument] dulwich/web.py:509:11: No argument provided for required parameter `backend` of bound method `__init__`
- error[missing-argument] dulwich/web.py:527:9: No argument provided for required parameter `msg` of bound method `error`
- error[missing-argument] dulwich/web.py:543:9: No argument provided for required parameter `msg` of bound method `error`
- Found 170 diagnostics
+ Found 161 diagnostics

starlette (https://github.com/encode/starlette)
- error[missing-argument] starlette/requests.py:155:20: No arguments provided for required parameters `host`, `port`
- error[no-matching-overload] starlette/staticfiles.py:107:33: No overload of function `join` matches arguments
- error[missing-argument] starlette/testclient.py:691:32: No arguments provided for required parameters `send_stream`, `receive_stream`
- error[missing-argument] starlette/testclient.py:692:35: No arguments provided for required parameters `send_stream`, `receive_stream`
+ error[invalid-argument-type] starlette/testclient.py:691:52: Argument is incorrect: Expected `ObjectSendStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
+ error[invalid-argument-type] starlette/testclient.py:691:52: Argument is incorrect: Expected `ObjectReceiveStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
+ error[invalid-argument-type] starlette/testclient.py:692:55: Argument is incorrect: Expected `ObjectSendStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
+ error[invalid-argument-type] starlette/testclient.py:692:55: Argument is incorrect: Expected `ObjectReceiveStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`

comtypes (https://github.com/enthought/comtypes)
- error[missing-argument] comtypes/_comobject.py:256:40: No arguments provided for required parameters `guid`, `wMajorVerNum`, `wMinorVerNum` of function `LoadRegTypeLib`
+ warning[unused-ignore-comment] comtypes/_memberspec.py:85:61: Unused blanket `type: ignore` directive
- error[missing-argument] comtypes/automation.py:331:18: No arguments provided for required parameters `rGuidTypeLib`, `verMajor`, `verMinor`, `lcid`, `rGuidTypeInfo` of function `GetRecordInfoFromGuids`
- error[missing-argument] comtypes/automation.py:381:22: No arguments provided for required parameters `rGuidTypeLib`, `verMajor`, `verMinor`, `lcid`, `rGuidTypeInfo` of function `GetRecordInfoFromGuids`
- error[missing-argument] comtypes/automation.py:403:22: No arguments provided for required parameters `rGuidTypeLib`, `verMajor`, `verMinor`, `lcid`, `rGuidTypeInfo` of function `GetRecordInfoFromGuids`
- error[missing-argument] comtypes/client/_generate.py:151:16: No arguments provided for required parameters `wMajorVerNum`, `wMinorVerNum` of function `LoadRegTypeLib`
- error[missing-argument] comtypes/client/_generate.py:154:16: No arguments provided for required parameters `wMajorVerNum`, `wMinorVerNum` of function `LoadRegTypeLib`
- error[missing-argument] comtypes/safearray.py:101:25: No arguments provided for required parameters `rGuidTypeLib`, `verMajor`, `verMinor`, `lcid`, `rGuidTypeInfo` of function `GetRecordInfoFromGuids`
- error[missing-argument] comtypes/server/connectionpoints.py:128:16: No arguments provided for required parameters `guid`, `wMajorVerNum`, `wMinorVerNum` of function `LoadRegTypeLib`
- error[missing-argument] comtypes/server/register.py:233:17: No arguments provided for required parameters `libID`, `wVerMajor`, `wVerMinor` of function `UnRegisterTypeLib`
- error[missing-argument] comtypes/test/test_midl_safearray_create.py:63:17: No arguments provided for required parameters `rGuidTypeLib`, `verMajor`, `verMinor`, `lcid`, `rGuidTypeInfo` of function `GetRecordInfoFromGuids`
- error[missing-argument] comtypes/test/test_recordinfo.py:19:12: No arguments provided for required parameters `rGuidTypeLib`, `verMajor`, `verMinor`, `lcid`, `rGuidTypeInfo` of function `GetRecordInfoFromGuids`
+ error[invalid-argument-type] comtypes/test/test_typeinfo.py:57:38: Argument to function `QueryPathOfRegTypeLib` is incorrect: Expected `str`, found `Unknown | GUID`
- error[missing-argument] comtypes/test/test_typeinfo.py:35:22: No arguments provided for required parameters `guid`, `wMajorVerNum`, `wMinorVerNum` of function `LoadRegTypeLib`
- error[missing-argument] comtypes/test/test_typeinfo.py:57:16: No arguments provided for required parameters `libid`, `wVerMajor`, `wVerMinor` of function `QueryPathOfRegTypeLib`
- error[missing-argument] comtypes/tools/codegenerator/helpers.py:151:20: No arguments provided for required parameters `type_name`, `arg_name`, `idlflags`, `default` of function `_to_arg_definition`
- error[missing-argument] comtypes/tools/codegenerator/helpers.py:255:20: No arguments provided for required parameters `type_name`, `arg_name`, `idlflags`, `default` of function `_to_arg_definition`
- Found 423 diagnostics
+ Found 410 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[missing-argument] psycopg/psycopg/pq/pq_ctypes.py:306:23: No arguments provided for required parameters `arg1`, `arg2`, `arg3`, `arg4`, `arg5`, `arg6`, `arg7`, `arg8` of function `PQexecParams`
- error[missing-argument] psycopg/psycopg/pq/pq_ctypes.py:324:16: No arguments provided for required parameters `arg1`, `arg2`, `arg3`, `arg4`, `arg5`, `arg6`, `arg7`, `arg8` of function `PQsendQueryParams`
- error[missing-argument] psycopg/psycopg/pq/pq_ctypes.py:361:16: No arguments provided for required parameters `arg1`, `arg2`, `arg3`, `arg4`, `arg5`, `arg6`, `arg7` of function `PQsendQueryPrepared`
- error[missing-argument] tests/types/test_datetime.py:784:12: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- error[missing-argument] tests/types/test_multirange.py:308:15: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- error[missing-argument] tests/types/test_multirange.py:309:15: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- error[missing-argument] tests/types/test_range.py:206:15: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- error[missing-argument] tests/types/test_range.py:207:15: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- error[no-matching-overload] tests/utils.py:162:14: No overload of function `raises` matches arguments
- Found 654 diagnostics
+ Found 645 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- error[missing-argument] dedupe/branch_and_bound.py:13:16: No argument provided for required parameter `self` of function `union`
- error[missing-argument] dedupe/branch_and_bound.py:49:17: No argument provided for required parameter `self` of function `union`
- error[missing-argument] dedupe/training.py:57:27: No argument provided for required parameter `self` of function `union`
- error[missing-argument] dedupe/training.py:284:20: No argument provided for required parameter `self` of function `union`
- Found 58 diagnostics
+ Found 54 diagnostics

svcs (https://github.com/hynek/svcs)
- error[no-matching-overload] src/svcs/aiohttp.py:284:18: No overload of bound method `aget` matches arguments
- error[no-matching-overload] src/svcs/flask.py:82:12: No overload of function `get` matches arguments
- error[no-matching-overload] src/svcs/flask.py:325:12: No overload of bound method `get` matches arguments
- error[no-matching-overload] src/svcs/pyramid.py:200:12: No overload of bound method `get` matches arguments
- error[no-matching-overload] src/svcs/pyramid.py:329:12: No overload of bound method `get` matches arguments
- error[no-matching-overload] src/svcs/starlette.py:257:18: No overload of bound method `aget` matches arguments
- Found 85 diagnostics
+ Found 79 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[missing-argument] scrapy/core/engine.py:411:21: No arguments provided for required parameters `level`, `msg` of bound method `log`
- error[missing-argument] scrapy/core/scraper.py:213:17: No arguments provided for required parameters `level`, `msg` of bound method `log`
- error[missing-argument] scrapy/core/scraper.py:289:9: No arguments provided for required parameters `level`, `msg` of bound method `log`
- error[missing-argument] scrapy/core/scraper.py:418:17: No arguments provided for required parameters `level`, `msg` of bound method `log`
- error[missing-argument] scrapy/core/scraper.py:432:13: No arguments provided for required parameters `level`, `msg` of bound method `log`
- error[missing-argument] scrapy/core/scraper.py:447:17: No arguments provided for required parameters `level`, `msg` of bound method `log`
- error[missing-argument] scrapy/mail.py:115:45: No arguments provided for required parameters `_maintype`, `_subtype` of bound method `__init__`
- error[missing-argument] scrapy/mail.py:135:24: No arguments provided for required parameters `_maintype`, `_subtype` of bound method `__init__`
- Found 1089 diagnostics
+ Found 1081 diagnostics

rich (https://github.com/Textualize/rich)
- error[missing-argument] rich/color.py:521:24: No arguments provided for required parameters `r`, `g`, `b` of function `rgb_to_hls`
- error[missing-argument] rich/color.py:550:27: No arguments provided for required parameters `red`, `green`, `blue`
- error[missing-argument] rich/color.py:563:27: No arguments provided for required parameters `red`, `green`, `blue`
- error[missing-argument] rich/palette.py:18:16: No arguments provided for required parameters `red`, `green`, `blue`
- error[missing-argument] rich/palette.py:39:52: No arguments provided for required parameters `red`, `green`, `blue` of bound method `from_rgb`
- error[missing-argument] rich/terminal_theme.py:27:33: No arguments provided for required parameters `red`, `green`, `blue`
- error[missing-argument] rich/terminal_theme.py:28:33: No arguments provided for required parameters `red`, `green`, `blue`
- Found 321 diagnostics
+ Found 314 diagnostics

kopf (https://github.com/nolar/kopf)
- error[missing-argument] kopf/_cogs/structs/diffs.py:74:29: No arguments provided for required parameters `operation`, `field`, `old`, `new`
- Found 119 diagnostics
+ Found 118 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
- error[missing-argument] test_htmlgen/util.py:14:9: No arguments provided for required parameters `name`, `value` of bound method `add_attribute`
- Found 28 diagnostics
+ Found 27 diagnostics

trio (https://github.com/python-trio/trio)
- error[missing-argument] src/trio/_core/_tests/test_run.py:813:19: No arguments provided for required parameters `exc_type`, `exc_value`, `traceback` of bound method `__aexit__`
- error[missing-argument] src/trio/_tests/test_highlevel_open_tcp_stream.py:160:26: No arguments provided for required parameters `host`, `port` of function `open_tcp_stream`
- error[missing-argument] src/trio/_tests/test_highlevel_open_tcp_stream.py:182:19: No arguments provided for required parameters `host`, `port` of function `open_tcp_stream`
- error[missing-argument] src/trio/_tests/test_highlevel_open_tcp_stream.py:185:26: No arguments provided for required parameters `host`, `port` of function `open_tcp_stream`
- error[no-matching-overload] src/trio/_tests/test_highlevel_open_tcp_stream.py:554:12: No overload of bound method `__init__` matches arguments
- error[missing-argument] src/trio/_tests/test_ssl.py:1342:34: No arguments provided for required parameters `host`, `port` of function `open_tcp_stream`
- error[no-matching-overload] src/trio/_tests/test_subprocess.py:90:18: No overload of function `open_process` matches arguments
- Found 746 diagnostics
+ Found 739 diagnostics

httpx-caching (https://github.com/johtso/httpx-caching)
- error[missing-argument] httpx_caching/_heuristics.py:64:60: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
+ warning[unused-ignore-comment] tests/_async/test_expires_heuristics.py:149:80: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] tests/_async/test_expires_heuristics.py:173:80: Unused blanket `type: ignore` directive
- Found 28 diagnostics
+ Found 29 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[no-matching-overload] pydantic/_internal/_model_construction.py:532:14: No overload of function `__new__` matches arguments
+ warning[unused-ignore-comment] pydantic/color.py:330:43: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] pydantic/color.py:334:40: Unused blanket `type: ignore` directive
- error[no-matching-overload] pydantic/main.py:1151:26: No overload of function `__new__` matches arguments
- error[missing-argument] pydantic/main.py:1615:16: No arguments provided for required parameters `values`, `fields_set`, `deep` of function `_copy_and_set_values`
+ error[missing-argument] pydantic/main.py:1615:16: No argument provided for required parameter `deep` of function `_copy_and_set_values`
- error[missing-argument] pydantic/main.py:1630:16: No arguments provided for required parameters `v`, `to_dict`, `by_alias`, `include`, `exclude`, `exclude_unset`, `exclude_defaults`, `exclude_none` of function `_get_value`
- error[missing-argument] pydantic/main.py:1644:16: No arguments provided for required parameters `include`, `exclude`, `exclude_unset` of function `_calculate_keys`
+ warning[unused-ignore-comment] pydantic/v1/color.py:265:43: Unused blanket `type: ignore` directive
- error[missing-argument] pydantic/v1/env_settings.py:76:20: No argument provided for required parameter `mapping` of function `deep_update`
- Found 779 diagnostics
+ Found 777 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ error[parameter-already-assigned] discord/ext/commands/bot.py:297:48: Multiple values provided for parameter 1 (`name`) of function `hybrid_command`
+ error[parameter-already-assigned] discord/ext/commands/bot.py:321:46: Multiple values provided for parameter 1 (`name`) of function `hybrid_group`
+ error[no-matching-overload] discord/ext/commands/core.py:1520:22: No overload of function `command` matches arguments
+ error[no-matching-overload] discord/ext/commands/core.py:1579:22: No overload of function `group` matches arguments
+ error[parameter-already-assigned] discord/ext/commands/hybrid.py:841:48: Multiple values provided for parameter 1 (`name`) of function `hybrid_command`
+ error[parameter-already-assigned] discord/ext/commands/hybrid.py:865:46: Multiple values provided for parameter 1 (`name`) of function `hybrid_group`
- Found 521 diagnostics
+ Found 527 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[no-matching-overload] src/hydra_zen/structured_configs/_make_custom_builds.py:349:26: No overload of bound method `builds` matches arguments
- Found 571 diagnostics
+ Found 570 diagnostics

kornia (https://github.com/kornia/kornia)
- error[missing-argument] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:112:21: No arguments provided for required parameters `in_channels`, `mid_channels`, `out_channels` of bound method `__init__`
- error[missing-argument] kornia/contrib/object_detection.py:68:12: No arguments provided for required parameters `detections`, `format` of function `results_from_detections`
- error[missing-argument] kornia/core/mixin/image_module.py:75:34: No argument provided for required parameter 1
- error[missing-argument] kornia/core/module.py:81:34: No argument provided for required parameter 1
- error[missing-argument] kornia/feature/lightglue.py:253:26: No arguments provided for required parameters `embed_dim`, `num_heads` of bound method `__init__`
- error[missing-argument] kornia/feature/lightglue.py:254:27: No arguments provided for required parameters `embed_dim`, `num_heads` of bound method `__init__`
- error[missing-argument] kornia/filters/gaussian.py:151:12: No arguments provided for required parameters `input`, `kernel_size`, `sigma` of function `gaussian_blur2d`
- error[missing-argument] kornia/filters/kernels.py:1010:12: No arguments provided for required parameters `kernel_size`, `sigma` of function `get_gaussian_kernel1d`
- error[missing-argument] kornia/filters/kernels.py:1015:12: No arguments provided for required parameters `kernel_size`, `sigma` of function `get_gaussian_kernel2d`
- error[missing-argument] kornia/filters/kernels.py:1020:12: No arguments provided for required parameters `kernel_size`, `sigma` of function `get_gaussian_kernel3d`
- error[missing-argument] kornia/geometry/subpix/nms.py:56:40: No arguments provided for required parameters `kx`, `ky` of function `_get_nms_kernel2d`
- error[missing-argument] kornia/geometry/subpix/nms.py:97:23: No arguments provided for required parameters `kd`, `ky`, `kx` of function `_get_nms_kernel3d`
- Found 789 diagnostics
+ Found 777 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[missing-argument] porcupine/plugins/jump_to_definition.py:105:9: No arguments provided for required parameters `x`, `y` of bound method `tk_popup`
- error[missing-argument] porcupine/plugins/mergeconflict.py:160:27: No arguments provided for required parameters `start_lineno`, `middle_lineno`, `end_lineno` of bound method `__init__`
- Found 33 diagnostics
+ Found 31 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[missing-argument] pymongo/server_type.py:33:15: No arguments provided for required parameters `Unknown`, `Mongos`, `RSPrimary`, `RSSecondary`, `RSArbiter`, `RSOther`, `RSGhost`, `Standalone`, `LoadBalancer`
- error[missing-argument] pymongo/topology_description.py:54:17: No arguments provided for required parameters `Single`, `ReplicaSetNoPrimary`, `ReplicaSetWithPrimary`, `Sharded`, `Unknown`, `LoadBalanced`
- Found 425 diagnostics
+ Found 423 diagnostics

websockets (https://github.com/aaugustin/websockets)
+ error[parameter-already-assigned] src/websockets/asyncio/connection.py:1005:13: Multiple values provided for parameter `pause` of bound method `__init__`
+ error[parameter-already-assigned] src/websockets/asyncio/connection.py:1006:13: Multiple values provided for parameter `resume` of bound method `__init__`
- error[missing-argument] src/websockets/client.py:111:40: No arguments provided for required parameters `username`, `password` of function `build_authorization_basic`
- error[missing-argument] src/websockets/legacy/client.py:282:48: No arguments provided for required parameters `username`, `password` of function `build_authorization_basic`
- error[missing-argument] src/websockets/legacy/server.py:607:19: No arguments provided for required parameters `status`, `headers` of bound method `__init__`
+ error[invalid-argument-type] src/websockets/legacy/server.py:607:34: Argument to bound method `__init__` is incorrect: Expected `bytes`, found `@Todo(map_with_boundness: intersections with negative contributions) | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[Unknown]`

jinja (https://github.com/pallets/jinja)
- error[no-matching-overload] src/jinja2/sandbox.py:91:11: No overload of function `__new__` matches arguments
- error[no-matching-overload] tests/test_loader.py:334:26: No overload of function `join` matches arguments
- error[no-matching-overload] tests/test_loader.py:356:26: No overload of function `join` matches arguments
- error[no-matching-overload] tests/test_loader.py:379:26: No overload of function `join` matches arguments
- Found 196 diagnostics
+ Found 192 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- error[no-matching-overload] sockeye/data_io.py:197:81: No overload of function `max` matches arguments
- error[no-matching-overload] test/unit/test_arguments.py:369:16: No overload of function `__new__` matches arguments
- error[missing-argument] test/unit/test_data_io.py:353:15: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_data_io.py:398:15: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_data_io.py:431:15: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_data_io.py:603:15: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_data_io.py:660:16: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_data_io.py:662:16: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_data_io.py:729:16: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_data_io.py:731:16: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_data_io.py:762:15: No arguments provided for required parameters `source`, `target` of bound method `__init__`
- error[missing-argument] test/unit/test_inference.py:447:24: No arguments provided for required parameters `sequence`, `length`, `seq_scores`, `estimated_reference_length` of function `_assemble_translation`
- Found 321 diagnostics
+ Found 309 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[missing-argument] mkosi/__init__.py:3429:13: No arguments provided for required parameters `distribution`, `release` of function `can_orphan_file`
- error[missing-argument] mkosi/run.py:56:13: No arguments provided for required parameters 1, 2, 3
- error[missing-argument] mkosi/run.py:61:13: No arguments provided for required parameters 1, 2, 3
- error[missing-argument] mkosi/run.py:77:13: No arguments provided for required parameters 1, 2, 3
- error[missing-argument] mkosi/run.py:79:9: No arguments provided for required parameters 1, 2, 3
- Found 91 diagnostics
+ Found 86 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[missing-argument] ignite/handlers/clearml_logger.py:207:16: No argument provided for required parameter `tag` of bound method `__init__`
- error[missing-argument] ignite/handlers/clearml_logger.py:210:16: No argument provided for required parameter `optimizer` of bound method `__init__`
- error[missing-argument] ignite/handlers/mlflow_logger.py:159:16: No argument provided for required parameter `tag` of bound method `__init__`
- error[missing-argument] ignite/handlers/mlflow_logger.py:162:16: No argument provided for required parameter `optimizer` of bound method `__init__`
- error[missing-argument] ignite/handlers/neptune_logger.py:198:16: No argument provided for required parameter `tag` of bound method `__init__`
- error[missing-argument] ignite/handlers/neptune_logger.py:201:16: No argument provided for required parameter `optimizer` of bound method `__init__`
- error[missing-argument] ignite/handlers/polyaxon_logger.py:132:16: No argument provided for required parameter `tag` of bound method `__init__`
- error[missing-argument] ignite/handlers/polyaxon_logger.py:135:16: No argument provided for required parameter `optimizer` of bound method `__init__`
- error[missing-argument] ignite/handlers/tensorboard_logger.py:215:16: No argument provided for required parameter `tag` of bound method `__init__`
- error[missing-argument] ignite/handlers/tensorboard_logger.py:218:16: No argument provided for required parameter `optimizer` of bound method `__init__`
- error[missing-argument] ignite/handlers/tqdm_logger.py:233:16: No argument provided for required parameter `description` of bound method `__init__`
- error[missing-argument] ignite/handlers/visdom_logger.py:212:16: No argument provided for required parameter `tag` of bound method `__init__`
- error[missing-argument] ignite/handlers/visdom_logger.py:215:16: No argument provided for required parameter `optimizer` of bound method `__init__`
- error[missing-argument] ignite/handlers/wandb_logger.py:153:16: No argument provided for required parameter `tag` of bound method `__init__`
- error[missing-argument] ignite/handlers/wandb_logger.py:156:16: No argument provided for required parameter `optimizer` of bound method `__init__`
- error[missing-argument] tests/ignite/engine/__init__.py:80:12: No arguments provided for required parameters `start`, `end` of bound method `__init__`
- error[missing-argument] tests/ignite/handlers/test_base_logger.py:36:16: No argument provided for required parameter `tag` of bound method `__init__`
- error[missing-argument] tests/ignite/handlers/test_base_logger.py:39:16: No argument provided for required parameter `optimizer` of bound method `__init__`
- error[missing-argument] tests/ignite/metrics/vision/test_object_detection_map.py:609:11: No arguments provided for required parameters `predictions`, `targets` of function `pycoco_mAP`
- Found 2130 diagnostics
+ Found 2111 diagnostics

ppb-vector (https://github.com/ppb/ppb-vector)
- error[no-matching-overload] tests/test_convert.py:41:17: No overload of function `__new__` matches arguments
- Found 19 diagnostics
+ Found 18 diagnostics

apprise (https://github.com/caronc/apprise)
+ error[parameter-already-assigned] apprise/plugins/telegram.py:402:13: Multiple values provided for parameter `fmt` of function `validate_regex`
- Found 4315 diagnostics
+ Found 4316 diagnostics

stone (https://github.com/dropbox/stone)
- error[missing-argument] stone/frontend/ir_generator.py:663:35: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:668:35: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:675:31: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:682:31: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1086:19: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1093:19: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1106:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1111:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1122:19: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1142:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1146:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1149:19: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1154:19: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1161:19: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1166:19: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1188:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1201:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1215:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1219:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1223:19: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1412:31: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1417:31: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1431:31: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1439:31: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1447:27: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1456:31: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1465:27: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1468:27: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1471:27: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1480:31: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1487:27: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1491:27: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1496:27: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1500:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1546:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/ir_generator.py:1566:23: No argument provided for required parameter `lineno` of bound method `__init__`
- error[missing-argument] stone/frontend/parser.py:592:16: No arguments provided for required parameters `arg_type_ref`, `result_type_ref` of bound method `__init__`
- Found 116 diagnostics
+ Found 79 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[no-matching-overload] tests/test_buffers.py:22:19: No overload of function `getattr` matches arguments
- error[no-matching-overload] tests/test_buffers.py:23:21: No overload of function `getattr` matches arguments
- Found 211 diagnostics
+ Found 209 diagnostics

flake8 (https://github.com/pycqa/flake8)
- error[missing-argument] tests/unit/test_statistics.py:58:12: No arguments provided for required parameters `prefix`, `filename` of bound method `matches`
- Found 42 diagnostics
+ Found 41 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ warning[unused-ignore-comment] aiohttp_devtools/logs.py:83:51: Unused blanket `type: ignore` directive
- Found 57 diagnostics
+ Found 58 diagnostics

optuna (https://github.com/optuna/optuna)
- error[missing-argument] tests/storages_tests/test_heartbeat.py:288:16: No arguments provided for required parameters `trial_id`, `state` of bound method `set_trial_state_values`
- Found 571 diagnostics
+ Found 570 diagnostics

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
- error[missing-argument] modnotes/modnotes.py:168:23: No arguments provided for required parameters `uid`, `author_id`, `subject_id`, `guild_id`, `note`, `created_at`
- error[missing-argument] modnotes/modnotes.py:183:23: No arguments provided for required parameters `uid`, `author_id`, `subject_id`, `guild_id`, `note`, `created_at`
- error[missing-argument] modnotes/modnotes.py:196:23: No arguments provided for required parameters `uid`, `author_id`, `subject_id`, `guild_id`, `note`, `created_at`
- error[missing-argument] modnotes/modnotes.py:209:23: No arguments provided for required parameters `uid`, `author_id`, `subject_id`, `guild_id`, `note`, `created_at`
- error[missing-argument] rss/core.py:363:25: No arguments provided for required parameters `year`, `month`, `day` of function `__new__`
- Found 149 diagnostics
+ Found 144 diagnostics

black (https://github.com/psf/black)
- error[missing-argument] src/black/cache.py:81:33: No arguments provided for required parameters `st_mtime`, `st_size`, `hash`
- error[missing-argument] src/black/files.py:79:9: No argument provided for required parameter `self` of function `intersection`
- error[missing-argument] src/blib2to3/pgen2/tokenize.py:219:9: No arguments provided for required parameters `type`, `token`, `srow_col`, `erow_col`, `line` of function `printtoken`
- Found 59 diagnostics
+ Found 56 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[no-matching-overload] cki_lib/misc.py:279:10: No overload of bound method `__init__` matches arguments
- error[missing-argument] tests/cki_pipeline/test_trigger_multiple.py:46:20: No arguments provided for required parameters `gl_instance`, `pipelines` of function `trigger_multiple`
+ error[parameter-already-assigned] tests/test_cronjob.py:66:25: Multiple values provided for parameter `tzinfo` of function `__new__`
- error[missing-argument] tests/test_psql.py:29:27: No arguments provided for required parameters `host`, `port`, `dbname` of bound method `__init__`
- Found 155 diagnostics
+ Found 153 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- error[missing-argument] src/schemathesis/config/_auth.py:35:13: No arguments provided for required parameters `username`, `password` of function `_validate_basic`
- error[missing-argument] src/schemathesis/pytest/plugin.py:141:71: No arguments provided for required parameters `username`, `password` of function `_basic_auth_str`
- error[missing-argument] src/schemathesis/specs/openapi/formats.py:87:16: No arguments provided for required parameters `username`, `password` of function `_basic_auth_str`
- Found 285 diagnostics
+ Found 282 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[missing-argument] cloudinit/distros/__init__.py:642:25: No argument provided for required parameter `canonical_hostname` of bound method `add_entry`
- error[missing-argument] cloudinit/sources/DataSourceGCE.py:159:13: No arguments provided for required parameters `key_type`, `key_value` of function `_write_host_key_to_guest_attributes`
- error[no-matching-overload] cloudinit/sources/helpers/openstack.py:364:16: No overload of function `join` matches arguments
- error[no-matching-overload] tests/unittests/config/test_apt_source_v1.py:113:20: No overload of function `join` matches arguments
- error[missing-argument] tests/unittests/config/test_cc_set_passwords.py:169:23: No argument provided for required parameter `iterable` of function `__new__`
- error[missing-argument] tests/unittests/config/test_cc_set_passwords.py:194:36: No argument provided for required parameter `iterable` of function `__new__`
- error[missing-argument] tests/unittests/distros/test_networking.py:206:36: No arguments provided for required parameters `mac`, `name` of function `ethernet`
- error[missing-argument] tests/unittests/helpers.py:227:20: No argument provided for required parameter `args` of function `subp`
- error[missing-argument] tests/unittests/net/test_init.py:1289:24: No arguments provided for required parameters `mac`, `name`, `driver`, `device_id` of function `_mk_v1_phys`
- error[missing-argument] tests/unittests/net/test_init.py:1306:36: No arguments provided for required parameters `mac`, `name` of function `_mk_v2_phys`
- error[missing-argument] tests/unittests/net/test_init.py:1323:24: No arguments provided for required parameters `mac`, `name`, `driver`, `device_id` of function `_mk_v1_phys`
- error[missing-argument] tests/unittests/net/test_init.py:1340:36: No arguments provided for required parameters `mac`, `name` of function `_mk_v2_phys`
- error[missing-argument] tests/unittests/net/test_init.py:1357:24: No arguments provided for required parameters `mac`, `name`, `driver`, `device_id` of function `_mk_v1_phys`
- error[missing-argument] tests/unittests/net/test_init.py:1371:36: No arguments provided for required parameters `mac`, `name` of function `_mk_v2_phys`
- error[missing-argument] tests/unittests/test_util.py:1755:13: No arguments provided for required parameters `outfmt`, `errfmt` of function `redirect_output`
- Found 610 diagnostics
+ Found 595 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- error[missing-argument] mkdocs/config/base.py:155:20: No argument provided for required parameter `schema` of bound method `__init__`
- error[no-matching-overload] mkdocs/tests/base.py:108:16: No overload of function `join` matches arguments
- error[no-matching-overload] mkdocs/tests/base.py:114:16: No overload of function `join` matches arguments
- error[no-matching-overload] mkdocs/tests/base.py:120:16: No overload of function `join` matches arguments
- error[no-matching-overload] mkdocs/tests/base.py:126:16: No overload of function `join` matches arguments
- Found 188 diagnostics
+ Found 183 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- error[missing-argument] dragonchain/contract_invoker/contract_invoker.py:55:15: No argument provided for required parameter `value` of function `rpush_async`
- Found 306 diagnostics
+ Found 305 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[no-matching-overload] tornado/process.py:238:25: No overload of bound method `__init__` matches arguments
- error[missing-argument] tornado/routing.py:355:28: No argument provided for required parameter `target` of bound method `__init__`
- error[missing-argument] tornado/routing.py:357:28: No arguments provided for required parameters `matcher`, `target` of bound method `__init__`
- error[no-matching-overload] tornado/web.py:1369:25: No overload of function `format_exception` matches arguments
- Found 246 diagnostics
+ Found 242 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[missing-argument] src/urllib3/filepost.py:48:19: No arguments provided for required parameters `fieldname`, `value` of bound method `from_tuples`
- error[no-matching-overload] src/urllib3/util/connection.py:100:9: No overload of bound method `setsockopt` matches arguments
- error[missing-argument] test/with_dummyserver/test_socketlevel.py:2442:19: No argument provided for required parameter `self` of function `connect`
- Found 400 diagnostics
+ Found 397 diagnostics

paasta (https://github.com/yelp/paasta)
- error[missing-argument] paasta_tools/cli/fsm_cmd.py:38:9: No arguments provided for required parameters `src`, `dst` of function `copyfile`
- error[missing-argument] paasta_tools/cli/fsm_cmd.py:42:9: No arguments provided for required parameters `src`, `dst` of function `copymode`
- error[missing-argument] paasta_tools/frameworks/constraints.py:48:30: No arguments provided for required parameters `_`, `attr_val`, `attr_name`, `state` of function `nested_inc`
- error[missing-argument] paasta_tools/frameworks/constraints.py:49:29: No arguments provided for required parameters `_`, `attr_val`, `attr_name`, `state` of function `nested_inc`
- error[missing-argument] paasta_tools/utils.py:1850:9: No arguments provided for required parameters `typ`, `value`, `traceback` of bound method `__exit__`
- error[no-matching-overload] paasta_tools/yaml_tools.py:23:12: No overload of function `dump` matches arguments
- error[no-matching-overload] paasta_tools/yaml_tools.py:28:12: No overload of function `dump_all` matches arguments
- error[missing-argument] paasta_tools/yaml_tools.py:33:12: No arguments provided for required parameters `stream`, `Loader` of function `load`
- error[missing-argument] paasta_tools/yaml_tools.py:38:12: No arguments provided for required parameters `stream`, `Loader` of function `load_all`
- Found 885 diagnostics
+ Found 876 diagnostics

colour (https://github.com/colour-science/colour)
- error[missing-argument] colour/appearance/zcam.py:115:20: No arguments provided for required parameters `F`, `c`, `N_c`
- error[missing-argument] colour/appearance/zcam.py:118:16: No arguments provided for required parameters `F`, `c`, `N_c`
- error[missing-argument] colour/appearance/zcam.py:119:17: No arguments provided for required parameters `F`, `c`, `N_c`
- error[missing-argument] colour/colorimetry/datasets/illuminants/hunterlab.py:87:19: No arguments provided for required parameters `name`, `XYZ_n`, `K_ab`
- error[missing-argument] colour/colorimetry/datasets/illuminants/hunterlab.py:116:19: No arguments provided for required parameters `name`, `XYZ_n`, `K_ab`
- error[missing-argument] colour/colorimetry/spectrum.py:1169:17: No arguments provided for required parameters `start`, `end`, `interval` of bound method `__init__`
- error[missing-argument] colour/graph/conversion.py:1090:5: No arguments provided for required parameters `source`, `target`, `conversion_function`
- error[no-matching-overload] colour/models/rgb/itut_h_273.py:216:25: No overload of function `clip` matches arguments
- error[missing-argument] colour/plotting/models.py:1986:13: No arguments provided for required parameters `a`, `b` of function `_linear_equation`
- error[missing-argument] colour/temperature/robertson1968.py:150:5: No arguments provided for required parameters `r`, `u`, `v`, `t`
- error[no-matching-overload] colour/utilities/verbose.py:325:5: No overload of function `warn` matches arguments
- error[missing-argument] colour/volume/rgb.py:76:12: No argument provided for required parameter `colourspace` of function `sample_RGB_colourspace_volume_MonteCarlo`
- Found 492 diagnostics
+ Found 480 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- error[missing-argument] Pythonwin/pywin/framework/winout.py:129:13: No argument provided for required parameter `flags` of bound method `AppendMenu`
- error[no-matching-overload] Pythonwin/pywin/test/test_pywin.py:32:10: No overload of function `open` matches arguments
- error[missing-argument] adodbapi/__init__.py:65:12: No arguments provided for required parameters `year`, `month`, `day` of function `Date`
- error[missing-argument] adodbapi/__init__.py:72:12: No arguments provided for required parameters `hour`, `minute`, `second` of function `Time`
- error[missing-argument] adodbapi/__init__.py:79:12: No arguments provided for required parameters `year`, `month`, `day`, `hour`, `minute`, `second` of function `Timestamp`
- error[no-matching-overload] adodbapi/apibase.py:548:46: No overload of function `__new__` matches arguments
- error[no-matching-overload] adodbapi/apibase.py:610:46: No overload of function `__new__` matches arguments
- error[missing-argument] com/win32com/client/gencache.py:766:31: No arguments provided for required parameters `clsid`, `lcid`, `major`, `minor` of function `GetGeneratedFileName`
- error[missing-argument] com/win32com/demos/ietoolbar.py:101:16: No argument provided for required parameter `fmt` of function `pack`
- error[missing-argument] com/win32com/server/register.py:372:9: No argument provided for required parameter `path` of function `recurse_delete_key`
- error[missing-argument] com/win32com/test/GenTestScripts.py:54:13: No argument provided for required parameter `fname` of function `GenerateFromRegistered`
- error[missing-argument] com/win32com/test/util.py:213:17: No argument provided for required parameter `testFunc` of bound method `__init__`
- error[missing-argument] com/win32comext/axscript/client/framework.py:56:16: No argument provided for required parameter `func` of bound method `runcall`
- error[missing-argument] win32/Demos/security/sspi/socket_server.py:63:9: No arguments provided for required parameters `server_address`, `RequestHandlerClass` of function `__init__`
- error[missing-argument] win32/Demos/win32gui_dialog.py:84:16: No argument provided for required parameter `fmt` of function `pack`
- error[missing-argument] win32/Demos/winprocess.py:122:27: No arguments provided for required parameters `appName`, `commandLine`, `processAttributes`, `threadAttributes`, `bInheritHandles`, `dwCreationFlags`, `newEnvironment`, `currentDirectory`, `startupinfo` of function `CreateProcessAsUser`
- error[missing-argument] win32/Demos/winprocess.py:125:27: No arguments provided for required parameters `appName`, `commandLine`, `processAttributes`, `threadAttributes`, `bInheritHandles`, `dwCreationFlags`, `newEnvironment`, `currentDirectory`, `startupinfo` of function `CreateProcess`
+ error[invalid-argument-type] win32/Demos/winprocess.py:122:67: Argument to function `CreateProcessAsUser` is incorrect: Expected `str`, found `None`
+ error[invalid-argument-type] win32/Demos/winprocess.py:122:67: Argument to function `CreateProcessAsUser` is incorrect: Expected `PySECURITY_ATTRIBUTES`, found `None`
+ error[invalid-argument-type] win32/Demos/winprocess.py:122:67: Argument to function `CreateProcessAsUser` is incorrect: Expected `PySECURITY_ATTRIBUTES`, found `None`
+ error[invalid-argument-type] win32/Demos/winprocess.py:122:67: Argument to function `CreateProcessAsUser` is incorrect: Expected `str`, found `None`
- error[missing-argument] win32/Lib/win32gui_struct.py:409:25: No arguments provided for required parameters `hitem`, `state`, `stateMask`, `text`, `image`, `selimage`, `citems`, `param` of function `PackTVITEM`
- error[missing-argument] win32/Lib/win32pdhquery.py:370:9: No argument provided for required parameter `self` of function `__init__`
- error[missing-argument] win32/Lib/win32pdhquery.py:468:9: No argument provided for required parameter `self` o...*[Comment body truncated]*

---

_@dcreager reviewed on 2025-06-27 22:56_

---

_Marked ready for review by @dcreager on 2025-06-27 22:56_

---

_Review requested from @carljm by @dcreager on 2025-06-27 22:56_

---

_Review requested from @AlexWaygood by @dcreager on 2025-06-27 22:56_

---

_Review requested from @sharkdp by @dcreager on 2025-06-27 22:56_

---

_Renamed from "[ty] WIP: Splat variadic arguments into parameter list" to "[ty] Splat variadic arguments into parameter list" by @dcreager on 2025-06-27 23:00_

---

_Review requested from @dhruvmanila by @carljm on 2025-06-28 04:20_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/function.md`:72 on 2025-06-28 04:29_

These tests appear to do a thorough job of testing arity checking, but I don't see any tests with mixed argument types that verify we are actually lining up the arguments correctly?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/function.md`:245 on 2025-06-28 04:33_

The comment above suggests that there's a TODO in this area, but the test doesn't include any TODO comment, or clearly demonstrate any wrong result that need to be improved. Is it possible to write a test that demonstrates specifically where our current approach falls short and needs to be improved?

---

_@carljm approved on 2025-06-28 05:02_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/function.md`:237 on 2025-06-30 10:10_

Interesting. The spec does say to loop over from step 2 which is the type checking step.

I've opened https://github.com/astral-sh/ty/issues/735 to keep track of it.


---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1435 on 2025-07-01 04:47_

I think it'd be useful to add a test case for this new scenario where the step 5 needs to be performed for a variadic argument against some number of parameters. These would probably go in `call/overloads.md` file.

A few examples that I can think of:

```py
from typing import Any, overload

class A: ...
class B: ...

@overload
def f1(x: int) -> A: ...
@overload
def f1(x: Any, y: Any) -> A: ...

@overload
def f2(x: int) -> A: ...
@overload
def f2(x: Any, y: Any) -> B: ...

@overload
def f3(x: int) -> A: ...
@overload
def f3(x: Any, y: Any) -> A: ...
@overload
def f3(x: Any, y: Any, *, z: str) -> B: ...

@overload
def f4(x: int) -> A: ...
@overload
def f4(x: Any, y: Any) -> B: ...
@overload
def f4(x: Any, y: Any, *, z: str) -> B: ...

def _(arg: list[Any]):
    # Matches both overload and the return types are equivalent
    reveal_type(f1(*arg))  # revealed: A
    # Matches both overload but the return types aren't equivalent
    reveal_type(f2(*arg))  # revealed: Unknown
    # Filters out the final overload and the return types are equivalent
    reveal_type(f3(*arg))  # revealed: A
    # Filters out the final overload but the return types aren't equivalent
    reveal_type(f4(*arg))  # revealed: Unknown
```

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1394 on 2025-07-01 05:02_

Similar to my other comment, it'd be useful to have a test case for this new scenario as well where one of the parameter is not being considered for the filtering process.

We can use the [existing test case](https://github.com/astral-sh/ruff/blob/87f2ff9516dccddc385b4ed136700284ca3d1979/crates/ty_python_semantic/resources/mdtest/call/overloads.md?plain=1#L782-L805) as a base to create this new test case.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/function.md`:157 on 2025-07-01 05:31_

These examples are great! Can we have an example containing positional-only parameter? I think it should just work but it might be useful to have coverage for them.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1960 on 2025-07-01 05:35_

nit: At this point I think we're using this pattern in multiple places. Do you think it might be useful to use something like https://docs.rs/vec1/1.12.1/vec1/smallvec_v1/index.html ?

---

_@dhruvmanila approved on 2025-07-01 05:39_

This looks great!

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-01 07:23_

---

_Review requested from @MichaReiser by @dcreager on 2025-07-14 21:22_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1960 on 2025-07-15 15:08_

Instead of that other crate I pulled this out into a new `ArgumentParameters` type alias

---

_@dcreager reviewed on 2025-07-15 15:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/call/function.md`:245 on 2025-07-15 19:08_

Done

---

_@dcreager reviewed on 2025-07-15 19:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/call/function.md`:72 on 2025-07-15 20:08_

There were a few in a couple of the sections, but I've added more (including mixed _parameter_ types, not just mixed _argument_ types)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1435 on 2025-07-15 20:41_

Added this test

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1394 on 2025-07-15 20:42_

I think I added what you're suggesting; let me know if you had something else in mind.  (Note that the test currently fails, but I'd like to dig into that as a follow-on PR)

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/call/function.md`:157 on 2025-07-15 20:47_

Done

---

_@dcreager reviewed on 2025-07-15 20:49_

---

_Comment by @dcreager on 2025-07-15 20:50_

I think I've covered all of the requested new test cases. @dhruvmanila, can you spot-check what I've added for overloads?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:540 on 2025-07-16 13:53_

I think this TODO is still remaining to resolve

---

_@dhruvmanila approved on 2025-07-16 13:55_

The tests looks great, thank you for adding them.

---

_@dhruvmanila reviewed on 2025-07-16 14:05_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1394 on 2025-07-16 14:05_

You can try looking at what the `top_materialized_argument_types` is which I think for the test case should be the top materialized types for `Any` and `Literal[True]` so, `vec![object, Literal[True]]` which basically gets merged back into `tuple[object, Literal[True]]`.

---

_Merged by @dcreager on 2025-07-22 18:33_

---

_Closed by @dcreager on 2025-07-22 18:33_

---

_Branch deleted on 2025-07-22 18:33_

---
