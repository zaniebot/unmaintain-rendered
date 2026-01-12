```yaml
number: 21654
title: "[ty] Always infer the submodule literal for submodule attribute-acces…"
type: pull_request
state: closed
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: alex/submodule-attr-last
head: gankra/submodule-attr-fallback-ty
created_at: 2025-11-27T03:17:50Z
updated_at: 2025-11-28T14:28:54Z
url: https://github.com/astral-sh/ruff/pull/21654
synced_at: 2026-01-12T15:57:30Z
```

# [ty] Always infer the submodule literal for submodule attribute-acces…

---

_@Gankra_

This is #21619 but based on top the #21583 to see if this looks better on top of it.

---

_Label `ty` added by @Gankra on 2025-11-27 03:17_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-27 03:17_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 03:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 03:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pip (https://github.com/pypa/pip)
+ src/pip/_internal/utils/logging.py:299:37: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `_FilterConfigurationTypedDict` constructor
+ src/pip/_internal/utils/logging.py:300:21: error[invalid-key] Unknown key "()" for TypedDict `_FilterConfigurationTypedDict`: Unknown key "()"
+ src/pip/_internal/utils/logging.py:301:21: error[invalid-key] Unknown key "level" for TypedDict `_FilterConfigurationTypedDict`: Unknown key "level"
+ src/pip/_internal/utils/logging.py:304:21: error[invalid-key] Unknown key "()" for TypedDict `_FilterConfigurationTypedDict`: Unknown key "()"
+ src/pip/_internal/utils/logging.py:308:21: error[invalid-key] Unknown key "()" for TypedDict `_FilterConfigurationTypedDict`: Unknown key "()"
- Found 603 diagnostics
+ Found 608 diagnostics

asynq (https://github.com/quora/asynq)
+ asynq/tests/test_mock.py:242:28: warning[possibly-missing-attribute] Submodule `test_mock` may not be available as an attribute on module `asynq.tests`
+ asynq/tests/test_mock.py:244:23: warning[possibly-missing-attribute] Submodule `test_mock` may not be available as an attribute on module `asynq.tests`
+ asynq/tests/test_mock.py:246:28: warning[possibly-missing-attribute] Submodule `test_mock` may not be available as an attribute on module `asynq.tests`
- Found 176 diagnostics
+ Found 179 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown | str, Unknown | str] & dict[str, Any]`
+ tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & dict[Unknown | str, Unknown | str]`

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
- Found 195 diagnostics
+ Found 204 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ pandera/api/pandas/components.py:23:9: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `str | type | DataType | ExtensionDtype | dtype[Any]`
+ pandera/api/pandas/components.py:365:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | type | DataType | ExtensionDtype | dtype[Any]`, found `Unknown | DataType | None`
+ pandera/api/pandas/types.py:17:5: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
- pandera/backends/pandas/checks.py:59:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/backends/pandas/hypotheses.py:139:40: error[invalid-argument-type] Argument to function `_format_groupby_input` is incorrect: Expected `SeriesGroupBy[Unknown, Unknown] | DataFrameGroupBy[Unknown, Unknown]`, found `list[tuple[Unknown, Unknown] | Unknown]`
+ pandera/engines/type_aliases.py:9:23: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
+ pandera/engines/type_aliases.py:10:24: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
+ pandera/typing/pandas.py:106:9: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
- tests/pandas/test_model.py:178:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/pandas/test_model.py:193:12: error[invalid-argument-type] Argument to class `Series` is incorrect: Expected `int | str | float | ... omitted 10 union elements`, found `list[int]`
+ tests/pandas/test_multithreaded.py:11:11: error[invalid-argument-type] Argument to class `Series` is incorrect: Expected `int | str | float | ... omitted 10 union elements`, found `floating[_32Bit]`
+ tests/pandas/test_pandas_accessor.py:63:9: warning[possibly-missing-attribute] Submodule `pandas` may not be available as an attribute on module `pandera.backends`
+ tests/pandas/test_pandas_accessor.py:63:9: warning[possibly-missing-attribute] Submodule `container` may not be available as an attribute on module `pandera.backends.pandas`
- tests/pandas/test_schema_components.py:663:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/pandas/test_typing.py:320:19: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
+ tests/pandas/test_typing.py:482:12: warning[possibly-missing-attribute] Submodule `base` may not be available as an attribute on module `pandas.core.dtypes`
- Found 1625 diagnostics
+ Found 1635 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ cki_lib/s3bucket.py:76:22: error[invalid-assignment] Object of type `ParseResult` is not assignable to `str`
+ cki_lib/s3bucket.py:77:23: error[unresolved-attribute] Object of type `str` has no attribute `scheme`
+ cki_lib/s3bucket.py:77:45: error[unresolved-attribute] Object of type `str` has no attribute `netloc`
+ cki_lib/s3bucket.py:78:31: error[unresolved-attribute] Object of type `str` has no attribute `path`
- Found 225 diagnostics
+ Found 229 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tests/test_gevent.py:61:36: warning[possibly-missing-attribute] Member `wait_c` may be missing on module `psycopg.waiting`
+ tests/test_gevent.py:76:36: warning[possibly-missing-attribute] Member `wait_c` may be missing on module `psycopg.waiting`
- Found 657 diagnostics
+ Found 659 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ examples/contrib/check_ssl_pinning.py:57:29: warning[deprecated] The class `X509Extension` is deprecated: X509Extension support in pyOpenSSL is deprecated. You should use the APIs in cryptography.
+ examples/contrib/check_ssl_pinning.py:61:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Certificate`, found `X509`
+ mitmproxy/tools/web/app.py:331:23: warning[possibly-missing-attribute] Submodule `view` may not be available as an attribute on module `mitmproxy.addons`
- Found 1862 diagnostics
+ Found 1865 diagnostics

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
- tests/test_utils_pem.py:229:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:261:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:290:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:300:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:302:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:304:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:313:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:315:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:317:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:325:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:329:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:334:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:347:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:349:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:364:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:367:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:376:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:388:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
- tests/test_utils_pem.py:395:13: warning[possibly-missing-attribute] Submodule `pem` may not be available as an attribute on module `apprise.utils`
+ tests/test_utils_pem.py:515:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `None`
- Found 2755 diagnostics
+ Found 2763 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/sources/DataSourceScaleway.py:47:9: error[invalid-method-override] Invalid override of method `init_poolmanager`: Definition is incompatible with `HTTPAdapter.init_poolmanager`
- Found 1208 diagnostics
+ Found 1209 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/worksearch/schemes/works.py:662:42: warning[possibly-missing-attribute] Submodule `client` may not be available as an attribute on module `infogami.infobase`
- Found 1115 diagnostics
+ Found 1116 diagnostics

xarray (https://github.com/pydata/xarray)
+ asv_bench/benchmarks/dataset_io.py:654:21: error[invalid-argument-type] Argument to function `explicit_indexing_adapter` is incorrect: Expected `ExplicitIndexer`, found `tuple[Unknown, ...]`
+ asv_bench/benchmarks/dataset_io.py:704:58: error[invalid-argument-type] Argument is incorrect: Expected `SerializableLock`, found `SerializableLock | None`
- Found 1832 diagnostics
+ Found 1834 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/heap/mallocng.py:1165:9: error[invalid-assignment] Object of type `int | None` is not assignable to attribute `ctx_addr` of type `int`
+ pwndbg/aglib/heap/mallocng.py:1184:34: error[invalid-argument-type] Argument to function `search` is incorrect: Expected `bytes`, found `bytearray`
+ pwndbg/aglib/kernel/dmabuf.py:106:24: warning[possibly-missing-attribute] Submodule `cymbol` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/aglib/kernel/dmabuf.py:107:5: warning[possibly-missing-attribute] Submodule `cymbol` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/aglib/kernel/kmod.py:27:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:47:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:90:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:117:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:152:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/aglib/kernel/kmod.py:166:2: warning[possibly-missing-attribute] Submodule `cache` may not be available as an attribute on module `pwndbg.lib`
- pwndbg/aglib/kernel/kmod.py:203:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `None | Unknown` and `int`
+ pwndbg/aglib/kernel/kmod.py:203:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `None | int` and `int`
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
+ pwndbg/commands/__init__.py:863:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Value | None | Unknown`
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
+ pwndbg/commands/flags.py:39:20: error[unresolved-attribute] Object of type `None` has no attribute `current`
+ pwndbg/commands/flags.py:58:31: error[unresolved-attribute] Object of type `None` has no attribute `read_reg`
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
+ pwndbg/dbg/lldb/repl/__init__.py:497:9: warning[possibly-missing-attribute] Submodule `version` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/dbg/lldb/repl/__init__.py:674:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/dbg/lldb/repl/__init__.py:703:20: warning[possibly-missing-attribute] Submodule `start` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/dbg/lldb/repl/__init__.py:704:23: warning[possibly-missing-attribute] Submodule `start` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/gdblib/events.py:49:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/ghidra.py:25:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/lib/pretty_print.py:18:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/search.py:15:26: warning[possibly-missing-attribute] Submodule `memory` may not be available as an attribute on module `pwndbg.lib`
+ pwndbg/ui.py:27:17: warning[possibly-missing-attribute] Submodule `config` may not be available as an attribute on module `pwndbg.lib`
- Found 2713 diagnostics
+ Found 2827 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/server/api/run_history.py:159:20: error[invalid-await] `Result[tuple[Unknown, Unknown, Unknown]]` is not awaitable
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:35:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:40:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:45:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:51:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:57:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:63:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:69:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:81:30: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:84:27: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:88:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:93:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:98:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:103:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:110:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:115:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:121:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:127:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:160:30: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:171:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:177:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:183:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:216:30: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:220:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:224:33: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:227:32: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:231:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:236:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:241:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:247:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_04_03_112409_aeea5ee6f070_automations_models.py:253:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/postgresql/2024_09_11_090317_555ed31b284d_add_concurrency_options.py:27:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:34:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:39:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:44:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:50:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:56:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:64:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:70:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:84:30: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:87:27: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:91:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:96:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:101:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:106:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:113:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:118:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:126:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:132:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:162:30: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:173:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:181:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:187:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:219:30: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module `prefect.server.utilities`
+ src/prefect/server/database/_migrations/versions/sqlite/2024_04_03_111618_07ed05dfd4ec_automations_models.py:223:13: warning[possibly-missing-attribute] Submodule `database` may not be available as an attribute on module

... (truncated 93 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 03:26_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-missing-attribute` | 179 | 25 | 0 |
| `invalid-argument-type` | 68 | 0 | 3 |
| `unresolved-attribute` | 17 | 0 | 0 |
| `invalid-assignment` | 4 | 0 | 0 |
| `invalid-key` | 4 | 0 | 0 |
| `unsupported-operator` | 3 | 0 | 1 |
| `invalid-await` | 3 | 0 | 0 |
| `unused-ignore-comment` | 0 | 3 | 0 |
| `no-matching-overload` | 2 | 0 | 0 |
| `deprecated` | 1 | 0 | 0 |
| `invalid-method-override` | 1 | 0 | 0 |
| `invalid-parameter-default` | 1 | 0 | 0 |
| `missing-typed-dict-key` | 1 | 0 | 0 |
| `unsupported-base` | 0 | 1 | 0 |
| **Total** | **284** | **29** | **4** |

**[Full report with detailed diff](https://gankra-submodule-attr-fallba.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-submodule-attr-fallba.ecosystem-663.pages.dev/timing))




---

_Comment by @Gankra on 2025-11-27 03:44_

Looks like it's nearly entirely orthogonal. The wins on mitmproxy go away, I think because the other PR already finds those wins.

---

_Closed by @Gankra on 2025-11-28 14:28_

---
