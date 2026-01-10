```yaml
number: 22055
title: "[ty] ensure `IntersectionBuilder` does not have multiple identical `InnerIntersectionBuilder`s"
type: pull_request
state: closed
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: intersection-hash
created_at: 2025-12-18T17:48:43Z
updated_at: 2025-12-19T15:59:15Z
url: https://github.com/astral-sh/ruff/pull/22055
synced_at: 2026-01-10T16:42:11Z
```

# [ty] ensure `IntersectionBuilder` does not have multiple identical `InnerIntersectionBuilder`s

---

_Pull request opened by @mtshiba on 2025-12-18 17:48_

## Summary

From #17371

See [the comments in #17371](https://github.com/astral-sh/ruff/pull/17371#issuecomment-3671180138) for the motivation for this change.

## Test Plan

N/A


---

_Label `ty` added by @mtshiba on 2025-12-18 17:48_

---

_Label `internal` added by @mtshiba on 2025-12-18 17:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:51_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5084 diagnostics
+ Found 5085 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-18 17:53_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:59_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 2 | 2 |
| `invalid-return-type` | 0 | 0 | 3 |
| **Total** | **0** | **2** | **5** |




---

_Comment by @codspeed-hq[bot] on 2025-12-18 18:07_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aintersection-hash?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22055 will **not alter performance**

<sub>Comparing <code>mtshiba:intersection-hash</code> (d529737) with <code>main</code> (76854fd)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aintersection-hash?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @mtshiba on 2025-12-18 18:15_

---

_Review requested from @carljm by @mtshiba on 2025-12-18 18:15_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-18 18:15_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-18 18:15_

---

_Review requested from @dcreager by @mtshiba on 2025-12-18 18:15_

---

_@MichaReiser reviewed on 2025-12-18 18:16_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:674 on 2025-12-18 18:16_

It seems risky to rely solely on the hash value, given the possibility of hash collisions.

---

_Comment by @AlexWaygood on 2025-12-18 18:26_

It looks like pydantic has a huge performance improvement here, but most other benchmarks regress: https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aintersection-hash?utm_source=github&utm_medium=comment&utm_content=header. I'd be interested in seeing if we still see the huge performance improvement on pydantic after https://github.com/astral-sh/ruff/pull/22044 has been merged.

---

_Comment by @AlexWaygood on 2025-12-18 21:41_

Can you rebase on `main` now https://github.com/astral-sh/ruff/pull/22044 has landed?

---

_Label `ecosystem-analyzer` removed by @mtshiba on 2025-12-19 00:40_

---

_Label `internal` removed by @mtshiba on 2025-12-19 00:40_

---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-19 00:41_

---

_@carljm reviewed on 2025-12-19 00:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:674 on 2025-12-19 00:43_

Especially since my understanding is that `FxHasher` is optimized for speed, not for collision-resistance?

---

_Comment by @mtshiba on 2025-12-19 01:46_

> I'd be interested in seeing if we still see the huge performance improvement on pydantic after #22044 has been merged.

Hmm, it seems that the performance improvement on pydantic has disappeared after merging #22044.
However, this PR still seems necessary in some points.

* ~~The mypy_primer results show that this change improves boundness analysis (narrowing).~~
* The original goal of this PR, which was to reduce the abnormal memory consumption in #17371, seems to be impossible to achieve without this PR.


---

_Label `ecosystem-analyzer` removed by @mtshiba on 2025-12-19 02:09_

---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-19 02:10_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/builder.rs`:674 on 2025-12-19 02:17_

If the hash function were ideal, the probability of collisions would be almost negligible in this case (where the number of elements is only about 100).
The collision probability is calculated using the same formula as the birthday paradox. If the hash space is $H$ and the number of elements is $N$, then:

$$
P(N) \simeq 1-\exp(-\frac{N(N-1)}{2H}) \simeq \frac{N^2}{2H}
$$

Since $H = 2^{64} \simeq 10^{19}, N^2 \simeq 10^4$, the probability is almost negligible.

The problem is that fxhash is not an ideal hash function. In this case, the actual effective hash space may be much smaller.

---

_@mtshiba reviewed on 2025-12-19 02:17_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/builder.rs`:674 on 2025-12-19 03:10_

Using a cryptographic hash function seems too expensive, so it's better to simply use an equality check to remove duplicates.

---

_@mtshiba reviewed on 2025-12-19 03:10_

---

_Label `ecosystem-analyzer` removed by @mtshiba on 2025-12-19 03:14_

---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-19 03:14_

---

_@MichaReiser reviewed on 2025-12-19 06:12_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:674 on 2025-12-19 06:12_

Could intersections be a FxIndexMap ir is the issue that we keep updating the entries?

---

_@mtshiba reviewed on 2025-12-19 09:49_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/builder.rs`:674 on 2025-12-19 09:49_

We can't use hashmaps because we're using `intersections` as a mutable iterator.

---

_Comment by @AlexWaygood on 2025-12-19 14:21_

This change no longer appears to have significant standalone benefits, so I'm sort-of inclined to say it should again just be part of https://github.com/astral-sh/ruff/pull/17371 rather than being a standalone PR

---

_Closed by @mtshiba on 2025-12-19 15:56_

---

_Branch deleted on 2025-12-19 15:59_

---
