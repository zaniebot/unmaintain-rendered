```yaml
number: 18348
title: "[ty] Type inference for `hasattr(obj, attr)`"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/hasattr-inference
created_at: 2025-05-28T07:59:04Z
updated_at: 2025-05-28T08:12:26Z
url: https://github.com/astral-sh/ruff/pull/18348
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Type inference for `hasattr(obj, attr)`

---

_Pull request opened by @sharkdp on 2025-05-28 07:59_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-05-28 07:59_

---

_Renamed from "[ty] Type inference for hasattr(…)" to "[ty] Type inference for `hasattr(…)`" by @sharkdp on 2025-05-28 07:59_

---

_Renamed from "[ty] Type inference for `hasattr(…)`" to "[ty] Type inference for `hasattr(obj, attr)`" by @sharkdp on 2025-05-28 07:59_

---

_Comment by @github-actions[bot] on 2025-05-28 08:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- warning[possibly-unresolved-reference] src/anyio/pytest_plugin.py:139:25: Name `backend_name` used when possibly not defined
+ error[unresolved-reference] src/anyio/pytest_plugin.py:139:25: Name `backend_name` used when not defined
- warning[possibly-unresolved-reference] src/anyio/pytest_plugin.py:139:39: Name `backend_options` used when possibly not defined
+ error[unresolved-reference] src/anyio/pytest_plugin.py:139:39: Name `backend_options` used when not defined

trio (https://github.com/python-trio/trio)
+ warning[unused-ignore-comment] src/trio/_tests/test_util.py:267:55: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/test_util.py:268:66: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/test_util.py:269:69: Unused blanket `type: ignore` directive
- error[unresolved-attribute] src/trio/_tests/test_util.py:271:5: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:272:5: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:273:5: Type `ModuleType` has no attribute `_private`
- error[unresolved-attribute] src/trio/_tests/test_util.py:274:5: Type `ModuleType` has no attribute `SomeClass`
- Found 1092 diagnostics
+ Found 1091 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-return-type] pydantic/types.py:1017:35: Function can implicitly return `None`, which is not assignable to return type `str`
- warning[possibly-unresolved-reference] pydantic/v1/dataclasses.py:312:17: Name `post_init` used when possibly not defined
- warning[possibly-unresolved-reference] pydantic/v1/dataclasses.py:320:17: Name `post_init` used when possibly not defined
+ warning[unused-ignore-comment] pydantic/v1/dataclasses.py:341:70: Unused blanket `type: ignore` directive
- Found 762 diagnostics
+ Found 760 diagnostics

black (https://github.com/psf/black)
- warning[possibly-unresolved-reference] src/blib2to3/pytree.py:852:34: Name `save_stderr` used when possibly not defined
- Found 133 diagnostics
+ Found 132 diagnostics

stone (https://github.com/dropbox/stone)
- error[invalid-argument-type] test/test_backend.py:272:30: Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None`
- warning[possibly-unbound-implicit-call] test/test_backend.py:273:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
- Found 173 diagnostics
+ Found 171 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- warning[possibly-unbound-attribute] tornado/test/httpserver_test.py:826:32: Attribute `bind_unix_socket` on type `<module 'tornado.netutil'>` is possibly unbound
- Found 351 diagnostics
+ Found 350 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- warning[possibly-unresolved-reference] test/test_ssltransport.py:25:5: Name `server_context` used when possibly not defined
- warning[possibly-unresolved-reference] test/test_ssltransport.py:30:5: Name `client_context` used when possibly not defined
- warning[possibly-unresolved-reference] test/test_ssltransport.py:31:12: Name `server_context` used when possibly not defined
- warning[possibly-unresolved-reference] test/test_ssltransport.py:31:28: Name `client_context` used when possibly not defined
- Found 449 diagnostics
+ Found 445 diagnostics

