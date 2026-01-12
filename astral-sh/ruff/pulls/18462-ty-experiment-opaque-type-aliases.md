```yaml
number: 18462
title: "[ty] Experiment: Opaque type aliases"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/opaque-type-aliases
created_at: 2025-06-04T14:24:30Z
updated_at: 2025-06-26T12:29:48Z
url: https://github.com/astral-sh/ruff/pull/18462
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Experiment: Opaque type aliases

---

_@sharkdp_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Do not review.

Would currently allows us to move `artigraph` and `home-assistant/core` to `good.txt`

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-06-04 14:24_

---

_Comment by @codspeed-hq[bot] on 2025-06-04 14:33_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fopaque-type-aliases)

### Merging #18462 will **degrade performances by 46.56%**

<sub>Comparing <code>david/opaque-type-aliases</code> (969efae) with <code>main</code> (f1883d7)</sub>



### Summary

`❌ 2` regressions  
`✅ 32` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fopaque-type-aliases)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_check_file[incremental] `` | 5.3 ms | 5.7 ms | -5.65% |
| ❌ | `` ty_micro[many_tuple_assignments] `` | 180.8 ms | 338.3 ms | -46.56% |


---

_Comment by @github-actions[bot] on 2025-06-04 14:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- error[call-non-callable] src/async_utils/corofunc_cache.py:98:30: Object of type `None` is not callable
+ error[call-non-callable] src/async_utils/corofunc_cache.py:98:30: Object of type `CacheTransformer | None` is not callable
- error[call-non-callable] src/async_utils/corofunc_cache.py:177:30: Object of type `None` is not callable
+ error[call-non-callable] src/async_utils/corofunc_cache.py:177:30: Object of type `CacheTransformer | None` is not callable
- error[call-non-callable] src/async_utils/task_cache.py:158:30: Object of type `None` is not callable
+ error[call-non-callable] src/async_utils/task_cache.py:158:30: Object of type `CacheTransformer | None` is not callable
- error[call-non-callable] src/async_utils/task_cache.py:244:30: Object of type `None` is not callable
+ error[call-non-callable] src/async_utils/task_cache.py:244:30: Object of type `CacheTransformer | None` is not callable

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ error[invalid-return-type] mitmproxy/contentviews/_view_image/image_parser.py:34:12: Return type does not match returned value: expected `ImageMetadata`, found `list[Unknown]`
+ error[invalid-return-type] mitmproxy/contentviews/_view_image/image_parser.py:60:12: Return type does not match returned value: expected `ImageMetadata`, found `list[Unknown]`
+ error[invalid-return-type] mitmproxy/contentviews/_view_image/image_parser.py:94:12: Return type does not match returned value: expected `ImageMetadata`, found `list[Unknown]`
+ error[invalid-return-type] mitmproxy/contentviews/_view_image/image_parser.py:119:12: Return type does not match returned value: expected `ImageMetadata`, found `list[Unknown]`
+ error[invalid-assignment] mitmproxy/net/dns/https_records.py:48:9: Object of type `dict[Unknown, Unknown]` is not assignable to `HTTPSRecordJSON`
- error[invalid-argument-type] test/mitmproxy/contentviews/test__utils.py:23:38: Argument to function `make_metadata` is incorrect: Expected `Message | TCPMessage | UDPMessage | WebSocketMessage | DNSMessage`, found `Response | None`
+ error[invalid-argument-type] test/mitmproxy/contentviews/test__utils.py:23:38: Argument to function `make_metadata` is incorrect: Expected `ContentviewMessage`, found `Response | None`
+ error[invalid-argument-type] test/mitmproxy/net/dns/test_https_records.py:126:13: Argument to bound method `from_json` is incorrect: Expected `HTTPSRecordJSON`, found `dict[Unknown, Unknown]`
- Found 2107 diagnostics
+ Found 2113 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ error[invalid-return-type] openlibrary/catalog/add_book/match.py:215:16: Return type does not match returned value: expected `ThresholdResult`, found `tuple[Literal["date"], Literal["value missing"], Unknown | Literal[-800]]`
+ error[invalid-return-type] openlibrary/catalog/add_book/match.py:225:12: Return type does not match returned value: expected `ThresholdResult`, found `tuple[Literal["date"], Literal["mismatch"], Unknown | Literal[-800]]`
+ error[invalid-return-type] openlibrary/catalog/add_book/match.py:234:24: Return type does not match returned value: expected `ThresholdResult`, found `tuple[Literal["ISBN"], Literal["match"], Unknown | Literal[85]]`
+ error[invalid-return-type] openlibrary/catalog/add_book/match.py:298:16: Return type does not match returned value: expected `ThresholdResult`, found `tuple[Literal["authors"], Literal["keyword match"], Unknown & ~AlwaysFalsy]`
- Found 733 diagnostics
+ Found 737 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- warning[possibly-unbound-attribute] sklearn/preprocessing/_polynomial.py:1059:38: Attribute `tolil` on type `csr_array[float64, tuple[int, int]] | float64 | lil_array[float64]` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/preprocessing/_polynomial.py:1071:38: Attribute `tolil` on type `csr_array[float64, tuple[int, int]] | float64 | lil_array[float64] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/preprocessing/_polynomial.py:1099:42: Attribute `tolil` on type `csr_array[float64, tuple[int, int]] | float64 | lil_array[float64]` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/preprocessing/_polynomial.py:1110:42: Attribute `tolil` on type `csr_array[float64, tuple[int, int]] | float64 | lil_array[float64] | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/preprocessing/_polynomial.py:1116:30: Attribute `tocsr` on type `csr_array[float64, tuple[int, int]] | float64 | lil_array[float64] | Unknown` is possibly unbound
- Found 2562 diagnostics
+ Found 2557 diagnostics

```
</details>


---

_Closed by @sharkdp on 2025-06-26 12:29_

---
