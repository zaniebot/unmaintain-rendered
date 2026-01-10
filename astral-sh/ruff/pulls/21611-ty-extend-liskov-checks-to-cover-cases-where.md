```yaml
number: 21611
title: "[ty] Extend Liskov checks to cover cases where methods are incompatibly overridden by non-methods"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/liskov-method-overridden-by-non-method
created_at: 2025-11-24T11:47:34Z
updated_at: 2025-12-22T13:07:09Z
url: https://github.com/astral-sh/ruff/pull/21611
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Extend Liskov checks to cover cases where methods are incompatibly overridden by non-methods

---

_Pull request opened by @AlexWaygood on 2025-11-24 11:47_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ty/issues/2156

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-11-24 11:47_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-24 11:47_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 11:49_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-25 10:33:16.881497031 +0000
+++ new-output.txt	2025-11-25 10:33:20.774498275 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/17bc55d/src/function/execute.rs:469:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1a6a3)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/17bc55d/src/function/execute.rs:469:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1baa3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-24 11:51_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
asynq (https://github.com/quora/asynq)
- asynq/tests/test_tools.py:183:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 176 diagnostics
+ Found 175 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/headers.py:108:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/werkzeug/datastructures/headers.py:625:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
- src/werkzeug/datastructures/structures.py:648:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/routing/rules.py:912:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 399 diagnostics
+ Found 397 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/commands/download.py:75:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ src/pip/_internal/commands/install.py:279:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ src/pip/_internal/commands/lock.py:89:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ src/pip/_internal/commands/wheel.py:100:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
- src/pip/_internal/utils/misc.py:557:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 603 diagnostics
+ Found 606 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/vendor/pyrsistent/_checked_types.py:303:5: error[invalid-method-override] Invalid override of method `create`: Definition is incompatible with `CheckedType.create`
+ lib/spack/spack/vendor/pyrsistent/_checked_types.py:394:5: error[invalid-method-override] Invalid override of method `create`: Definition is incompatible with `CheckedType.create`
- Found 7958 diagnostics
+ Found 7960 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_code/code.py:76:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/_code/source.py:50:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/python_api.py:88:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/_pytest/python_api.py:486:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
- testing/io/test_saferepr.py:48:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/io/test_saferepr.py:49:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 457 diagnostics
+ Found 453 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/metadata/metacheck.py:94:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_check/metadata/metadecor.py:305:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 505 diagnostics
+ Found 503 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_item.py:112:13: error[invalid-method-override] Invalid override of method `keys`: Definition is incompatible with `Item.keys`
+ tests/test_item.py:113:13: error[invalid-method-override] Invalid override of method `values`: Definition is incompatible with `Mapping.values`
+ tests/test_item.py:136:13: error[invalid-method-override] Invalid override of method `keys`: Definition is incompatible with `Item.keys`
+ tests/test_item.py:137:13: error[invalid-method-override] Invalid override of method `values`: Definition is incompatible with `Mapping.values`
+ tests/test_item.py:140:13: error[invalid-method-override] Invalid override of method `keys`: Definition is incompatible with `Item.keys`
- Found 1739 diagnostics
+ Found 1744 diagnostics

pybind11 (https://github.com/pybind/pybind11)
+ tests/conftest.py:81:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ tests/conftest.py:100:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ tests/conftest.py:124:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ tests/conftest.py:165:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ tests/test_pytypes.py:210:9: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ tests/test_pytypes.py:582:9: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
- Found 211 diagnostics
+ Found 217 diagnostics

nox (https://github.com/wntrblm/nox)
- nox/_parametrize.py:77:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 29 diagnostics
+ Found 28 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/test/auth_test.py:78:9: error[invalid-method-override] Invalid override of method `_oauth_get_user_future`: Definition is incompatible with `OAuthMixin._oauth_get_user_future`
- Found 323 diagnostics
+ Found 324 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- mypy-stubs/rdflib/namespace/__init__.pyi:36:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 228 diagnostics
+ Found 227 diagnostics

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/_collections_abc.pyi:75:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_collections_abc.pyi:93:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_contextvars.pyi:37:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_contextvars.pyi:60:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_frozen_importlib.pyi:47:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_interpchannels.pyi:49:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_ssl.pyi:125:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_tkinter.pyi:25:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_weakrefset.pyi:21:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/argparse.pyi:470:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/array.pyi:77:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/builtins.pyi:747:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/builtins.pyi:852:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/builtins.pyi:853:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/builtins.pyi:1034:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/builtins.pyi:1129:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/builtins.pyi:1182:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/collections/__init__.pyi:121:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/collections/__init__.pyi:256:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/decimal.pyi:209:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/email/charset.pyi:34:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/email/header.pyi:19:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/email/headerregistry.pyi:171:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/email/headerregistry.pyi:180:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/big5.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/big5.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/big5hkscs.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/big5hkscs.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/cp932.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/cp932.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/cp949.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/cp949.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/cp950.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/cp950.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/euc_jis_2004.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/euc_jis_2004.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/euc_jisx0213.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/euc_jisx0213.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/euc_jp.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/euc_jp.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/euc_kr.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/euc_kr.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/gb18030.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/gb18030.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/gb2312.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/gb2312.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/gbk.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/gbk.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/hz.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/hz.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_1.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_1.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_2.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_2.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_2004.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_2004.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_3.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_3.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_ext.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_jp_ext.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_kr.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/iso2022_kr.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/johab.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/johab.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/shift_jis.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/shift_jis.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/shift_jis_2004.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/shift_jis_2004.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/shift_jisx0213.pyi:8:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/encodings/shift_jisx0213.pyi:9:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/inspect.pyi:463:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/inspect.pyi:571:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/inspect.pyi:579:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/inspect.pyi:645:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/inspect.pyi:653:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/lib2to3/pgen2/pgen.pyi:49:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/lib2to3/pytree.pyi:27:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/numbers.pyi:109:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/optparse.pyi:237:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/parser.pyi:20:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/sched.pyi:29:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/select.pyi:63:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/tkinter/__init__.pyi:332:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/tkinter/font.pyi:62:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/traceback.pyi:233:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/traceback.pyi:314:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/types.pyi:313:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/types.pyi:337:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/types.pyi:354:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/types.pyi:391:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/unittest/mock.pyi:89:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/unittest/mock.pyi:120:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/unittest/mock.pyi:515:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/unittest/suite.pyi:21:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/weakref.pyi:47:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/weakref.pyi:53:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:247:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/xmlrpc/client.pyi:80:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/xmlrpc/client.pyi:100:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/xxlimited.pyi:22:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1779 diagnostics
+ Found 1677 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/regex.py:112:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 466 diagnostics
+ Found 465 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
+ src/zope/interface/declarations.py:241:5: error[invalid-method-override] Invalid override of method `changed`: Definition is incompatible with `Specification.changed`
+ src/zope/interface/declarations.py:241:15: error[invalid-method-override] Invalid override of method `subscribe`: Definition is incompatible with `Specification.subscribe`
+ src/zope/interface/declarations.py:241:27: error[invalid-method-override] Invalid override of method `unsubscribe`: Definition is incompatible with `Specification.unsubscribe`
+ src/zope/interface/tests/test_interface.py:352:13: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ src/zope/interface/tests/test_interface.py:357:13: error[invalid-method-override] Invalid override of method `__ne__`: Definition is incompatible with `object.__ne__`
- Found 421 diagnostics
+ Found 426 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/aiohttp/views.py:91:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/asgi/__init__.py:103:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/channels/handlers/ws_handler.py:103:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/fastapi/router.py:63:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/litestar/controller.py:216:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/quart/views.py:82:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 399 diagnostics
+ Found 393 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/_typed_ops.py:93:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/core/_typed_ops.py:362:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/core/_typed_ops.py:736:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/core/_typed_ops.py:1164:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/core/_typed_ops.py:1376:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/core/_typed_ops.py:1502:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/core/dataarray.py:1365:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/core/dataset.py:1507:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1833 diagnostics
+ Found 1825 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/linear_model/_coordinate_descent.py:2153:5: error[invalid-method-override] Invalid override of method `path`: Definition is incompatible with `LinearModelCV.path`
+ sklearn/linear_model/_coordinate_descent.py:2444:5: error[invalid-method-override] Invalid override of method `path`: Definition is incompatible with `LinearModelCV.path`
+ sklearn/linear_model/_coordinate_descent.py:3119:5: error[invalid-method-override] Invalid override of method `path`: Definition is incompatible with `LinearModelCV.path`
+ sklearn/linear_model/_coordinate_descent.py:3371:5: error[invalid-method-override] Invalid override of method `path`: Definition is incompatible with `LinearModelCV.path`
- Found 2321 diagnostics
+ Found 2325 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- mypy-stubs/rdflib/namespace/__init__.pyi:36:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 201 diagnostics
+ Found 200 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-aws/prefect_aws/s3.py:987:9: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `WritableDeploymentStorage.get_directory`
+ src/integrations/prefect-aws/prefect_aws/s3.py:1078:9: error[invalid-method-override] Invalid override of method `put_directory`: Definition is incompatible with `WritableDeploymentStorage.put_directory`
+ src/integrations/prefect-aws/prefect_aws/s3.py:1178:9: error[invalid-method-override] Invalid override of method `read_path`: Definition is incompatible with `WritableFileSystem.read_path`
+ src/integrations/prefect-aws/prefect_aws/s3.py:1260:9: error[invalid-method-override] Invalid override of method `write_path`: Definition is incompatible with `WritableFileSystem.write_path`
+ src/integrations/prefect-aws/prefect_aws/s3.py:1494:9: error[invalid-method-override] Invalid override of method `download_object_to_path`: Definition is incompatible with `ObjectStorageBlock.download_object_to_path`
+ src/integrations/prefect-aws/prefect_aws/s3.py:1610:9: error[invalid-method-override] Invalid override of method `download_object_to_file_object`: Definition is incompatible with `ObjectStorageBlock.download_object_to_file_object`
+ src/integrations/prefect-aws/prefect_aws/s3.py:1739:9: error[invalid-method-override] Invalid override of method `download_folder_to_path`: Definition is incompatible with `ObjectStorageBlock.download_folder_to_path`
+ src/integrations/prefect-aws/prefect_aws/s3.py:1981:9: error[invalid-method-override] Invalid override of method `upload_from_path`: Definition is incompatible with `ObjectStorageBlock.upload_from_path`
+ src/integrations/prefect-aws/prefect_aws/s3.py:2081:9: error[invalid-method-override] Invalid override of method `upload_from_file_object`: Definition is incompatible with `ObjectStorageBlock.upload_from_file_object`
+ src/integrations/prefect-aws/prefect_aws/s3.py:2205:9: error[invalid-method-override] Invalid override of method `upload_from_folder`: Definition is incompatible with `ObjectStorageBlock.upload_from_folder`
+ src/integrations/prefect-aws/prefect_aws/secrets_manager.py:419:9: error[invalid-method-override] Invalid override of method `read_secret`: Definition is incompatible with `SecretBlock.read_secret`
+ src/integrations/prefect-aws/prefect_aws/secrets_manager.py:505:9: error[invalid-method-override] Invalid override of method `write_secret`: Definition is incompatible with `SecretBlock.write_secret`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:238:15: error[invalid-method-override] Invalid override of method `download_folder_to_path`: Definition is incompatible with `ObjectStorageBlock.download_folder_to_path`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:313:15: error[invalid-method-override] Invalid override of method `download_object_to_file_object`: Definition is incompatible with `ObjectStorageBlock.download_object_to_file_object`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:372:15: error[invalid-method-override] Invalid override of method `download_object_to_path`: Definition is incompatible with `ObjectStorageBlock.download_object_to_path`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:438:15: error[invalid-method-override] Invalid override of method `upload_from_file_object`: Definition is incompatible with `ObjectStorageBlock.upload_from_file_object`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:495:15: error[invalid-method-override] Invalid override of method `upload_from_path`: Definition is incompatible with `ObjectStorageBlock.upload_from_path`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:552:15: error[invalid-method-override] Invalid override of method `upload_from_folder`: Definition is incompatible with `ObjectStorageBlock.upload_from_folder`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:624:15: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `WritableDeploymentStorage.get_directory`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:639:15: error[invalid-method-override] Invalid override of method `put_directory`: Definition is incompatible with `WritableDeploymentStorage.put_directory`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:688:15: error[invalid-method-override] Invalid override of method `read_path`: Definition is incompatible with `WritableFileSystem.read_path`
+ src/integrations/prefect-azure/prefect_azure/blob_storage.py:705:15: error[invalid-method-override] Invalid override of method `write_path`: Definition is incompatible with `WritableFileSystem.write_path`
+ src/integrations/prefect-azure/prefect_azure/repository.py:190:9: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `ReadableDeploymentStorage.get_directory`
+ src/integrations/prefect-bitbucket/prefect_bitbucket/repository.py:161:15: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `ReadableDeploymentStorage.get_directory`
+ src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:726:15: error[invalid-method-override] Invalid override of method `wait_for_completion`: Definition is incompatible with `JobRun.wait_for_completion`
+ src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:739:15: error[invalid-method-override] Invalid override of method `fetch_result`: Definition is incompatible with `JobRun.fetch_result`
+ src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:1054:15: error[invalid-method-override] Invalid override of method `trigger`: Definition is incompatible with `JobBlock.trigger`
+ src/integrations/prefect-gcp/prefect_gcp/bigquery.py:634:15: error[invalid-method-override] Invalid override of method `fetch_one`: Definition is incompatible with `DatabaseBlock.fetch_one`
+ src/integrations/prefect-gcp/prefect_gcp/bigquery.py:693:15: error[invalid-method-override] Invalid override of method `fetch_many`: Definition is incompatible with `DatabaseBlock.fetch_many`
+ src/integrations/prefect-gcp/prefect_gcp/bigquery.py:760:15: error[invalid-method-override] Invalid override of method `fetch_all`: Definition is incompatible with `DatabaseBlock.fetch_all`
+ src/integrations/prefect-gcp/prefect_gcp/bigquery.py:817:15: error[invalid-method-override] Invalid override of method `execute`: Definition is incompatible with `DatabaseBlock.execute`
+ src/integrations/prefect-gcp/prefect_gcp/bigquery.py:864:15: error[invalid-method-override] Invalid override of method `execute_many`: Definition is incompatible with `DatabaseBlock.execute_many`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:731:15: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `WritableDeploymentStorage.get_directory`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:784:15: error[invalid-method-override] Invalid override of method `put_directory`: Definition is incompatible with `WritableDeploymentStorage.put_directory`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:864:15: error[invalid-method-override] Invalid override of method `read_path`: Definition is incompatible with `WritableFileSystem.read_path`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:882:15: error[invalid-method-override] Invalid override of method `write_path`: Definition is incompatible with `WritableFileSystem.write_path`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:1063:15: error[invalid-method-override] Invalid override of method `download_object_to_path`: Definition is incompatible with `ObjectStorageBlock.download_object_to_path`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:1113:15: error[invalid-method-override] Invalid override of method `download_object_to_file_object`: Definition is incompatible with `ObjectStorageBlock.download_object_to_file_object`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:1168:15: error[invalid-method-override] Invalid override of method `download_folder_to_path`: Definition is incompatible with `ObjectStorageBlock.download_folder_to_path`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:1236:15: error[invalid-method-override] Invalid override of method `upload_from_path`: Definition is incompatible with `ObjectStorageBlock.upload_from_path`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:1282:15: error[invalid-method-override] Invalid override of method `upload_from_file_object`: Definition is incompatible with `ObjectStorageBlock.upload_from_file_object`
+ src/integrations/prefect-gcp/prefect_gcp/cloud_storage.py:1337:15: error[invalid-method-override] Invalid override of method `upload_from_folder`: Definition is incompatible with `ObjectStorageBlock.upload_from_folder`
+ src/integrations/prefect-gcp/prefect_gcp/secret_manager.py:319:15: error[invalid-method-override] Invalid override of method `read_secret`: Definition is incompatible with `SecretBlock.read_secret`
+ src/integrations/prefect-gcp/prefect_gcp/secret_manager.py:340:15: error[invalid-method-override] Invalid override of method `write_secret`: Definition is incompatible with `SecretBlock.write_secret`
+ src/integrations/prefect-github/prefect_github/repository.py:100:15: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `ReadableDeploymentStorage.get_directory`
+ src/integrations/prefect-gitlab/prefect_gitlab/repositories.py:148:15: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `ReadableDeploymentStorage.get_directory`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:392:15: error[invalid-method-override] Invalid override of method `wait_for_completion`: Definition is incompatible with `JobRun.wait_for_completion`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:482:15: error[invalid-method-override] Invalid override of method `fetch_result`: Definition is incompatible with `JobRun.fetch_result`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:546:15: error[invalid-method-override] Invalid override of method `trigger`: Definition is incompatible with `JobBlock.trigger`
+ src/integrations/prefect-shell/prefect_shell/commands.py:165:15: error[invalid-method-override] Invalid override of method `wait_for_completion`: Definition is incompatible with `JobRun.wait_for_completion`
+ src/integrations/prefect-shell/prefect_shell/commands.py:186:15: error[invalid-method-override] Invalid override of method `fetch_result`: Definition is incompatible with `JobRun.fetch_result`
+ src/integrations/prefect-shell/prefect_shell/commands.py:328:15: error[invalid-method-override] Invalid override of method `trigger`: Definition is incompatible with `JobBlock.trigger`
+ src/integrations/prefect-slack/prefect_slack/credentials.py:127:9: error[invalid-method-override] Invalid override of method `notify`: Definition is incompatible with `NotificationBlock.notify`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:463:15: error[invalid-method-override] Invalid override of method `fetch_one`: Definition is incompatible with `DatabaseBlock.fetch_one`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:515:15: error[invalid-method-override] Invalid override of method `fetch_many`: Definition is incompatible with `DatabaseBlock.fetch_many`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:571:15: error[invalid-method-override] Invalid override of method `fetch_all`: Definition is incompatible with `DatabaseBlock.fetch_all`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:620:15: error[invalid-method-override] Invalid override of method `execute`: Definition is incompatible with `DatabaseBlock.execute`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:662:15: error[invalid-method-override] Invalid override of method `execute_many`: Definition is incompatible with `DatabaseBlock.execute_many`
- src/prefect/_internal/concurrency/calls.py:273:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/blocks/notifications.py:51:15: error[invalid-method-override] Invalid override of method `notify`: Definition is incompatible with `NotificationBlock.notify`
+ src/prefect/blocks/notifications.py:86:15: error[invalid-method-override] Invalid override of method `notify`: Definition is incompatible with `NotificationBlock.notify`
+ src/prefect/blocks/notifications.py:310:15: error[invalid-method-override] Invalid override of method `notify`: Definition is incompatible with `NotificationBlock.notify`
+ src/prefect/blocks/notifications.py:848:15: error[invalid-method-override] Invalid override of method `notify`: Definition is incompatible with `NotificationBlock.notify`
+ src/prefect/blocks/notifications.py:932:15: error[invalid-method-override] Invalid override of method `notify`: Definition is incompatible with `NotificationBlock.notify`
+ src/prefect/blocks/redis.py:90:9: error[invalid-method-override] Invalid override of method `read_path`: Definition is incompatible with `WritableFileSystem.read_path`
+ src/prefect/blocks/redis.py:131:9: error[invalid-method-override] Invalid override of method `write_path`: Definition is incompatible with `WritableFileSystem.write_path`
+ src/prefect/filesystems.py:165:9: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `WritableDeploymentStorage.get_directory`
+ src/prefect/filesystems.py:258:9: error[invalid-method-override] Invalid override of method `put_directory`: Definition is incompatible with `WritableDeploymentStorage.put_directory`
+ src/prefect/filesystems.py:318:9: error[invalid-method-override] Invalid override of method `read_path`: Definition is incompatible with `WritableFileSystem.read_path`
+ src/prefect/filesystems.py:350:9: error[invalid-method-override] Invalid override of method `write_path`: Definition is incompatible with `WritableFileSystem.write_path`
+ src/prefect/filesystems.py:428:15: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `WritableDeploymentStorage.get_directory`
+ src/prefect/filesystems.py:451:15: error[invalid-method-override] Invalid override of method `put_directory`: Definition is incompatible with `WritableDeploymentStorage.put_directory`
+ src/prefect/filesystems.py:505:15: error[invalid-method-override] Invalid override of method `read_path`: Definition is incompatible with `WritableFileSystem.read_path`
+ src/prefect/filesystems.py:514:15: error[invalid-method-override] Invalid override of method `write_path`: Definition is incompatible with `WritableFileSystem.write_path`
+ src/prefect/filesystems.py:610:15: error[invalid-method-override] Invalid override of method `get_directory`: Definition is incompatible with `WritableDeploymentStorage.get_directory`
+ src/prefect/filesystems.py:622:15: error[invalid-method-override] Invalid override of method `put_directory`: Definition is incompatible with `WritableDeploymentStorage.put_directory`
+ src/prefect/filesystems.py:640:15: error[invalid-method-override] Invalid override of method `read_path`: Definition is incompatible with `WritableFileSystem.read_path`
+ src/prefect/filesystems.py:644:15: error[invalid-method-override] Invalid override of method `write_path`: Definition is incompatible with `WritableFileSystem.write_path`
+ src/prefect/testing/fixtures.py:427:19: error[invalid-method-override] Invalid override of method `process_events`: Definition is incompatible with `EventsPipeline.process_events`
+ src/prefect/testing/fixtures.py:464:19: error[invalid-method-override] Invalid override of method `process_events`: Definition is incompatible with `EventsPipeline.process_events`
- Found 3299 diagnostics
+ Found 3377 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ tests/internal/bytecode_injection/framework_injection/_config.py:35:5: error[invalid-method-override] Invalid override of method `include`: Definition is incompatible with `Env.include`
+ tests/tracer/test_telemetry.py:24:5: error[invalid-method-override] Invalid override of method `count`: Definition is incompatible with `tuple.count`
- Found 8250 diagnostics
+ Found 8252 diagnostics

colour (https://github.com/colour-science/colour)
+ colour/io/luts/lut.py:370:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ colour/io/luts/operator.py:423:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ colour/io/luts/sequence.py:275:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ colour/utilities/structures.py:530:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
- Found 385 diagnostics
+ Found 389 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- com/win32com/server/policy.py:612:9: error[invalid-method-override] Invalid override of method `_invokeex_`: Definition is incompatible with `BasicWrapPolicy._invokeex_`
- com/win32com/server/policy.py:697:9: error[invalid-method-override] Invalid override of method `_invokeex_`: Definition is incompatible with `BasicWrapPolicy._invokeex_`
- Found 2656 diagnostics
+ Found 2654 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/common/bases.py:167:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ ibis/common/bases.py:205:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ ibis/common/collections.py:153:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ ibis/common/deferred.py:180:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ ibis/common/egraph.py:124:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ ibis/common/grounds.py:164:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
+ ibis/common/tests/test_deferred.py:238:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
- Found 4414 diagnostics
+ Found 4421 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/index_base.py:114:5: error[invalid-method-override] Invalid override of method `__add__`: Definition is incompatible with `ContainerOperandSequence.__add__`
+ static_frame/core/index_base.py:115:5: error[invalid-method-override] Invalid override of method `__sub__`: Definition is incompatible with `ContainerOperandSequence.__sub__`
+ static_frame/core/index_base.py:116:5: error[invalid-method-override] Invalid override of method `__mul__`: Definition is incompatible with `ContainerOperandSequence.__mul__`
+ static_frame/core/index_base.py:117:5: error[invalid-method-override] Invalid override of method `__matmul__`: Definition is incompatible with `ContainerOperandSequence.__matmul__`
+ static_frame/core/index_base.py:118:5: error[invalid-method-override] Invalid override of method `__truediv__`: Definition is incompatible with `ContainerOperandSequence.__truediv__`
+ static_frame/core/index_base.py:119:5: error[invalid-method-override] Invalid override of method `__floordiv__`: Definition is incompatible with `ContainerOperandSequence.__floordiv__`
+ static_frame/core/index_base.py:120:5: error[invalid-method-override] Invalid override of method `__mod__`: Definition is incompatible with `ContainerOperandSequence.__mod__`
+ static_frame/core/index_base.py:122:5: error[invalid-method-override] Invalid override of method `__pow__`: Definition is incompatible with `ContainerOperandSequence.__pow__`
+ static_frame/core/index_base.py:123:5: error[invalid-method-override] Invalid override of method `__lshift__`: Definition is incompatible with `ContainerOperandSequence.__lshift__`
+ static_frame/core/index_base.py:124:5: error[invalid-method-override] Invalid override of method `__rshift__`: Definition is incompatible with `ContainerOperandSequence.__rshift__`
+ static_frame/core/index_base.py:125:5: error[invalid-method-override] Invalid override of method `__and__`: Definition is incompatible with `ContainerOperandSequence.__and__`
+ static_frame/core/index_base.py:126:5: error[invalid-method-override] Invalid override of method `__xor__`: Definition is incompatible with `ContainerOperandSequence.__xor__`
+ static_frame/core/index_base.py:127:5: error[invalid-method-override] Invalid override of method `__or__`: Definition is incompatible with `ContainerOperandSequence.__or__`
+ static_frame/core/index_base.py:128:5: error[invalid-method-override] Invalid override of method `__lt__`: Definition is incompatible with `ContainerOperandSequence.__lt__`
+ static_frame/core/index_base.py:129:5: error[invalid-method-override] Invalid override of method `__le__`: Definition is incompatible with `ContainerOperandSequence.__le__`
+ static_frame/core/index_base.py:130:5: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `ContainerOperandSequence.__eq__`
+ static_frame/core/index_base.py:131:5: error[invalid-method-override] Invalid override of method `__ne__`: Definition is incompatible with `ContainerOperandSequence.__ne__`
+ static_frame/core/index_base.py:132:5: error[invalid-method-override] Invalid override of method `__gt__`: Definition is incompatible with `ContainerOperandSequence.__gt__`
+ static_frame/core/index_base.py:133:5: error[invalid-method-override] Invalid override of method `__ge__`: Definition is incompatible with `ContainerOperandSequence.__ge__`
+ static_frame/core/index_base.py:134:5: error[invalid-method-override] Invalid override of method `__radd__`: Definition is incompatible with `ContainerOperandSequence.__radd__`
+ static_frame/core/index_base.py:135:5: error[invalid-method-override] Invalid override of method `__rsub__`: Definition is incompatible with `ContainerOperandSequence.__rsub__`
+ static_frame/core/index_base.py:136:5: error[invalid-method-override] Invalid override of method `__rmul__`: Definition is incompatible with `ContainerOperandSequence.__rmul__`
+ static_frame/core/index_base.py:137:5: error[invalid-method-override] Invalid override of method `__rtruediv__`: Definition is incompatible with `ContainerOperandSequence.__rtruediv__`
+ static_frame/core/index_base.py:138:5: error[invalid-method-override] Invalid override of method `__rfloordiv__`: Definition is incompatible with `ContainerOperandSequence.__rfloordiv__`
- Found 1948 diagnostics
+ Found 1972 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/basearray.py:61:3: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
- jax/_src/core.py:550:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/core.py:902:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/core.py:3100:7: error[invalid-method-override] Invalid override of method `bind_with_trace`: Definition is incompatible with `Primitive.bind_with_trace`
- jax/_src/core.py:3145:7: error[invalid-method-override] Invalid override of method `bind_with_trace`: Definition is incompatible with `Primitive.bind_with_trace`
- jax/_src/custom_transpose.py:173:7: error[invalid-method-override] Invalid override of method `bind_with_trace`: Definition is incompatible with `Primitive.bind_with_trace`
- jax/_src/earray.py:33:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/prng.py:303:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/shard_map.py:668:7: error[invalid-method-override] Invalid override of method `bind_with_trace`: Definition is incompatible with `Primitive.bind_with_trace`
- jax/experimental/sparse/_base.py:34:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2727 diagnostics
+ Found 2719 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/arrays/base.py:2259:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas/core/generic.py:1880:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas/core/indexes/base.py:5263:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandas/tests/dtypes/test_inference.py:449:9: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
- Found 3617 diagnostics
+ Found 3615 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/core/frame.pyi:398:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas-stubs/core/generic.pyi:67:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas-stubs/core/indexes/base.pyi:129:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandas-stubs/core/series.pyi:344:5: error[invalid-method-override] Invalid override of method `__hash__`: Definition is incompatible with `object.__hash__`
- Found 6114 diagnostics
+ Found 6112 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/calculus/accumulationbounds.py:284:9: error[invalid-method-override] Invalid override of method `_eval_power`: Definition is incompatible with `Expr._eval_power`
+ sympy/calculus/accumulationbounds.py:473:9: error[invalid-method-override] Invalid override of method `__pow__`: Definition is incompatible with `Expr.__pow__`
+ sympy/core/numbers.py:1718:9: error[invalid-method-override] Invalid override of method `gcd`: Definition is incompatible with `Number.gcd`
+ sympy/core/numbers.py:1728:9: error[invalid-method-override] Invalid override of method `lcm`: Definition is incompatible with `Number.lcm`
+ sympy/core/tests/test_priority.py:26:9: error[invalid-method-override] Invalid override of method `__mul__`: Definition is incompatible with `Integer.__mul__`
+ sympy/core/tests/test_priority.py:30:9: error[invalid-method-override] Invalid override of method `__rmul__`: Definition is incompatible with `Integer.__rmul__`
+ sympy/core/tests/test_priority.py:34:9: error[invalid-method-override] Invalid override of method `__add__`: Definition is incompatible with `Integer.__add__`
+ sympy/core/tests/test_priority.py:38:9: error[invalid-method-override] Invalid override of method `__radd__`: Definition is incompatible with `Integer.__radd__`
+ sympy/core/tests/test_priority.py:42:9: error[invalid-method-override] Invalid override of method `__sub__`: Definition is incompatible with `Integer.__sub__`
+ sympy/core/tests/test_priority.py:46:9: error[invalid-method-override] Invalid override of method `__rsub__`: Definition is incompatible with `Integer.__rsub__`
+ sympy/core/tests/test_priority.py:50:9: error[invalid-method-override] Invalid override of method `__pow__`: Definition is incompatible with `Integer.__pow__`
+ sympy/core/tests/test_priority.py:66:9: error[invalid-method-override] Invalid override of method `__mod__`: Definition is incompatible with `Integer.__mod__`
+ sympy/core/tests/test_priority.py:70:9: error[invalid-method-override]

... (truncated 573 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct metadata = ~20MB
+     struct metadata = ~21MB
-     struct fields = ~20MB
+     struct fields = ~21MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~626MB
+ TOTAL MEMORY USAGE: ~657MB
-     struct metadata = ~40MB
+     struct metadata = ~47MB
-     struct fields = ~44MB
+     struct fields = ~49MB
-     memo metadata = ~138MB
+     memo metadata = ~159MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-24 11:56_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-method-override` | 716 | 6 | 0 |
| `unused-ignore-comment` | 0 | 147 | 0 |
| **Total** | **716** | **153** | **0** |

**[Full report with detailed diff](https://alex-liskov-method-overridde.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-liskov-method-overridde.ecosystem-663.pages.dev/timing))




---

_Comment by @codspeed-hq[bot] on 2025-11-24 12:00_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fliskov-method-overridden-by-non-method?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21611 will **degrade performances by 25.02%**

<sub>Comparing <code>alex/liskov-method-overridden-by-non-method</code> (bff97d0) with <code>main</code> (b19ddca)</sub>



### Summary

`❌ 1` regression  
`✅ 51` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fliskov-method-overridden-by-non-method?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` ty_micro[many_enum_members] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fliskov-method-overridden-by-non-method?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members%3A%3Aty_micro%5Bmany_enum_members%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 94.6 ms | 126.1 ms | -25.02% |


---

_Comment by @astral-sh-bot[bot] on 2025-11-24 14:20_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-24 21:16_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-24 21:17_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-25 10:53_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-25 10:53_

---
