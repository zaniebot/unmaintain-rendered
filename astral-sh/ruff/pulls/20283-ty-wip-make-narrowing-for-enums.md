```yaml
number: 20283
title: "[ty][WIP] make narrowing for enums"
type: pull_request
state: closed
author: Renkai
labels: []
assignees: []
draft: true
base: main
head: renkai-narrow-enum
created_at: 2025-09-07T01:32:28Z
updated_at: 2025-09-07T02:14:53Z
url: https://github.com/astral-sh/ruff/pull/20283
synced_at: 2026-01-10T17:46:21Z
```

# [ty][WIP] make narrowing for enums

---

_Pull request opened by @Renkai on 2025-09-07 01:32_

Finished todos left in https://github.com/astral-sh/ruff/pull/20164

---

_Comment by @github-actions[bot] on 2025-09-07 01:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-07 01:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/_check/error/_pep/pep484585/errpep484585container.py:62:9: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` cannot be called with key of type `HintSign | None` on object of type `dict[HintSign, range]`
+ beartype/_check/error/_pep/pep484585/errpep484585mapping.py:56:9: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` cannot be called with key of type `HintSign | None` on object of type `dict[HintSign, range]`
- Found 588 diagnostics
+ Found 590 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/http.py:391:40: error[invalid-argument-type] Argument to function `unquote` is incorrect: Expected `str`, found `None | Unknown | str`
+ src/werkzeug/http.py:553:34: error[invalid-argument-type] Argument to function `unquote` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | None`
- Found 369 diagnostics
+ Found 371 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_highlevel_serve_listeners.py:52:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Mapping[int, str].__getitem__(key: int, /) -> str` cannot be called with key of type `int | None` on object of type `Mapping[int, str]`
+ src/trio/_highlevel_serve_listeners.py:53:37: error[invalid-argument-type] Argument to function `strerror` is incorrect: Expected `int`, found `int | None`
- Found 676 diagnostics
+ Found 678 diagnostics

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
+ flake8_pyi/visitor.py:958:43: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None`
- Found 8 diagnostics
+ Found 9 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/distributed/utils/__init__.py:154:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["xla-tpu"]`
+ tests/ignite/distributed/utils/__init__.py:154:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["xla-tpu"]`
- tests/ignite/distributed/utils/__init__.py:157:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:157:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["horovod"]`
- tests/ignite/distributed/utils/__init__.py:258:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:258:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["horovod"]`
- tests/ignite/distributed/utils/__init__.py:286:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["xla-tpu"]`
+ tests/ignite/distributed/utils/__init__.py:286:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["xla-tpu"]`

apprise (https://github.com/caronc/apprise)
+ apprise/config/base.py:1171:37: error[invalid-argument-type] Argument to function `_special_token_handler` is incorrect: Expected `str`, found `None | Unknown | str`
+ apprise/config/base.py:1184:29: error[invalid-argument-type] Argument to function `_special_token_handler` is incorrect: Expected `str`, found `None | Unknown | str`
- Found 1878 diagnostics
+ Found 1880 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
+ dragonchain/webserver/lib/dragonnet.py:82:120: error[invalid-argument-type] Argument to function `verification_storage_location` is incorrect: Expected `str`, found `str | Unknown | None`
+ dragonchain/webserver/lib/dragonnet.py:89:79: error[invalid-argument-type] Argument to function `add_receipt` is incorrect: Expected `str`, found `str | Unknown | None`
+ dragonchain/webserver/lib/dragonnet.py:93:124: error[invalid-argument-type] Argument to function `set_receieved_verification_for_block_from_chain_sync` is incorrect: Expected `str`, found `str | Unknown | None`
- Found 303 diagnostics
+ Found 306 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/config/__init__.py:498:25: warning[possibly-unbound-attribute] Attribute `replace` on type `str | None` is possibly unbound
- Found 471 diagnostics
+ Found 472 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/upstream/addbook.py:383:16: warning[possibly-unbound-attribute] Attribute `startswith` on type `str | None` is possibly unbound
- Found 710 diagnostics
+ Found 711 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/environment/__init__.py:397:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Literal[False] | ((Node, /) -> bool)].__getitem__(key: str, /) -> Literal[False] | ((Node, /) -> bool)` cannot be called with key of type `(str & ~((...) -> object)) | (((Node, /) -> bool) & ~((...) -> object))` on object of type `dict[str, Literal[False] | ((Node, /) -> bool)]`
+ sphinx/environment/__init__.py:397:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Literal[False] | ((Node, /) -> bool)].__getitem__(key: str, /) -> Literal[False] | ((Node, /) -> bool)` cannot be called with key of type `(str & ~((...) -> object)) | (((Node, /) -> bool) & str & ~((...) -> object)) | (((Node, /) -> bool) & ~((...) -> object))` on object of type `dict[str, Literal[False] | ((Node, /) -> bool)]`

core (https://github.com/home-assistant/core)
+ homeassistant/components/network/__init__.py:90:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | None | @Todo(Subscript expressions on intersections)`
+ homeassistant/components/tibber/sensor.py:556:33: warning[possibly-unbound-attribute] Attribute `replace` on type `str | None` is possibly unbound
- Found 13343 diagnostics
+ Found 13345 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/core/expr.py:3304:36: error[unsupported-operator] Operator `**` is unsupported between objects of type `Unknown | None` and `Unknown | Literal[6]`
- sympy/crypto/crypto.py:2257:12: warning[possibly-unbound-attribute] Attribute `join` on type `int | Unknown` is possibly unbound
+ sympy/crypto/crypto.py:2257:12: warning[possibly-unbound-attribute] Attribute `join` on type `Unknown | int` is possibly unbound
- sympy/simplify/cse_main.py:676:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `(Unknown & Basic) | (Unknown & Unevaluated)` with `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ sympy/simplify/cse_main.py:676:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `(Unknown & Basic) | (Unknown & Unevaluated & Basic) | (Unknown & Unevaluated)` with `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- Found 13409 diagnostics
+ Found 13410 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/exchanges/utils.py:201:37: warning[possibly-unresolved-reference] Name `url` used when possibly not defined
- Found 1598 diagnostics
+ Found 1597 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/sparse/_compressed.py:507:16: error[unsupported-operator] Operator `%` is unsupported between objects of type `Unknown | None` and `Literal[2]`
- Found 6429 diagnostics
+ Found 6430 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @Renkai on 2025-09-07 02:14_

though it works, it makes too many differences than I expected, can't vibe out a simpler one yet, will archive it first.

---

_Closed by @Renkai on 2025-09-07 02:14_

---
