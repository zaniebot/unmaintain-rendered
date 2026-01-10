```yaml
number: 22062
title: "[ty] Avoid storing invalid multi-inference attempts"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/failed-multi-inference
created_at: 2025-12-18T21:20:51Z
updated_at: 2025-12-19T15:38:49Z
url: https://github.com/astral-sh/ruff/pull/22062
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Avoid storing invalid multi-inference attempts

---

_Pull request opened by @ibraheemdev on 2025-12-18 21:20_

## Summary

This should make revealed types a little nicer, as well as avoid confusing the constraint solver in some cases (which were showing up in https://github.com/astral-sh/ruff/pull/21930).

---

_Label `ty` added by @ibraheemdev on 2025-12-18 21:20_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-18 21:22_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 21:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 21:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
graphql-core (https://github.com/graphql-python/graphql-core)
- tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown | str, Unknown | str] & dict[str, Any]`
+ tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & dict[Unknown | str, Unknown | str]`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_downloadermiddleware_cookies.py:363:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | bytes] & dict[Unknown | str, Unknown | bytes]`
+ tests/test_downloadermiddleware_cookies.py:363:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | bytes]`
- tests/test_downloadermiddleware_cookies.py:368:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | bytes] & dict[Unknown | str, Unknown | bytes]`
+ tests/test_downloadermiddleware_cookies.py:368:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | bytes]`
- tests/test_downloadermiddleware_cookies.py:438:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | bool] & dict[Unknown | str, Unknown | bool]`
+ tests/test_downloadermiddleware_cookies.py:438:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | bool]`
- tests/test_downloadermiddleware_cookies.py:443:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | float] & dict[Unknown | str, Unknown | float]`
+ tests/test_downloadermiddleware_cookies.py:443:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | float]`
- tests/test_downloadermiddleware_cookies.py:448:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | int] & dict[Unknown | str, Unknown | int]`
+ tests/test_downloadermiddleware_cookies.py:448:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | int]`
- tests/test_http_request.py:667:58: error[invalid-argument-type] Argument to bound method `from_response` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[str, Iterable[str] | None] & dict[Unknown | str, Unknown | None]`
+ tests/test_http_request.py:667:58: error[invalid-argument-type] Argument to bound method `from_response` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | str, Unknown | None]`
- tests/test_utils_request.py:377:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `list[VerboseCookie | dict[Unknown | str, Unknown | str]] & list[Unknown | dict[Unknown | str, Unknown | str]]`
+ tests/test_utils_request.py:377:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/proxy/layers/http/test_http.py:84:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:84:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:92:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:92:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:98:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:98:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:154:34: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:154:34: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:198:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:198:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:212:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:212:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:272:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:272:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:322:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:322:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:846:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:846:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:965:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:965:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:1029:37: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:1029:37: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:1296:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:1296:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:1749:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:1749:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:1753:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:1753:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__raw_layers.py:105:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__raw_layers.py:105:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__raw_layers.py:136:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__raw_layers.py:136:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__raw_layers.py:157:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__raw_layers.py:157:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__raw_layers.py:165:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__raw_layers.py:165:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__raw_layers.py:217:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__raw_layers.py:217:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__raw_layers.py:239:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__raw_layers.py:239:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__raw_layers.py:255:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__raw_layers.py:255:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:480:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:480:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:555:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:555:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:610:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:610:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:615:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:615:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:714:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:714:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:716:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:716:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:778:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:778:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:786:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:786:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:800:45: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:800:45: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:804:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:804:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:851:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:886:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:851:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:886:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:888:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:888:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:946:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:946:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:948:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:948:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:987:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:987:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:997:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:997:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1008:40: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1008:40: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1022:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1022:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1030:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `(QuicTlsData & Unknown) | (_Placeholder[QuicTlsData] & Unknown) | (QuicTlsData & _Placeholder[Unknown]) | (_Placeholder[QuicTlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1030:36: error[invalid-argument-type] Argument is incorrect: Expected `QuicTlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1064:40: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1064:40: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1078:40: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1078:40: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1102:40: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1102:40: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_modes.py:100:31: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_modes.py:100:31: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_modes.py:111:31: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_modes.py:111:31: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_modes.py:308:35: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_modes.py:308:35: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_modes.py:427:38: error[invalid-argument-type] Argument is incorrect: Expected `Socks5AuthData`, found `(Socks5AuthData & Unknown) | (_Placeholder[Socks5AuthData] & Unknown) | (Socks5AuthData & _Placeholder[Unknown]) | (_Placeholder[Socks5AuthData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_modes.py:427:38: error[invalid-argument-type] Argument is incorrect: Expected `Socks5AuthData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:301:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:301:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:324:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:324:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:353:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:353:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:357:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:357:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:405:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:405:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:433:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:433:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:471:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:471:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:548:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:548:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:550:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:550:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:591:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:591:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:599:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:599:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:613:45: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:613:45: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:616:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:616:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:659:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:659:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:718:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:718:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:720:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:720:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:761:44: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:761:44: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:768:43: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:768:43: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:778:44: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:778:44: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:819:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `(ClientHelloData & Unknown) | (_Placeholder[ClientHelloData] & Unknown) | (ClientHelloData & _Placeholder[Unknown]) | (_Placeholder[ClientHelloData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:819:39: error[invalid-argument-type] Argument is incorrect: Expected `ClientHelloData`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:821:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `(TlsData & Unknown) | (_Placeholder[TlsData] & Unknown) | (TlsData & _Placeholder[Unknown]) | (_Placeholder[TlsData] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/test_tls.py:821:39: error[invalid-argument-type] Argument is incorrect: Expected `TlsData`, found `Unknown | _Placeholder[Unknown]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/backend/ninjabackend.py:2131:67: error[invalid-argument-type] Argument to bound method `get_link_whole_for` is incorrect: Expected `list[str]`, found `list[str | None] & list[Unknown | str | None]`
+ mesonbuild/backend/ninjabackend.py:2131:67: error[invalid-argument-type] Argument to bound method `get_link_whole_for` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[Unknown | tuple[str, int, Unknown]] & list[tuple[str, str, str] | tuple[str, int, Unknown]]`
+ ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[Unknown | tuple[str, int, Unknown]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1221:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5080 diagnostics
+ Found 5081 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any], generic[object]]`
- Found 1839 diagnostics
+ Found 1838 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/home_connect/switch.py:286:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | None`, found `dict[str, str | UndefinedType] & dict[Unknown | str, Unknown | str | UndefinedType]`
+ homeassistant/components/home_connect/switch.py:286:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | None`, found `dict[Unknown | str, Unknown | str | UndefinedType]`
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14414 diagnostics
+ Found 14415 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 21:28_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 0 | 81 |
| `invalid-return-type` | 0 | 0 | 2 |
| `type-assertion-failure` | 0 | 0 | 2 |
| **Total** | **0** | **0** | **85** |

**[Full report with detailed diff](https://ibraheem-failed-multi-infere.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-failed-multi-infere.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @ibraheemdev on 2025-12-18 21:28_

---

_Review requested from @carljm by @ibraheemdev on 2025-12-18 21:28_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-18 21:28_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-18 21:28_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-18 21:28_

---

_Comment by @codspeed-hq[bot] on 2025-12-18 21:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ffailed-multi-inference?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22062 will **degrade performances by 5.31%**

<sub>Comparing <code>ibraheem/failed-multi-inference</code> (6c2eba8) with <code>main</code> (fa57253)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>main</code> (fa57253) during the generation of this report, so fa57253 was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`⚡ 2` improvements  
`❌ 1` regression  
`✅ 19` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ffailed-multi-inference?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ffailed-multi-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 105.3 s | 111.2 s | -5.31% |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ffailed-multi-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.3 s | +4% |
| ⚡ | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ffailed-multi-inference?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.4 s | 1.3 s | +6.91% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ffailed-multi-inference?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@AlexWaygood approved on 2025-12-19 14:08_

That's a pretty huge improvement in the types we're displaying in our diagnostics 😃

---

_Merged by @ibraheemdev on 2025-12-19 15:38_

---

_Closed by @ibraheemdev on 2025-12-19 15:38_

---

_Branch deleted on 2025-12-19 15:38_

---