asynq (https://github.com/quora/asynq)
- error[unresolved-attribute] asynq/decorators.py:92:9: Unresolved attribute `is_pure_async_fn` on type `def sync_to_async_fn_wrapper(*args, **kwargs) -> Unknown`.
- Found 210 diagnostics
+ Found 209 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:1133:34: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `(~None & ~<Protocol with members '__iter__'> & ~type) | (Unknown & ~type)`
- Found 639 diagnostics
+ Found 638 diagnostics

vision (https://github.com/pytorch/vision)
- warning[possibly-unbound-attribute] torchvision/io/video.py:31:23: Attribute `FFmpegError` on type `Unknown | ImportError` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:33:23: Attribute `AVError` on type `Unknown | ImportError` is possibly unbound
- Found 1840 diagnostics
+ Found 1838 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- warning[possibly-unresolved-reference] schema_salad/schema.py:387:56: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] schema_salad/schema.py:394:56: Name `name` used when possibly not defined
- Found 250 diagnostics
+ Found 248 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- warning[unsupported-base] src/werkzeug/serving.py:879:25: Unsupported class base with type `<class 'ForkingMixIn'> | <class 'ForkingMixIn'>`
- Found 452 diagnostics
+ Found 451 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- warning[possibly-unbound-attribute] src/_pytest/runner.py:265:19: Attribute `value` on type `ExceptionInfo[BaseException] | None` is possibly unbound
- Found 883 diagnostics
+ Found 882 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[possibly-unresolved-reference] static_frame/core/frame.py:934:34: Name `fields_dc` used when possibly not defined
- warning[possibly-unresolved-reference] static_frame/core/frame.py:943:29: Name `fields_dc` used when possibly not defined
+ warning[unused-ignore-comment] static_frame/core/frame.py:954:80: Unused blanket `type: ignore` directive
- Found 2067 diagnostics
+ Found 2066 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- warning[possibly-unresolved-reference] scrapy/contracts/__init__.py:52:29: Name `cb` used when possibly not defined
- warning[possibly-unresolved-reference] scrapy/contracts/__init__.py:68:29: Name `cb` used when possibly not defined
- warning[possibly-unresolved-reference] scrapy/core/engine.py:410:44: Name `d` used when possibly not defined
- Found 1304 diagnostics
+ Found 1301 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- warning[possibly-unresolved-reference] src/bokeh/core/serialization.py:470:41: Name `arr` used when possibly not defined
- error[unresolved-attribute] src/bokeh/embed/bundle.py:301:24: Type `<module 'json'>` has no attribute `decoder`
- warning[possibly-unbound-attribute] src/bokeh/server/server.py:422:24: Attribute `bind_unix_socket` on type `<module 'tornado.netutil'>` is possibly unbound
- warning[possibly-unresolved-reference] src/bokeh/util/compiler.py:233:28: Name `file` used when possibly not defined
- Found 953 diagnostics
+ Found 949 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- warning[possibly-unbound-attribute] lib/streamlit/web/server/server.py:194:19: Attribute `bind_unix_socket` on type `<module 'tornado.netutil'>` is possibly unbound
- Found 3269 diagnostics
+ Found 3268 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- warning[possibly-unresolved-reference] lib/Crypto/Math/_IntegerGMP.py:139:16: Name `byref` used when possibly not defined
- warning[possibly-unresolved-reference] lib/Crypto/Math/_IntegerGMP.py:139:22: Name `_MPZ` used when possibly not defined
- error[unresolved-import] lib/Crypto/Math/_IntegerGMP.py:145:10: Cannot resolve imported module `Crypto.Util._raw_api`
- warning[possibly-unresolved-reference] lib/Crypto/Math/_IntegerGMP.py:148:16: Name `ffi` used when possibly not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_dss.py:179:16: Name `hash_module` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_dss.py:181:47: Name `generator` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_dss.py:181:58: Name `modulus` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_dss.py:181:67: Name `suborder` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_dss.py:217:16: Name `hash_module` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_dss.py:218:51: Name `generator` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_dss.py:218:62: Name `modulus` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_dss.py:218:71: Name `suborder` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_pkcs1_15.py:141:50: Name `signer` used when not defined
+ warning[unused-ignore-comment] lib/Crypto/SelfTest/Signature/test_pkcs1_15.py:87:77: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] lib/Crypto/SelfTest/Signature/test_pkcs1_15.py:134:88: Unused blanket `type: ignore` directive
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_pss.py:193:26: Name `private_key` used when not defined
- error[unresolved-reference] lib/Crypto/SelfTest/Signature/test_pss.py:195:26: Name `private_key` used when not defined
+ warning[unused-ignore-comment] lib/Crypto/SelfTest/Signature/test_pss.py:135:78: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] lib/Crypto/SelfTest/Signature/test_pss.py:186:89: Unused blanket `type: ignore` directive
- Found 1544 diagnostics
+ Found 1533 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- warning[possibly-unresolved-reference] ddtrace/contrib/internal/openai/utils.py:338:60: Name `prompt_tokens` used when possibly not defined
- warning[possibly-unresolved-reference] ddtrace/contrib/internal/openai/utils.py:340:64: Name `completion_tokens` used when possibly not defined
- warning[possibly-unbound-attribute] ddtrace/vendor/psutil/__init__.py:2218:15: Attribute `net_io_counters` on type `<module 'ddtrace.vendor.psutil._pslinux'> | <module 'ddtrace.vendor.psutil._pswindows'> | <module 'ddtrace.vendor.psutil._psosx'> | <module 'ddtrace.vendor.psutil._psbsd'> | <module 'ddtrace.vendor.psutil._pssunos'> | <module 'ddtrace.vendor.psutil._psaix'>` is possibly unbound
+ error[unresolved-attribute] ddtrace/vendor/psutil/_psbsd.py:248:9: Unresolved attribute `__called__` on type `def per_cpu_times() -> Unknown`.
- error[unresolved-attribute] ddtrace/vendor/psutil/_psbsd.py:246:12: Type `(def per_cpu_times() -> Unknown) | (def per_cpu_times() -> Unknown)` has no attribute `__called__`
- error[invalid-assignment] ddtrace/vendor/psutil/_psbsd.py:248:9: Object of type `Literal[True]` is not assignable to attribute `__called__` on type `(def per_cpu_times() -> Unknown) | (def per_cpu_times() -> Unknown)`
- warning[possibly-unresolved-reference] ddtrace/vendor/psutil/_pswindows.py:365:16: Name `_loadavg_inititialized` used when possibly not defined
- error[unresolved-import] tests/appsec/iast_packages/inside_env_runner.py:12:10: Cannot resolve imported module `astunparse`
- Found 6847 diagnostics
+ Found 6841 diagnostics

