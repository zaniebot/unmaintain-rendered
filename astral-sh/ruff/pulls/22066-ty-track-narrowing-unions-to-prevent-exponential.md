```yaml
number: 22066
title: "[ty] Track narrowing unions to prevent exponential blowup"
type: pull_request
state: closed
author: zanieb
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: zb/narrow-track
created_at: 2025-12-19T02:39:28Z
updated_at: 2025-12-20T04:04:00Z
url: https://github.com/astral-sh/ruff/pull/22066
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Track narrowing unions to prevent exponential blowup

---

_Pull request opened by @zanieb on 2025-12-19 02:39_

See https://github.com/astral-sh/ty/issues/2026#issuecomment-3673229602

I'm mostly exploring this as a short-term way to resolve the exponential complexity of recursive analysis as described in the linked issue. It may or may not have longstanding value once we support discriminated (or "tagged") unions for typed dicts.

---

_Label `performance` added by @zanieb on 2025-12-19 02:39_

---

_Label `ty` added by @zanieb on 2025-12-19 02:39_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 02:41_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 02:42_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 43 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2802 diagnostics
+ Found 2805 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5086 diagnostics
+ Found 5085 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-19 02:56_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zb%2Fnarrow-track?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22066 will **improve performance by 72.68%**

<sub>Comparing <code>zb/narrow-track</code> (fd525e8) with <code>main</code> (1a18ada)</sub>



### Summary

`⚡ 2` improvements  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/zb%2Fnarrow-track?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 107.1 s | 100.7 s | +6.35% |
| ⚡ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/zb%2Fnarrow-track?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 105.9 s | 61.3 s | +72.68% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/zb%2Fnarrow-track?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @zanieb on 2025-12-19 17:40_

Hm is this a fix for some false positives?

```
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5085 diagnostics
+ Found 5083 diagnostics
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:7147 on 2025-12-19 21:19_

If you make `narrowing_unions` an `FxOrderMap`, this can become a `pop`, since the nested calls should be stack-like

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:7032 on 2025-12-19 21:27_

nit: let chains!

```suggestion
        let call_expression_tcx = if let Some(Type::Union(union)) = call_expression_tcx.annotation
            && let Some(&narrowed_ty) = self.narrowing_unions.get(&union)
        {
            TypeContext::new(Some(narrowed_ty))
        } else {
            call_expression_tcx
        };
```

(I mildly prefer how that means you don't have to repeat the fallback case)

---

_@dcreager approved on 2025-12-19 21:30_

This looks fine to me, esp with an eye towards landing a quick fix and iterating on that as follow-on work.

@ibraheemdev mentioned in discord:

> I think we need something slightly different than [this] PR though, we should be propagating the narrowed type context through the nested calls instead of the entire union, but not ignoring it completely

It looks like with the latest commit, you are propagating the narrowed _type_ (though not the narrowed _type context_) along with the union. I'm not sure whether that's sufficient, but if not, I'm happy to address that as a follow-on.

---

_@zanieb reviewed on 2025-12-19 21:33_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/types/infer/builder.rs`:7032 on 2025-12-19 21:33_

Strong agree. The AI doesn't know about the power of let-chains yet, I think — too new.

---

_Comment by @zanieb on 2025-12-19 21:35_

Thanks for giving this a look!

> It looks like with the latest commit, you are propagating the narrowed type (though not the narrowed type context) along with the union. I'm not sure whether that's sufficient, but if not, I'm happy to address that as a follow-on.

Ah interesting. I missed the nuance there.

---

_Comment by @dcreager on 2025-12-19 21:41_

> Ah interesting. I missed the nuance there.

It _might_ be the case that the narrowed type context will always end up being the narrowed type — that's the point of the bidi type context after all. But I'm not certain whether there are cases where the narrowed type will still be narrower than the narrowed type context, and if so, whether that's important. I would defer to @ibraheemdev on that question.

---

_Comment by @ibraheemdev on 2025-12-20 04:02_

As discussed on Discord, I don't think this is correct. We already correctly propagate the narrowed type context during inference, this PR essentially argues that if a child call has a union as type context which we are currently narrowing as part of a parent call, the child call will narrow to the same element as the parent. It's very rare to have nested generic calls where this matters in the first place, but that heuristic is not necessarily true, e.g.,

```py
def f(x: list[int | None] | list[str | None]) -> str:
    return ""

def g[T](x: T) -> list[T]:
    return [x]

# regression: error: Expected `list[int | None] | list[str | None]`, found `list[str | None | int]`
f(g(f(g(1))))
```

The performance improvement here seems to be in cases where didn't need to narrow in the first place, which https://github.com/astral-sh/ruff/pull/22102 should address.

---

_Comment by @zanieb on 2025-12-20 04:04_

Thank you!

---

_Closed by @zanieb on 2025-12-20 04:04_

---
