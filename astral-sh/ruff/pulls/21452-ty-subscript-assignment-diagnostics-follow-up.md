```yaml
number: 21452
title: "[ty] Subscript assignment diagnostics follow-up"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/follow-up-setitem
created_at: 2025-11-14T13:31:08Z
updated_at: 2025-11-17T11:14:59Z
url: https://github.com/astral-sh/ruff/pull/21452
synced_at: 2026-01-10T16:53:56Z
```

# [ty] Subscript assignment diagnostics follow-up

---

_Pull request opened by @sharkdp on 2025-11-14 13:31_

## Summary

Follow up from https://github.com/astral-sh/ruff/pull/21411. Again, there are more things that could be improved here (like the diagnostics for `lists`, or extending what we have for `dict` to `OrderedDict` etc), but that will have to be postponed.

---

_Review requested from @carljm by @sharkdp on 2025-11-14 13:31_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-14 13:31_

---

_Review requested from @dcreager by @sharkdp on 2025-11-14 13:31_

---

_Label `ty` added by @sharkdp on 2025-11-14 13:31_

---

_Label `diagnostics` added by @sharkdp on 2025-11-14 13:31_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 13:33_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-17 11:12:13.684914983 +0000
+++ new-output.txt	2025-11-17 11:12:17.143922916 +0000
@@ -963,7 +963,7 @@
 typeddicts_extra_items.py:339:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
 typeddicts_extra_items.py:339:13: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `popitem`
 typeddicts_extra_items.py:342:27: error[invalid-key] TypedDict `IntDictWithNum` can only be subscripted with a string literal key, got key of type `str`.
-typeddicts_extra_items.py:343:31: error[invalid-key] TypedDict `IntDictWithNum` can only be subscripted with string literal keys, got key of type `str`
+typeddicts_extra_items.py:343:31: error[invalid-key] TypedDict `IntDictWithNum` can only be subscripted with a string literal key, got key of type `str`
 typeddicts_extra_items.py:352:5: error[invalid-assignment] Object of type `dict[str, int]` is not assignable to `IntDict`
 typeddicts_operations.py:22:17: error[invalid-assignment] Invalid assignment to key "name" with declared type `str` on TypedDict `Movie`: value of type `Literal[1982]`
 typeddicts_operations.py:23:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal[""]`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-14 13:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/markupsafe/__init__.py:248:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]) | (bound method _ListOrDict@_escape_argspec.__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Any` and a value of type `Markup` on object of type `_ListOrDict@_escape_argspec`
+ lib/spack/spack/vendor/markupsafe/__init__.py:248:13: error[invalid-assignment] Invalid subscript assignment with key of type `Any` and value of type `Markup` on object of type `_ListOrDict@_escape_argspec`