zulip (https://github.com/zulip/zulip)
- warning[possibly-unresolved-reference] zerver/lib/test_helpers.py:507:43: Name `re_strip` used when possibly not defined
- warning[possibly-unresolved-reference] zerver/lib/test_helpers.py:509:25: Name `calls` used when possibly not defined
- warning[possibly-unresolved-reference] zerver/lib/test_helpers.py:513:23: Name `cleanup_url` used when possibly not defined
- warning[possibly-unresolved-reference] zerver/lib/test_helpers.py:522:13: Name `pattern_cnt` used when possibly not defined
- error[unresolved-attribute] zerver/tornado/handlers.py:162:37: Type `<module 'django.http'>` has no attribute `cookie`
- Found 6987 diagnostics
+ Found 6982 diagnostics

sympy (https://github.com/sympy/sympy)
- warning[possibly-unresolved-reference] sympy/combinatorics/testutil.py:196:47: Name `subgr_gens` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/core/multidimensional.py:29:16: Name `is_arg` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/matrices/matrixbase.py:3875:21: Name `evaluate` used when possibly not defined
+ error[unresolved-reference] sympy/matrices/matrixbase.py:3875:21: Name `evaluate` used when not defined
- warning[possibly-unresolved-reference] sympy/matrices/matrixbase.py:3876:56: Name `ismat` used when possibly not defined
+ error[unresolved-reference] sympy/matrices/matrixbase.py:3876:56: Name `ismat` used when not defined
- warning[possibly-unresolved-reference] sympy/matrices/matrixbase.py:3894:37: Name `make_explicit` used when possibly not defined
+ error[unresolved-reference] sympy/matrices/matrixbase.py:3894:37: Name `make_explicit` used when not defined
- warning[possibly-unresolved-reference] sympy/matrices/matrixbase.py:3896:36: Name `make_explicit` used when possibly not defined
+ error[unresolved-reference] sympy/matrices/matrixbase.py:3896:36: Name `make_explicit` used when not defined
- error[unsupported-operator] sympy/polys/polyfuncs.py:84:20: Operator `+` is unsupported between objects of type `list[Unknown] | Unknown` and `tuple[@Todo(list comprehension type)]`
+ error[unsupported-operator] sympy/polys/polyfuncs.py:84:20: Operator `+` is unsupported between objects of type `list[Unknown]` and `tuple[@Todo(list comprehension type)]`
- warning[possibly-unresolved-reference] sympy/utilities/tests/test_pickling.py:99:104: Name `protocol` used when possibly not defined
- Found 18569 diagnostics
+ Found 18566 diagnostics

scipy (https://github.com/scipy/scipy)
- error[unresolved-import] scipy/_lib/array_api_compat/array_api_compat/common/_helpers.py:621:28: Cannot resolve imported module `jax.experimental.array_api`
- error[unresolved-import] scipy/_lib/array_api_compat/array_api_compat/common/_helpers.py:883:20: Cannot resolve imported module `jax.experimental.array_api`
- error[unresolved-import] scipy/_lib/array_api_compat/tests/test_array_namespace.py:36:24: Cannot resolve imported module `jax.experimental.array_api`
- warning[possibly-unresolved-reference] scipy/io/_netcdf.py:582:34: Name `nc_type` used when possibly not defined
- warning[possibly-unresolved-reference] scipy/io/_netcdf.py:582:34: Name `nc_type` used when possibly not defined
- warning[possibly-unresolved-reference] scipy/io/_netcdf.py:582:34: Name `nc_type` used when possibly not defined
- warning[possibly-unresolved-reference] scipy/io/_netcdf.py:589:23: Name `nc_type` used when possibly not defined
- error[invalid-argument-type] scipy/io/wavfile.py:907:19: Argument to bound method `write` is incorrect: Expected `str`, found `bytes`
- error[invalid-argument-type] scipy/io/wavfile.py:910:19: Argument to bound method `write` is incorrect: Expected `str`, found `Literal[b"data"]`
- error[invalid-argument-type] scipy/io/wavfile.py:912:19: Argument to bound method `write` is incorrect: Expected `str`, found `bytes`
- error[invalid-argument-type] scipy/io/wavfile.py:924:23: Argument to bound method `write` is incorrect: Expected `str`, found `bytes`
- error[invalid-argument-type] scipy/io/wavfile.py:927:23: Argument to bound method `write` is incorrect: Expected `str`, found `bytes`
- warning[possibly-unresolved-reference] scipy/spatial/tests/test_qhull.py:1012:28: Name `vertex_map` used when possibly not defined
- warning[possibly-unresolved-reference] scipy/spatial/tests/test_qhull.py:1015:38: Name `objx` used when possibly not defined
- warning[possibly-unbound-attribute] scipy/stats/tests/test_distributions.py:9176:22: Attribute `numargs` on type `_distr_gen` is possibly unbound
+ error[unresolved-attribute] scipy/stats/tests/test_distributions.py:9176:22: Type `_distr_gen` has no attribute `numargs`
- warning[possibly-unbound-attribute] scipy/stats/tests/test_distributions.py:9186:22: Attribute `numargs` on type `_distr6_gen` is possibly unbound
+ error[unresolved-attribute] scipy/stats/tests/test_distributions.py:9186:22: Type `_distr6_gen` has no attribute `numargs`
- Found 7763 diagnostics
+ Found 7749 diagnostics

```
</details>


---

_Comment by @sharkdp on 2025-05-28 08:12_

Seeing the ecosystem impact, this seems like a bad idea. Attributes are always available on dynamic types, so this can easily lead to problems:
```py
def f(x):
    if hasattr(x, "foo"):
        # always true!
    else:
        # unreachable!
```

---

_Closed by @sharkdp on 2025-05-28 08:12_

---
