```yaml
number: 21619
title: "[ty] Always infer the submodule literal for submodule attribute-access, even when we emit a diagnostic"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/submodule-attr-fallback-ty
created_at: 2025-11-24T19:34:42Z
updated_at: 2025-11-27T03:25:52Z
url: https://github.com/astral-sh/ruff/pull/21619
synced_at: 2026-01-12T15:57:29Z
```

# [ty] Always infer the submodule literal for submodule attribute-access, even when we emit a diagnostic

---

_@AlexWaygood_

## Summary

Closes https://github.com/astral-sh/ty/issues/1623

## Test Plan

mdtests updated


---

_Label `ty` added by @AlexWaygood on 2025-11-24 19:34_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-24 19:34_

---

_Label `ty` added by @AlexWaygood on 2025-11-24 19:34_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-24 19:34_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 19:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@AlexWaygood reviewed on 2025-11-24 19:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:438 on 2025-11-24 19:37_

the fact that we now emit two diagnostics on this line (rather than one previously) makes me pretty nervous about this. Let's see what the ecosystem says, but something like `<module 'mypackage.submodule'> & Any` might be better for the inferred type of `mypackage.submodule`; then (in theory) we wouldn't emit the second diagnostic

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 19:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
asynq (https://github.com/quora/asynq)
+ asynq/tests/test_mock.py:242:28: warning[possibly-missing-attribute] Submodule `test_mock` may not be available as an attribute on module `asynq.tests`
+ asynq/tests/test_mock.py:244:23: warning[possibly-missing-attribute] Submodule `test_mock` may not be available as an attribute on module `asynq.tests`
+ asynq/tests/test_mock.py:246:28: warning[possibly-missing-attribute] Submodule `test_mock` may not be available as an attribute on module `asynq.tests`
- Found 176 diagnostics
+ Found 179 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/utils/logging.py:299:37: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `_FilterConfigurationTypedDict` constructor
+ src/pip/_internal/utils/logging.py:300:21: error[invalid-key] Unknown key "()" for TypedDict `_FilterConfigurationTypedDict`: Unknown key "()"
+ src/pip/_internal/utils/logging.py:301:21: error[invalid-key] Unknown key "level" for TypedDict `_FilterConfigurationTypedDict`: Unknown key "level"
+ src/pip/_internal/utils/logging.py:304:21: error[invalid-key] Unknown key "()" for TypedDict `_FilterConfigurationTypedDict`: Unknown key "()"
+ src/pip/_internal/utils/logging.py:308:21: error[invalid-key] Unknown key "()" for TypedDict `_FilterConfigurationTypedDict`: Unknown key "()"
- Found 603 diagnostics
+ Found 608 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_feedexport.py:1348:29: warning[possibly-missing-attribute] Submodule `feedexport` may not be available as an attribute on module `scrapy.extensions`
+ tests/test_feedexport.py:1352:29: warning[possibly-missing-attribute] Submodule `feedexport` may not be available as an attribute on module `scrapy.extensions`
- Found 1727 diagnostics
+ Found 1729 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ test/unit/test_knn.py:75:46: error[invalid-argument-type] Argument is incorrect: Expected `DataStatistics`, found `None`
- Found 413 diagnostics
+ Found 414 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ tests/Framework.py:396:17: error[invalid-argument-type] Argument to bound method `injectConnectionClasses` is incorrect: Expected `type[HTTPRequestsConnectionClass]`, found `<class 'RecordingHttpConnection'>`
+ tests/Framework.py:397:17: error[invalid-argument-type] Argument to bound method `injectConnectionClasses` is incorrect: Expected `type[HTTPSRequestsConnectionClass]`, found `<class 'RecordingHttpsConnection'>`
+ tests/Framework.py:413:17: error[invalid-argument-type] Argument to bound method `injectConnectionClasses` is incorrect: Expected `type[HTTPRequestsConnectionClass]`, found `<class 'ReplayingHttpConnection'>`
+ tests/Framework.py:414:17: error[invalid-argument-type] Argument to bound method `injectConnectionClasses` is incorrect: Expected `type[HTTPSRequestsConnectionClass]`, found `<class 'ReplayingHttpsConnection'>`
+ tests/GithubObject.py:206:66: error[invalid-argument-type] Argument to function `_makeHttpDatetimeAttribute` is incorrect: Expected `str | None`, found `Unknown | str | int`
+ tests/GithubObject.py:212:62: error[invalid-argument-type] Argument to function `_makeDatetimeAttribute` is incorrect: Expected `str | None`, found `Unknown | str | int`
+ tests/GithubObject.py:225:59: error[invalid-argument-type] Argument to function `_makeTimestampAttribute` is incorrect: Expected `int`, found `None`
+ tests/GithubObject.py:235:63: error[invalid-argument-type] Argument to function `_makeTimestampAttribute` is incorrect: Expected `int`, found `Unknown | str | float`
+ tests/Logging_.py:85:49: error[invalid-argument-type] Argument to bound method `injectLogger` is incorrect: Expected `Logger`, found `MockLogger`
- Found 288 diagnostics
+ Found 297 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ examples/contrib/check_ssl_pinning.py:57:29: warning[deprecated] The class `X509Extension` is deprecated: X509Extension support in pyOpenSSL is deprecated. You should use the APIs in cryptography.
+ examples/contrib/check_ssl_pinning.py:61:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Certificate`, found `X509`
+ examples/contrib/check_ssl_pinning.py:93:9: error[invalid-assignment] Implicit shadowing of function `dummy_cert`
+ mitmproxy/tools/web/app.py:331:23: warning[possibly-missing-attribute] Submodule `view` may not be available as an attribute on module `mitmproxy.addons`
- mitmproxy/tools/web/app.py:585:29: error[unresolved-attribute] Unresolved attribute `port` on type `object`.
- mitmproxy/tools/web/app.py:587:29: error[unresolved-attribute] Object of type `object` has no attribute `headers`
+ mitmproxy/tools/web/app.py:580:55: error[invalid-assignment] Object of type `object` is not assignable to `Request`
- mitmproxy/tools/web/app.py:589:33: error[unresolved-attribute] Object of type `object` has no attribute `headers`
- mitmproxy/tools/web/app.py:591:32: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:592:33: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:594:33: error[unresolved-attribute] Unresolved attribute `trailers` on type `object`.
+ mitmproxy/tools/web/app.py:596:33: warning[possibly-missing-attribute] Attribute `add` may be missing on object of type `Headers | None`
- mitmproxy/tools/web/app.py:596:33: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:598:29: error[unresolved-attribute] Unresolved attribute `text` on type `object`.
- mitmproxy/tools/web/app.py:608:29: error[unresolved-attribute] Unresolved attribute `status_code` on type `object`.
- mitmproxy/tools/web/app.py:610:29: error[unresolved-attribute] Object of type `object` has no attribute `headers`
+ mitmproxy/tools/web/app.py:603:57: error[invalid-assignment] Object of type `object` is not assignable to `Response`
- mitmproxy/tools/web/app.py:612:33: error[unresolved-attribute] Object of type `object` has no attribute `headers`
- mitmproxy/tools/web/app.py:614:32: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:615:33: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:617:33: error[unresolved-attribute] Unresolved attribute `trailers` on type `object`.
+ mitmproxy/tools/web/app.py:619:33: warning[possibly-missing-attribute] Attribute `add` may be missing on object of type `Headers | None`
- mitmproxy/tools/web/app.py:619:33: error[unresolved-attribute] Object of type `object` has no attribute `trailers`
- mitmproxy/tools/web/app.py:621:29: error[unresolved-attribute] Unresolved attribute `text` on type `object`.
+ test/mitmproxy/test_flow.py:135:25: warning[possibly-missing-attribute] Submodule `tutils` may not be available as an attribute on module `mitmproxy.test`
+ test/mitmproxy/test_flow.py:139:26: warning[possibly-missing-attribute] Submodule `tutils` may not be available as an attribute on module `mitmproxy.test`
- Found 1878 diagnostics
+ Found 1872 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ cki_lib/s3bucket.py:76:22: error[invalid-assignment] Object of type `ParseResult` is not assignable to `str`
+ cki_lib/s3bucket.py:77:23: error[unresolved-attribute] Object of type `str` has no attribute `scheme`
+ cki_lib/s3bucket.py:77:45: error[unresolved-attribute] Object of type `str` has no attribute `netloc`
+ cki_lib/s3bucket.py:78:31: error[unresolved-attribute] Object of type `str` has no attribute `path`
- Found 214 diagnostics
+ Found 218 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ pandera/api/pandas/components.py:23:9: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `str | type | DataType | ExtensionDtype | dtype[Any]`
+ pandera/api/pandas/components.py:365:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | type | DataType | ExtensionDtype | dtype[Any]`, found `Unknown | DataType | None`
+ pandera/api/pandas/types.py:18:5: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
- pandera/backends/pandas/checks.py:59:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/backends/pandas/hypotheses.py:138:40: error[invalid-argument-type] Argument to function `_format_groupby_input` is incorrect: Expected `SeriesGroupBy[Unknown, Unknown] | DataFrameGroupBy[Unknown, Unknown]`, found `list[tuple[Unknown, Unknown] | Unknown]`
+ pandera/engines/type_aliases.py:10:23: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
+ pandera/engines/type_aliases.py:11:24: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
+ pandera/typing/pandas.py:106:9: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
+ tests/dask/test_dask.py:111:27: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing.dask`
+ tests/dask/test_dask.py:114:28: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing.dask`
+ tests/modin/test_schemas_on_modin.py:317:20: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing.modin`
+ tests/modin/test_schemas_on_modin.py:318:22: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing.modin`
+ tests/modin/test_schemas_on_modin.py:319:20: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing.modin`
+ tests/modin/test_schemas_on_modin.py:387:12: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing.modin`
+ tests/modin/test_schemas_on_modin.py:390:12: warning[possibly-missing-attribute] Member `Series` may be missing on module `pandera.typing.modin`
+ tests/modin/test_schemas_on_modin.py:416:13: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing.modin`
+ tests/modin/test_schemas_on_modin.py:417:10: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing.modin`
+ tests/modin/test_schemas_on_modin.py:423:13: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing.modin`
+ tests/modin/test_schemas_on_modin.py:424:10: warning[possibly-missing-attribute] Member `DataFrame` may be missing on module `pandera.typing.modin`
- tests/pandas/test_model.py:178:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/pandas/test_model.py:193:21: error[invalid-argument-type] Argument to class `Series` is incorrect: Expected `int | str | float | ... omitted 10 union elements`, found `list[int]`
+ tests/pandas/test_multithreaded.py:11:11: error[invalid-argument-type] Argument to class `Series` is incorrect: Expected `int | str | float | ... omitted 10 union elements`, found `floating[_32Bit]`
+ tests/pandas/test_pandas_accessor.py:63:9: warning[possibly-missing-attribute] Submodule `pandas` may not be available as an attribute on module `pandera.backends`
+ tests/pandas/test_pandas_accessor.py:63:9: warning[possibly-missing-attribute] Submodule `container` may not be available as an attribute on module `pandera.backends.pandas`
- tests/pandas/test_schema_components.py:664:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/pandas/test_typing.py:320:19: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
+ tests/pandas/test_typing.py:482:12: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
- Found 1635 diagnostics
+ Found 1656 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ schema_salad/sourceline.py:17:30: error[invalid-argument-type] Argument to function `_add_lc_filename` is incorrect: Expected `CommentedBase`, found `object`
+ schema_salad/sourceline.py:20:30: error[invalid-argument-type] Argument to function `_add_lc_filename` is incorrect: Expected `CommentedBase`, found `object`
- Found 214 diagnostics
+ Found 216 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tests/test_gevent.py:61:36: warning[possibly-missing-attribute] Member `wait_c` may be missing on module `psycopg.waiting`
+ tests/test_gevent.py:76:36: warning[possibly-missing-attribute] Member `wait_c` may be missing on module `psycopg.waiting`
- Found 663 diagnostics
+ Found 665 diagnostics

xarray (https://github.com/pydata/xarray)
+ asv_bench/benchmarks/dataset_io.py:654:21: error[invalid-argument-type] Argument to function `explicit_indexing_adapter` is incorrect: Expected `ExplicitIndexer`, found `tuple[Unknown, ...]`
+ asv_bench/benchmarks/dataset_io.py:704:58: error[invalid-argument-type] Argument is incorrect: Expected `SerializableLock`, found `SerializableLock | None`
- Found 1830 diagnostics
+ Found 1832 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/sources/DataSourceScaleway.py:47:9: error[invalid-method-override] Invalid override of method `init_poolmanager`: Definition is incompatible with `HTTPAdapter.init_poolmanager`
+ tests/unittests/cmd/test_clean.py:544:17: warning[possibly-missing-attribute] Submodule `clean` may not be available as an attribute on module `cloudinit.cmd`
- Found 1204 diagnostics
+ Found 1206 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/util/docutils.py:211:9: warning[possibly-missing-attribute] Submodule `rst` may not be available as an attribute on module `docutils.parsers`
+ sphinx/util/docutils.py:211:9: warning[possibly-missing-attribute] Submodule `languages` may not be available as an attribute on module `docutils.parsers.rst`
- Found 573 diagnostics
+ Found 575 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/heap/mallocng.py:1165:9: error[invalid-assignment] Object of type `int | None` is not assignable to attribute `ctx_addr` of type `int`
+ pwndbg/aglib/heap/mallocng.py:1184:34: error[invalid-argument-type] Argument to function `search` is incorrect: Expected `bytes`, found `bytearray`
+ pwndbg/aglib/kernel/dmabuf.py:106:24: warning[possibly-missing-attribute] Submodule `cymbol` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/aglib/kernel/dmabuf.py:107:5: warning[possibly-missing-attribute] Submodule `cymbol` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/aglib/kernel/kmod.py:27:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:29:15: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:33:14: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:37:18: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:47:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:49:15: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:53:14: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:57:23: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:66:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:69:24: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:74:31: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:78:33: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:80:24: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:90:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:92:15: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:96:14: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:100:12: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:102:16: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:107:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:117:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:119:15: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:123:14: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:127:12: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:129:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:130:12: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:132:18: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:133:12: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:135:22: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:136:12: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:138:18: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:139:12: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:141:12: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:142:23: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:145:16: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:152:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:154:15: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:159:12: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:166:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:168:15: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:174:11: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:177:15: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:186:14: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:187:18: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:188:14: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:190:8: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:191:19: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:194:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:198:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:202:12: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:203:28: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
- pwndbg/aglib/kernel/kmod.py:203:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `None | Unknown` and `int`
+ pwndbg/aglib/kernel/kmod.py:203:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `None | int` and `int`
+ pwndbg/aglib/kernel/kmod.py:206:17: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:214:8: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/kmod.py:221:24: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/kernel/slab.py:365:6: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/symbol.py:251:24: warning[possibly-missing-attribute] Submodule `cymbol` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/aglib/kernel/symbol.py:252:5: warning[possibly-missing-attribute] Submodule `cymbol` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/aglib/kernel/symbol.py:302:71: error[invalid-argument-type] Argument to function `get_typed_pointer` is incorrect: Expected `int | Value`, found `Value | None | Unknown`
+ pwndbg/aglib/kernel/symbol.py:310:80: error[invalid-argument-type] Argument to function `get_typed_pointer_value` is incorrect: Expected `int | Value`, found `Value | None | Unknown`
+ pwndbg/aglib/kernel/symbol.py:318:71: error[invalid-argument-type] Argument to function `get_typed_pointer` is incorrect: Expected `int | Value`, found `None | Unknown`
+ pwndbg/aglib/kernel/symbol.py:326:71: error[invalid-argument-type] Argument to function `get_typed_pointer` is incorrect: Expected `int | Value`, found `(Value & ~AlwaysTruthy) | None | Unknown`
+ pwndbg/aglib/kernel/symbol.py:339:74: error[invalid-argument-type] Argument to function `get_typed_pointer` is incorrect: Expected `int | Value`, found `(Value & ~AlwaysTruthy) | None | Unknown`
+ pwndbg/aglib/kernel/symbol.py:347:71: error[invalid-argument-type] Argument to function `get_typed_pointer` is incorrect: Expected `int | Value`, found `(Value & ~AlwaysTruthy) | None | Unknown`
+ pwndbg/aglib/kernel/symbol.py:355:71: error[invalid-argument-type] Argument to function `get_typed_pointer` is incorrect: Expected `int | Value`, found `(Value & ~AlwaysTruthy) | None | Unknown`
+ pwndbg/aglib/kernel/symbol.py:370:71: error[invalid-argument-type] Argument to function `get_typed_pointer` is incorrect: Expected `int | Value`, found `(Value & ~AlwaysTruthy) | None | Unknown | int`
+ pwndbg/aglib/macho.py:644:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/nearpc.py:261:24: warning[possibly-missing-attribute] Attribute `is_initialized` may be missing on object of type `MemoryAllocator | None`
+ pwndbg/aglib/nearpc.py:267:24: warning[possibly-missing-attribute] Submodule `ptmalloc` may not be available as an attribute on module `pwndbg.aglib.heap`
+ pwndbg/aglib/nearpc.py:287:21: error[unresolved-attribute] Object of type `MemoryAllocator | None` has no attribute `fastbins`
+ pwndbg/aglib/nearpc.py:288:21: error[unresolved-attribute] Object of type `MemoryAllocator | None` has no attribute `smallbins`
+ pwndbg/aglib/nearpc.py:289:21: error[unresolved-attribute] Object of type `MemoryAllocator | None` has no attribute `largebins`
+ pwndbg/aglib/nearpc.py:290:21: error[unresolved-attribute] Object of type `MemoryAllocator | None` has no attribute `unsortedbin`
+ pwndbg/aglib/nearpc.py:292:20: error[unresolved-attribute] Object of type `MemoryAllocator | None` has no attribute `has_tcache`
+ pwndbg/aglib/nearpc.py:293:38: error[unresolved-attribute] Object of type `MemoryAllocator | None` has no attribute `tcachebins`
+ pwndbg/aglib/nearpc.py:344:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[str, str]].__getitem__(key: str, /) -> dict[str, str]` cannot be called with key of type `str | None` on object of type `dict[str, dict[str, str]]`
+ pwndbg/aglib/objc.py:1045:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/objc.py:1052:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/objc.py:1056:64: warning[possibly-missing-attribute] Submodule `functions` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/objc.py:1132:23: warning[possibly-missing-attribute] Submodule `functions` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/objc.py:1148:28: warning[possibly-missing-attribute] Submodule `functions` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/objc.py:1150:16: warning[possibly-missing-attribute] Submodule `functions` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/shellcode.py:33:20: warning[possibly-missing-attribute] Submodule `regs` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/shellcode.py:95:20: warning[possibly-missing-attribute] Submodule `regs` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/typeinfo.py:115:12: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/typeinfo.py:116:12: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/typeinfo.py:117:12: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/typeinfo.py:118:12: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/color/memory.py:32:14: warning[possibly-missing-attribute] Submodule `symbol` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/color/memory.py:36:16: warning[possibly-missing-attribute] Submodule `vmmap` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/color/memory.py:55:14: warning[possibly-missing-attribute] Submodule `symbol` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/color/memory.py:59:16: warning[possibly-missing-attribute] Submodule `vmmap` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/color/memory.py:83:12: warning[possibly-missing-attribute] Submodule `vmmap` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/color/memory.py:109:16: warning[possibly-missing-attribute] Submodule `pretty_print` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/__init__.py:856:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Value | None | Unknown`
+ pwndbg/commands/attachp.py:30:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/comments.py:23:20: error[unresolved-attribute] Object of type `None` has no attribute `pc`
+ pwndbg/commands/comments.py:28:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/comments.py:32:33: warning[possibly-missing-attribute] Submodule `proc` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/comments.py:34:20: warning[possibly-missing-attribute] Submodule `proc` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/comments.py:35:21: error[invalid-assignment] Invalid subscript assignment with key of type `None | str` and value of type `dict[Unknown, Unknown]` on object of type `dict[str, dict[str, str]]`
+ pwndbg/commands/comments.py:35:32: warning[possibly-missing-attribute] Submodule `proc` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/comments.py:36:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[str, str]].__getitem__(key: str, /) -> dict[str, str]` cannot be called with key of type `str | None` on object of type `dict[str, dict[str, str]]`
+ pwndbg/commands/comments.py:36:28: warning[possibly-missing-attribute] Submodule `proc` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/context.py:103:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/context.py:644:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/context.py:1090:9: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/context.py:1132:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/context.py:1415:13: warning[possibly-missing-attribute] Submodule `tips` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/context.py:1499:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/cymbol.py:298:26: error[unresolved-attribute] Object of type `CachedFunction[Unknown]` has no attribute `__wrapped__`
+ pwndbg/commands/cymbol.py:298:26: warning[possibly-missing-attribute] Submodule `context` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/commands/flags.py:39:20: error[unresolved-attribute] Object of type `None` has no attribute `current`
+ pwndbg/commands/flags.py:58:31: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ pwndbg/commands/kconfig.py:30:19: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/kconfig.py:30:19: warning[possibly-missing-attribute] Submodule `kconfig` may not be available as an attribute on module `pwndbg.lib.kernel`
+ pwndbg/commands/kcurrent.py:83:20: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kcurrent.py:84:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:34:16: warning[possibly-missing-attribute] Submodule `symbol` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:45:34: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:52:26: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:53:37: error[invalid-argument-type] Argument to function `get_typed_pointer_value` is incorrect: Expected `int | Value`, found `Value | None`
+ pwndbg/commands/kdmesg.py:63:31: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:64:37: error[invalid-argument-type] Argument to function `get_typed_pointer_value` is incorrect: Expected `int | Value`, found `Value | None`
+ pwndbg/commands/kdmesg.py:69:21: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:80:25: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:80:25: warning[possibly-missing-attribute] Attribute `sizeof` may be missing on object of type `Type | None`
+ pwndbg/commands/kdmesg.py:81:28: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:81:28: warning[possibly-missing-attribute] Attribute `sizeof` may be missing on object of type `Type | None`
+ pwndbg/commands/kdmesg.py:86:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:101:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:118:29: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:123:32: warning[possibly-missing-attribute] Submodule `symbol` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:133:20: warning[possibly-missing-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/kdmesg.py:141:27: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/ksyscalls.py:25:18: warning[possibly-missing-attribute] Submodule `symbol` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/ksyscalls.py:55:23: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/ksyscalls.py:57:22: warning[possibly-missing-attribute] Submodule `symbol` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/ktask.py:30:20: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/ktask.py:31:68: warning[possibly-missing-attribute] Submodule `kernel` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/ktask.py:90:17: warning[possibly-missing-attribute] Submodule `symbol` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/mallocng.py:35:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/mallocng.py:36:11: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/mallocng.py:948:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/mallocng.py:949:11: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/paging.py:96:17: warning[possibly-missing-attribute] Submodule `kcurrent` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/commands/ptmalloc2.py:886:65: error[invalid-argument-type] Argument to function `round_up` is incorrect: Expected `int`, found `object`
+ pwndbg/commands/ptmalloc2.py:887:9: error[unsupported-operator] Operator `|=` is unsupported between objects of type `int` and `object`
+ pwndbg/commands/ptmalloc2.py:921:34: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | int` and `object`
+ pwndbg/commands/ptmalloc2.py:924:34: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | int` and `object`
+ pwndbg/commands/spray.py:39:16: warning[possibly-missing-attribute] Submodule `vmmap` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/spray.py:67:19: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/spray.py:72:58: error[invalid-argument-type] Argument to bound method `unpack` is incorrect: Expected `bytes`, found `bytearray`
+ pwndbg/commands/spray.py:73:24: warning[possibly-missing-attribute] Submodule `vmmap` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/spray.py:75:21: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/spray.py:79:13: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/valist.py:29:17: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/valist.py:32:25: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/valist.py:33:21: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/commands/vmmap.py:291:17: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/vmmap.py:377:12: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/commands/vmmap.py:466:20: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/dbg/gdb/__init__.py:1673:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `context_shown` of type `Literal[False]`
+ pwndbg/dbg/lldb/repl/__init__.py:497:9: warning[possibly-missing-attribute] Submodule `version` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/dbg/lldb/repl/__init__.py:671:26: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/dbg/lldb/repl/__init__.py:674:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/dbg/lldb/repl/__init__.py:703:20: warning[possibly-missing-attribute] Submodule `start` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/dbg/lldb/repl/__init__.py:704:23: warning[possibly-missing-attribute] Submodule `start` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/gdblib/events.py:49:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/ghidra.py:25:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/lib/pretty_print.py:18:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/search.py:15:26: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/ui.py:27:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
- Found 2816 diagnostics
+ Found 2991 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_distutils/tests/test_build_ext.py:82:16: error[no-matching-overload] No overload of function `basename` matches arguments
+ setuptools/_distutils/tests/test_build_ext.py:82:33: warning[possibly-missing-attribute] Attribute `origin` may be missing on object of type `ModuleSpec | None`
+ setuptools/_distutils/tests/test_build_ext.py:85:5: error[no-matching-overload] No overload of function `copy` matches arguments
+ setuptools/_distutils/tests/test_build_ext.py:85:17: warning[possibly-missing-attribute] Attribute `origin` may be missing on object of type `ModuleSpec | None`
+ setuptools/_distutils/tests/test_version.py:11:10: error[unresolved-attribute] Module `distutils.version` has no member `suppress_known_deprecation`
+ setuptools/logging.py:29:25: error[unresolved-attribute] Module `distutils.dist` has no member `log`
+ setuptools/logging.py:35:9: error[unresolved-attribute] Unresolved attribute `log` on type `<module 'distutils.dist'>`.
+ setuptools/monkey.py:82:9: error[invalid-assignment] Object of type `<class 'Distribution'>` is not assignable to attribute `Distribution` on type `<module 'distutils.dist'> | <module 'distutils.core'> | <module 'distutils.cmd'>`
- Found 1274 diagnostics
+ Found 1282 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/server/api/run_history.py:159:20: error[invalid-await] `Result[tuple[Unknown, Unknown, Unknown]]` is not awaitable
+ src/prefect/server/services/scheduler.py:217:36: error[invalid-await] `Connection` is not awaitable
+ src/prefect/server/services/scheduler.py:234:27: error[invalid-await] `None` is not awaitable
+ src/prefect/server/services/scheduler.py:279:13: error[invalid-argument-type] Argument to function `_generate_scheduled_flow_runs` is incorrect: Expected `AsyncSession`, found `Session`
- Found 3289 diagnostics
+ Found 3293 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/worksearch/schemes/works.py:662:42: warning[possibly-missing-attribute] Submodule `client` may not be available as an attribute on module `infogami.infobase`
- Found 1119 diagnostics
+ Found 1120 diagnostics

apprise (https://github.com/caronc/apprise)
+ apprise/plugins/fcm/oauth.py:250:13: error[invalid-argument-type] Argument to bound method `sign` is incorrect: Expected `Prehashed | HashAlgorithm`, found `PKCS1v15`
+ apprise/plugins/fcm/oauth.py:250:13: error[invalid-argument-type] Argument to bound method `sign` is incorrect: Expected `EllipticCurveSignatureAlgorithm`, found `PKCS1v15`
+ tests/test_apprise_utils.py:3088:42: error[invalid-argument-type] Argument to function `base64_urlencode` is incorrect: Expected `bytes`, found `None`
+ tests/test_apprise_utils.py:3089:42: error[invalid-argument-type] Argument to function `base64_urlencode` is incorrect: Expected `bytes`, found `Literal[42]`
+ tests/test_apprise_utils.py:3090:42: error[invalid-argument-type] Argument to function `base64_urlencode` is incorrect: Expected `bytes`, found `<class 'object'>`
+ tests/test_apprise_utils.py:3091:42: error[invalid-argument-type] Argument to function `base64_urlencode` is incorrect: Expected `bytes`, found `dict[Unknown, Unknown]`
+ tests/test_apprise_utils.py:3092:42: error[invalid-argument-type] Argument to function `base64_urlencode` is incorrect: Expected `bytes`, found `Literal[""]`
+ tests/test_apprise_utils.py:3093:42: error[invalid-argument-type] Argument to function `base64_urlencode` is incorrect: Expected `bytes`, found `Literal["abc"]`
+ tests/test_apprise_utils.py:3097:42: error[invalid-argument-type] Argument to function `base64_urldecode` is incorrect: Expected `str`, found `None`
+ tests/test_apprise_utils.py:3098:42: error[invalid-argument-type] Argument to function `base64_urldecode` is incorrect: Expected `str`, found `Literal[42]`
+ tests/test_apprise_utils.py:3099:42: error[invalid-argument-type] Argument to function `base64_urldecode` is incorrect: Expected `str`, found `<class 'object'>`
+ tests/test_apprise_utils.py:3100:42: error[invalid-argument-type] Argument to function `base64_urldecode` is incorrect: Expected `str`, found `dict[Unknown, Unknown]`
+ tests/test_apprise_utils.py:3290:12: warning[possibly-missing-attribute] Attribute `key` may be missing on object of type `ZoneInfo | None`
+ tests/test_apprise_utils.py:3290:60: warning[possibly-missing-attribute] Attribute `key` may be missing on object of type `ZoneInfo | None`
+ tests/test_apprise_utils.py:3298:16: warning[possibly-missing-attribute] Attribute `key` may be missing on object of type `ZoneInfo | None`
+ tests/test_apprise_utils.py:3303:16: warning[possibly-missing-attribute] Attribute `key` may be missing on object of type `ZoneInfo | None`
+ tests/test_apprise_utils.py:3310:32: error[invalid-argument-type] Argument to function `zoneinfo` is incorrect: Expected `str`, found `<class 'object'>`
+ tests/test_apprise_utils.py:3311:32: error[invalid-argument-type] Argument to function `zoneinfo` is incorrect: Expected `str`, found `None`
+ tests/test_apprise_utils.py:3312:32: error[invalid-argument-type] Argument to function `zoneinfo` is incorrect: Expected `str`, found `Literal[1]`
+ tests/test_utils_pem.py:65:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `None`
+ tests/test_utils_pem.py:116:9: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `str | None`
+ tests/test_utils_pem.py:117:24: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `str | None`
+ tests/test_utils_pem.py:119:9: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `str | None`
+ tests/test_utils_pem.py:120:24: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `str | None`
+ tests/test_utils_pem.py:121:26: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `str | None`
+ tests/test_utils_pem.py:132:26: error[invalid-argument-type] Argument to bound method `encrypt_webpush` is incorrect: Expected `EllipticCurvePublicKey`, found `EllipticCurvePublicKey | None`
+ tests/test_utils_pem.py:138:9: error[invalid-argument-type] Argument to bound method `encrypt_webpush` is incorrect: Expected `EllipticCurvePublicKey`, found `EllipticCurvePublicKey | None`
+ tests/test_utils_pem.py:145:30: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `None`
+ tests/test_utils_pem.py:148:30: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `Literal[5]`
+ tests/test_utils_pem.py:151:30: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `Literal[False]`
+ tests/test_utils_pem.py:154:30: error[invalid-argument-type] Argument to bound method `decrypt` is incorrect: Expected `str | bytes`, found `<class 'object'>`
+ tests/test_utils_pem.py:158:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `None`
- tests/test_utils_pem.py:168:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:179:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:190:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:202:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:210:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:222:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:229:13: warning[possibly-missing-attribut

... (truncated 81 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-24 19:43_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-missing-attribute` | 202 | 25 | 0 |
| `invalid-argument-type` | 71 | 0 | 1 |
| `unresolved-attribute` | 17 | 16 | 0 |
| `invalid-assignment` | 9 | 0 | 0 |
| `invalid-key` | 4 | 0 | 0 |
| `unsupported-operator` | 3 | 0 | 1 |
| `invalid-await` | 3 | 0 | 0 |
| `unused-ignore-comment` | 0 | 3 | 0 |
| `no-matching-overload` | 2 | 0 | 0 |
| `deprecated` | 1 | 0 | 0 |
| `invalid-method-override` | 1 | 0 | 0 |
| `invalid-parameter-default` | 1 | 0 | 0 |
| `missing-typed-dict-key` | 1 | 0 | 0 |
| `non-subscriptable` | 1 | 0 | 0 |
| **Total** | **316** | **44** | **2** |

**[Full report with detailed diff](https://alex-submodule-attr-fallback.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-submodule-attr-fallback.ecosystem-663.pages.dev/timing))




---

_Comment by @Gankra on 2025-11-26 20:14_

Taking over this PR for Alex.

---

_Assigned to @Gankra by @Gankra on 2025-11-26 20:14_

---

_Comment by @AlexWaygood on 2025-11-26 20:29_

Some thoughts: this branch means that you can get three separate `possibly-unresolved-attribute` errors from something like `x = package.sub.sub2.sub3`. If we intersected the submodule-literal type with `Any`, that might make things better? But also just landing https://github.com/astral-sh/ruff/pull/21583 might make the impact on this look better too, so probs best to do that first

---

_Comment by @Gankra on 2025-11-27 03:25_

Horribly aware of how much "what if x AND y works well" haunts everything so just spinning that up here in parallel: https://github.com/astral-sh/ruff/pull/21654

---