werkzeug (https://github.com/pallets/werkzeug)
- tests/test_test.py:149:5: error[invalid-assignment] Method `__setitem__` of type `bound method MultiDict[str, str].__setitem__(key: str, value: str) -> None` cannot be called with a key of type `Literal["test_int"]` and a value of type `Literal[1]` on object of type `MultiDict[str, str]`
+ tests/test_test.py:149:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["test_int"]` and value of type `Literal[1]` on object of type `MultiDict[str, str]`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/pastebin.py:43:13: error[invalid-assignment] Method `__setitem__` of type `bound method Stash.__setitem__[T](key: StashKey[T@__setitem__], value: T@__setitem__) -> None` cannot be called with a key of type `StashKey[IO[bytes]]` and a value of type `TextIOWrapper[_WrappedBuffer]` on object of type `Stash`
+ src/_pytest/pastebin.py:43:13: error[invalid-assignment] Invalid subscript assignment with key of type `StashKey[IO[bytes]]` and value of type `TextIOWrapper[_WrappedBuffer]` on object of type `Stash`

scrapy (https://github.com/scrapy/scrapy)
- scrapy/http/headers.py:91:9: error[invalid-assignment] Method `__setitem__` of type `bound method Self@setlist.__setitem__[AnyStr](key: AnyStr@__setitem__, value: Any) -> None` cannot be called with a key of type `AnyStr@setlist` and a value of type `Iterable[@Todo]` on object of type `Self@setlist`
+ scrapy/http/headers.py:91:9: error[invalid-assignment] Invalid subscript assignment with key of type `AnyStr@setlist` and value of type `Iterable[@Todo]` on object of type `Self@setlist`
- scrapy/http/headers.py:101:9: error[invalid-assignment] Method `__setitem__` of type `bound method Self@appendlist.__setitem__[AnyStr](key: AnyStr@__setitem__, value: Any) -> None` cannot be called with a key of type `AnyStr@appendlist` and a value of type `list[bytes]` on object of type `Self@appendlist`
+ scrapy/http/headers.py:101:9: error[invalid-assignment] Invalid subscript assignment with key of type `AnyStr@appendlist` and value of type `list[bytes]` on object of type `Self@appendlist`
- scrapy/utils/python.py:152:13: error[invalid-assignment] Method `__setitem__` of type `bound method WeakKeyDictionary[typing.TypeVar, _T@memoizemethod_noargs].__setitem__(key: typing.TypeVar, value: _T@memoizemethod_noargs) -> None` cannot be called with a key of type `_SelfT@new_method` and a value of type `_T@memoizemethod_noargs` on object of type `WeakKeyDictionary[typing.TypeVar, _T@memoizemethod_noargs]`
+ scrapy/utils/python.py:152:13: error[invalid-assignment] Invalid subscript assignment with key of type `_SelfT@new_method` and value of type `_T@memoizemethod_noargs` on object of type `WeakKeyDictionary[typing.TypeVar, _T@memoizemethod_noargs]`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/log.py:131:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["level"]` and a value of type `Unknown | Literal[20]` on object of type `list[Unknown | str]`
+ sockeye/log.py:131:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["level"]` and value of type `Unknown | Literal[20]` on object of type `list[Unknown | str]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/httpclient_test.py:521:17: error[invalid-assignment] Method `__setitem__` of type `bound method HTTPHeaders.__setitem__(name: str, value: str) -> None` cannot be called with a key of type `Literal["User-Agent"]` and a value of type `Unknown | str | bytes` on object of type `HTTPHeaders`
+ tornado/test/httpclient_test.py:521:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["User-Agent"]` and value of type `Unknown | str | bytes` on object of type `HTTPHeaders`

pandera (https://github.com/pandera-dev/pandera)
- pandera/decorators.py:393:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `int | str` and a value of type `Unknown` on object of type `list[Unknown]`
+ pandera/decorators.py:393:17: error[invalid-assignment] Invalid subscript assignment with key of type `int | str` and value of type `Unknown` on object of type `list[Unknown]`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]` cannot be called with a key of type `Literal["text"]` and a value of type `str` on object of type `list[dict[Unknown, Unknown]]`
+ mitmproxy/addons/savehar.py:206:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["text"]` and value of type `str` on object of type `list[dict[Unknown, Unknown]]`
- mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]` cannot be called with a key of type `Literal["encoding"]` and a value of type `Literal["base64"]` on object of type `list[dict[Unknown, Unknown]]`
+ mitmproxy/addons/savehar.py:207:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["encoding"]` and value of type `Literal["base64"]` on object of type `list[dict[Unknown, Unknown]]`
- mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]` cannot be called with a key of type `Literal["text"]` and a value of type `Literal[""]` on object of type `list[dict[Unknown, Unknown]]`
+ mitmproxy/addons/savehar.py:211:21: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["text"]` and value of type `Literal[""]` on object of type `list[dict[Unknown, Unknown]]`
- mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[Unknown, Unknown], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[Unknown, Unknown]], /) -> None]` cannot be called with a key of type `Literal["text"]` and a value of type `str` on object of type `list[dict[Unknown, Unknown]]`
+ mitmproxy/addons/savehar.py:213:21: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["text"]` and value of type `str` on object of type `list[dict[Unknown, Unknown]]`
- test/mitmproxy/test_http.py:602:9: error[invalid-assignment] Method `__setitem__` of type `bound method MultiDictView[str, tuple[str, MultiDict[str, str | None]]].__setitem__(key: str, value: tuple[str, MultiDict[str, str | None]]) -> None` cannot be called with a key of type `Literal["foo"]` and a value of type `tuple[Literal["bar"], dict[Unknown, Unknown]]` on object of type `MultiDictView[str, tuple[str, MultiDict[str, str | None]]]`
+ test/mitmproxy/test_http.py:602:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["foo"]` and value of type `tuple[Literal["bar"], dict[Unknown, Unknown]]` on object of type `MultiDictView[str, tuple[str, MultiDict[str, str | None]]]`

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/schema.py:541:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]` cannot be called with a key of type `Literal["name"]` and a value of type `str` on object of type `MutableSequence[Any]`
+ schema_salad/schema.py:541:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["name"]` and value of type `str` on object of type `MutableSequence[Any]`
- schema_salad/schema.py:543:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]` cannot be called with a key of type `Literal["name"]` and a value of type `str` on object of type `MutableSequence[Any]`
+ schema_salad/schema.py:543:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["name"]` and value of type `str` on object of type `MutableSequence[Any]`
- schema_salad/schema.py:563:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]` cannot be called with a key of type `Literal["type", "items", "names", "values", "fields"]` and a value of type `MutableMapping[str, Any] | MutableSequence[Any] | str | MutableMapping[str, str] | list[Any | MutableMapping[str, str] | str]` on object of type `MutableSequence[Any]`
+ schema_salad/schema.py:563:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["type", "items", "names", "values", "fields"]` and value of type `MutableMapping[str, Any] | MutableSequence[Any] | str | MutableMapping[str, str] | list[Any | MutableMapping[str, str] | str]` on object of type `MutableSequence[Any]`
- schema_salad/schema.py:572:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(index: int, value: Any) -> None, (index: slice[Any, Any, Any], value: Iterable[Any]) -> None]` cannot be called with a key of type `Literal["symbols"]` and a value of type `list[str | Unknown]` on object of type `MutableSequence[Any]`
+ schema_salad/schema.py:572:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["symbols"]` and value of type `list[str | Unknown]` on object of type `MutableSequence[Any]`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/environment/__init__.py:1176:17: error[invalid-assignment] Method `__setitem__` of type `bound method Self@update.__setitem__(key: str, value: Any) -> None` cannot be called with a key of type `object` and a value of type `object` on object of type `Self@update`
+ sphinx/environment/__init__.py:1176:17: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `object` on object of type `Self@update`

manticore (https://github.com/trailofbits/manticore)
- manticore/platforms/decree.py:850:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | None, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | None], /) -> None]` cannot be called with a key of type `Unknown | int | None` and a value of type `Unknown | int` on object of type `list[Unknown | None]`
- manticore/platforms/decree.py:852:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | None, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | None], /) -> None]` cannot be called with a key of type `Unknown | int | None` and a value of type `None` on object of type `list[Unknown | None]`
+ manticore/platforms/decree.py:850:13: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown | int | None` and value of type `Unknown | int` on object of type `list[Unknown | None]`
+ manticore/platforms/decree.py:852:13: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown | int | None` and value of type `None` on object of type `list[Unknown | None]`

discord.py (https://github.com/Rapptz/discord.py)
- discord/message.py:2404:30: error[invalid-key] TypedDict `Message` can only be subscripted with string literal keys, got key of type `str`
+ discord/message.py:2404:30: error[invalid-key] TypedDict `Message` can only be subscripted with a string literal key, got key of type `str`

apprise (https://github.com/caronc/apprise)
- apprise/plugins/d7networks.py:238:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | None | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | None | str]], /) -> None]` cannot be called with a key of type `Literal["originator"]` and a value of type `Unknown & ~AlwaysFalsy` on object of type `list[Unknown | dict[Unknown | str, Unknown | None | str]]`
+ apprise/plugins/d7networks.py:238:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["originator"]` and value of type `Unknown & ~AlwaysFalsy` on object of type `list[Unknown | dict[Unknown | str, Unknown | None | str]]`
- apprise/plugins/email/base.py:1129:17: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEMultipart.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart`
- apprise/plugins/email/base.py:1129:17: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEText.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEText`
+ apprise/plugins/email/base.py:1129:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["Subject"]` and value of type `Header` on object of type `MIMEMultipart`
+ apprise/plugins/email/base.py:1129:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["Subject"]` and value of type `Header` on object of type `MIMEText`
- apprise/plugins/email/base.py:1165:17: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEMultipart.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Unknown` and a value of type `Header` on object of type `MIMEMultipart`
- apprise/plugins/email/base.py:1165:17: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEText.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Unknown` and a value of type `Header` on object of type `MIMEText`
- apprise/plugins/email/base.py:1167:13: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEMultipart.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart`
- apprise/plugins/email/base.py:1167:13: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEText.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEText`
+ apprise/plugins/email/base.py:1165:17: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `Header` on object of type `MIMEMultipart`
+ apprise/plugins/email/base.py:1165:17: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `Header` on object of type `MIMEText`
+ apprise/plugins/email/base.py:1167:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["Subject"]` and value of type `Header` on object of type `MIMEMultipart`
+ apprise/plugins/email/base.py:1167:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["Subject"]` and value of type `Header` on object of type `MIMEText`
- apprise/plugins/ses.py:470:13: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEMultipart.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart`
- apprise/plugins/ses.py:470:13: error[invalid-assignment] Method `__setitem__` of type `bound method MIMEText.__setitem__(name: str, val: str) -> None` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEText`
+ apprise/plugins/ses.py:470:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["Subject"]` and value of type `Header` on object of type `MIMEMultipart`
+ apprise/plugins/ses.py:470:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["Subject"]` and value of type `Header` on object of type `MIMEText`

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/net/network_state.py:340:17: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["dns"]` and a value of type `dict[Unknown | str, Unknown]` on object of type `list[Unknown]`
+ cloudinit/net/network_state.py:340:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["dns"]` and value of type `dict[Unknown | str, Unknown]` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:437:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["mac_address"]` and a value of type `Unknown` on object of type `list[Unknown]`
+ cloudinit/net/network_state.py:437:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["mac_address"]` and value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:456:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["vlan-raw-device"]` and a value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:457:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["vlan_id"]` and a value of type `Unknown` on object of type `list[Unknown]`
+ cloudinit/net/network_state.py:456:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["vlan-raw-device"]` and value of type `Unknown` on object of type `list[Unknown]`
+ cloudinit/net/network_state.py:457:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["vlan_id"]` and value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:507:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["bond-master"]` and a value of type `Unknown` on object of type `list[Unknown]`
+ cloudinit/net/network_state.py:507:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["bond-master"]` and value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:558:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["bridge_ports"]` and a value of type `Unknown` on object of type `list[Unknown]`
+ cloudinit/net/network_state.py:558:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["bridge_ports"]` and value of type `Unknown` on object of type `list[Unknown]`
- cloudinit/net/network_state.py:616:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["dns"]` and a value of type `dict[Unknown | str, Unknown]` on object of type `list[Unknown]`
+ cloudinit/net/network_state.py:616:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["dns"]` and value of type `dict[Unknown | str, Unknown]` on object of type `list[Unknown]`
- cloudinit/sources/DataSourceVMware.py:1000:48: error[invalid-key] TypedDict `Interface` can only be subscripted with string literal keys, got key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1000:48: error[invalid-key] TypedDict `Interface` can only be subscripted with a string literal key, got key of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1001:26: error[invalid-key] TypedDict `Interface` can only be subscripted with string literal keys, got key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1001:26: error[invalid-key] TypedDict `Interface` can only be subscripted with a string literal key, got key of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1008:48: error[invalid-key] TypedDict `Interface` can only be subscripted with string literal keys, got key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1008:48: error[invalid-key] TypedDict `Interface` can only be subscripted with a string literal key, got key of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1009:26: error[invalid-key] TypedDict `Interface` can only be subscripted with string literal keys, got key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1009:26: error[invalid-key] TypedDict `Interface` can only be subscripted with a string literal key, got key of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1018:53: error[invalid-key] TypedDict `Interface` can only be subscripted with string literal keys, got key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1018:53: error[invalid-key] TypedDict `Interface` can only be subscripted with a string literal key, got key of type `Literal[0]`
- cloudinit/sources/DataSourceVMware.py:1028:53: error[invalid-key] TypedDict `Interface` can only be subscripted with string literal keys, got key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1028:53: error[invalid-key] TypedDict `Interface` can only be subscripted with a string literal key, got key of type `Literal[0]`
- tests/unittests/helpers.py:422:5: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["distro"]` and a value of type `Unknown` on object of type `list[Unknown | str]`
+ tests/unittests/helpers.py:422:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["distro"]` and value of type `Unknown` on object of type `list[Unknown | str]`
- tests/unittests/helpers.py:422:5: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["distro"]` and a value of type `Unknown` on object of type `list[Unknown]`
+ tests/unittests/helpers.py:422:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["distro"]` and value of type `Unknown` on object of type `list[Unknown]`
- tests/unittests/helpers.py:424:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["renderers"]` and a value of type `Unknown & ~AlwaysFalsy` on object of type `list[Unknown]`
+ tests/unittests/helpers.py:424:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["renderers"]` and value of type `Unknown & ~AlwaysFalsy` on object of type `list[Unknown]`
- tests/unittests/helpers.py:426:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["activators"]` and a value of type `Unknown & ~AlwaysFalsy` on object of type `list[Unknown]`
+ tests/unittests/helpers.py:426:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["activators"]` and value of type `Unknown & ~AlwaysFalsy` on object of type `list[Unknown]`
- tests/unittests/sources/test_azure.py:4096:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Unknown` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_azure.py:4158:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Literal["Running"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_azure.py:4281:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Literal["Running"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_azure.py:4405:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Literal["Savable"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_azure.py:4537:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Literal["Savable"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_azure.py:4691:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Literal["Savable"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_azure.py:4823:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Unknown` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_azure.py:4908:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Unknown` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_azure.py:4951:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]], /) -> None]` cannot be called with a key of type `Literal["ppsType"]` and a value of type `Literal["PreprovisionedOSDisk"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4096:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Unknown` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4158:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Literal["Running"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4281:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Literal["Running"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4405:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Literal["Savable"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4537:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Literal["Savable"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4691:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Literal["Savable"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4823:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Unknown` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4908:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Unknown` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
+ tests/unittests/sources/test_azure.py:4951:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["ppsType"]` and value of type `Literal["PreprovisionedOSDisk"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown]] | str]]`
- tests/unittests/sources/test_cloudsigma.py:94:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["base64_fields"]` and a value of type `Literal["cloudinit-user-data"]` on object of type `list[Unknown]`
- tests/unittests/sources/test_cloudsigma.py:94:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["base64_fields"]` and a value of type `Literal["cloudinit-user-data"]` on object of type `list[Unknown | str]`
+ tests/unittests/sources/test_cloudsigma.py:94:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["base64_fields"]` and value of type `Literal["cloudinit-user-data"]` on object of type `list[Unknown]`
+ tests/unittests/sources/test_cloudsigma.py:94:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["base64_fields"]` and value of type `Literal["cloudinit-user-data"]` on object of type `list[Unknown | str]`
- tests/unittests/sources/test_cloudsigma.py:95:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]` cannot be called with a key of type `Literal["cloudinit-user-data"]` and a value of type `Literal["aGkgd29ybGQK"]` on object of type `list[Unknown]`
- tests/unittests/sources/test_cloudsigma.py:95:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["cloudinit-user-data"]` and a value of type `Literal["aGkgd29ybGQK"]` on object of type `list[Unknown | str]`
+ tests/unittests/sources/test_cloudsigma.py:95:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["cloudinit-user-data"]` and value of type `Literal["aGkgd29ybGQK"]` on object of type `list[Unknown]`
+ tests/unittests/sources/test_cloudsigma.py:95:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["cloudinit-user-data"]` and value of type `Literal["aGkgd29ybGQK"]` on object of type `list[Unknown | str]`
- tests/unittests/sources/test_init.py:951:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["secure"]` and a value of type `Literal["redacted"]` on object of type `list[Unknown | str]`
- tests/unittests/sources/test_init.py:961:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["secure"]` and a value of type `Literal["redacted for non-root user"]` on object of type `list[Unknown | str]`
+ tests/unittests/sources/test_init.py:951:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["secure"]` and value of type `Literal["redacted"]` on object of type `list[Unknown | str]`
+ tests/unittests/sources/test_init.py:961:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["secure"]` and value of type `Literal["redacted for non-root user"]` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:931:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/cloud.cfg.d/99_networklayer_common.cfg"]` and a value of type `Literal["datasource_list: [ ConfigDrive, IBMCloud ]\n"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:931:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/cloud.cfg.d/99_networklayer_common.cfg"]` and a value of type `Literal["datasource_list: [ ConfigDrive, IBMCloud ]\n"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:931:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/cloud.cfg.d/99_networklayer_common.cfg"]` and value of type `Literal["datasource_list: [ ConfigDrive, IBMCloud ]\n"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:931:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/cloud.cfg.d/99_networklayer_common.cfg"]` and value of type `Literal["datasource_list: [ ConfigDrive, IBMCloud ]\n"]` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:936:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/cloud.cfg.d/99_networklayer_common.cfg"]` and a value of type `Literal["datasource_list: [ ConfigDrive, NoCloud ]\n"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:936:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/cloud.cfg.d/99_networklayer_common.cfg"]` and a value of type `Literal["datasource_list: [ ConfigDrive, NoCloud ]\n"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:936:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/cloud.cfg.d/99_networklayer_common.cfg"]` and value of type `Literal["datasource_list: [ ConfigDrive, NoCloud ]\n"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:936:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/cloud.cfg.d/99_networklayer_common.cfg"]` and value of type `Literal["datasource_list: [ ConfigDrive, NoCloud ]\n"]` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/ds-identify.cfg"]` and a value of type `str` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/ds-identify.cfg"]` and a value of type `str` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `str` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `str` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:1045:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/cloud.cfg.d/myds.cfg"]` and a value of type `Literal["datasource_list: [\"NoCloud\"]\n"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1045:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/cloud.cfg.d/myds.cfg"]` and a value of type `Literal["datasource_list: [\"NoCloud\"]\n"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1045:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/cloud.cfg.d/myds.cfg"]` and value of type `Literal["datasource_list: [\"NoCloud\"]\n"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:1045:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/cloud.cfg.d/myds.cfg"]` and value of type `Literal["datasource_list: [\"NoCloud\"]\n"]` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:1057:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/cloud.cfg.d/myds.cfg"]` and a value of type `Literal["datasource_list: [\"Ec2\", \"None\"]\n"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1057:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["etc/cloud/cloud.cfg.d/myds.cfg"]` and a value of type `Literal["datasource_list: [\"Ec2\", \"None\"]\n"]` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1057:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/cloud.cfg.d/myds.cfg"]` and value of type `Literal["datasource_list: [\"Ec2\", \"None\"]\n"]` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:1057:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/cloud.cfg.d/myds.cfg"]` and value of type `Literal["datasource_list: [\"Ec2\", \"None\"]\n"]` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:1095:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]` cannot be called with a key of type `Literal["sys/class/dmi/id/product_name"]` and a value of type `Unknown | str` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1095:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]` cannot be called with a key of type `Literal["sys/class/dmi/id/product_name"]` and a value of type `Unknown | str` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1095:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["sys/class/dmi/id/product_name"]` and value of type `Unknown | str` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:1095:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["sys/class/dmi/id/product_name"]` and value of type `Unknown | str` on object of type `list[Unknown | str]`
- tests/unittests/test_ds_identify.py:1149:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]` cannot be called with a key of type `Literal["sys/class/dmi/id/chassis_asset_tag"]` and a value of type `Litera

... (truncated 414 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3892 on 2025-11-14 13:35_

```suggestion
                                    diagnostic.info(format_args!(
```

---

_@AlexWaygood approved on 2025-11-14 13:35_

Thank you!!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:3842 on 2025-11-14 13:39_

you can be a bit more DRY:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index cfc248d42e..9b12ced6d6 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -3794,18 +3794,18 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                                             let assigned_d = rhs_value_ty.display(db);
                                             let object_d = object_ty.display(db);
 
+                                            let mut diagnostic = builder.into_diagnostic(format_args!(
+                                                "Invalid subscript assignment with key of type `{}` and value of \
+                                                    type `{assigned_d}` on object of type `{object_d}`",
+                                                slice_ty.display(db),
+                                            ));
+
                                             // Special diagnostic for dictionaries
                                             if let Some([expected_key_ty, expected_value_ty]) =
                                                 object_ty
                                                     .known_specialization(db, KnownClass::Dict)
                                                     .map(|s| s.types(db))
                                             {
-                                                let mut diagnostic = builder.into_diagnostic(format_args!(
-                                                    "Invalid subscript assignment with key of type `{}` and value of \
-                                                     type `{assigned_d}` on object of type `{object_d}`",
-                                                    slice_ty.display(db),
-                                                ));
-
                                                 if !slice_ty.is_assignable_to(db, *expected_key_ty)
                                                 {
                                                     diagnostic.annotate(
@@ -3833,13 +3833,6 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                                                     );
                                                 }
 
-                                                attach_original_type_info(&mut diagnostic);
-                                            } else {
-                                                let mut diagnostic = builder.into_diagnostic(format_args!(
-                                                    "Invalid subscript assignment with key of type `{}` and value of \
-                                                     type `{assigned_d}` on object of type `{object_d}`",
-                                                    slice_ty.display(db),
-                                                ));
                                                 attach_original_type_info(&mut diagnostic);
                                             }
                                         }
```

(all tests pass with that patch applied)

---

_@AlexWaygood reviewed on 2025-11-14 13:39_

---

_Merged by @sharkdp on 2025-11-17 11:14_

---

_Closed by @sharkdp on 2025-11-17 11:14_

---

_Branch deleted on 2025-11-17 11:14_

---
