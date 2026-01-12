```yaml
number: 17809
title: "[red-knot] Synthesize a `__call__` attribute for Callable types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/callable-dunder-call
created_at: 2025-05-03T11:59:40Z
updated_at: 2025-05-03T15:43:20Z
url: https://github.com/astral-sh/ruff/pull/17809
synced_at: 2026-01-12T15:56:06Z
```

# [red-knot] Synthesize a `__call__` attribute for Callable types

---

_@AlexWaygood_

## Summary

Currently this assertion fails on `main`, because we do not synthesize a `__call__` attribute for Callable types:

```py
from typing import Protocol, Callable
from knot_extensions import static_assert, is_assignable_to

class Foo(Protocol):
    def __call__(self, x: int, /) -> str: ...

static_assert(is_assignable_to(Callable[[int], str], Foo))
```

This PR fixes that.

See previous discussion about this in https://github.com/astral-sh/ruff/pull/16493#discussion_r1985098508 and https://github.com/astral-sh/ruff/pull/17682#issuecomment-2839527750

## Test Plan

Existing mdtests updated; a couple of new ones added.


---

_Label `red-knot` added by @AlexWaygood on 2025-05-03 11:59_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-03 11:59_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-03 11:59_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-03 11:59_

---

_Renamed from "synthesize a `__call__` attribute for Callable types" to "[red-knot] Synthesize a `__call__` attribute for Callable types" by @AlexWaygood on 2025-05-03 11:59_

---

_Comment by @github-actions[bot] on 2025-05-03 12:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
jinja (https://github.com/pallets/jinja)
- error[lint:unresolved-attribute] src/jinja2/runtime.py:280:35: Type `(...) -> Any` has no attribute `__call__`
- error[lint:unresolved-attribute] src/jinja2/runtime.py:282:21: Type `(...) -> Any` has no attribute `__call__`
- Found 396 diagnostics
+ Found 394 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[lint:invalid-return-type] tests/conftest.py:697:12: Return type does not match returned value: Expected `SetProjectContext`, found `(...) -> Unknown`
- Found 1153 diagnostics
+ Found 1152 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[lint:unresolved-attribute] scrapy/utils/python.py:269:39: Type `(...) -> Any` has no attribute `__call__`
- Found 1711 diagnostics
+ Found 1710 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[lint:invalid-argument-type] mesonbuild/rewriter.py:36:91: Argument to this function is incorrect: Expected `_FormatterClass`, found `(str, /) -> HelpFormatter`
- error[lint:invalid-argument-type] mesonbuild/rewriter.py:46:82: Argument to this function is incorrect: Expected `_FormatterClass`, found `(str, /) -> HelpFormatter`
- error[lint:invalid-argument-type] mesonbuild/rewriter.py:55:119: Argument to this function is incorrect: Expected `_FormatterClass`, found `(str, /) -> HelpFormatter`
- error[lint:invalid-argument-type] mesonbuild/rewriter.py:61:109: Argument to this function is incorrect: Expected `_FormatterClass`, found `(str, /) -> HelpFormatter`
- Found 1812 diagnostics
+ Found 1808 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[lint:invalid-argument-type] src/bokeh/resources.py:603:19: Argument to this function is incorrect: Expected `UrlsFn`, found `(components, kind) -> Unknown`
- error[lint:invalid-assignment] src/bokeh/resources.py:617:9: Object of type `(components, kind) -> Unknown` is not assignable to attribute `hashes` of type `HashesFn | None`
- error[lint:invalid-argument-type] src/bokeh/resources.py:637:17: Argument to this function is incorrect: Expected `UrlsFn`, found `(components, kind) -> Unknown`
- Found 1927 diagnostics
+ Found 1924 diagnostics

jax (https://github.com/google/jax)
- error[lint:invalid-argument-type] jax/_src/ad_checkpoint.py:816:32: Argument to this function is incorrect: Expected `LoweringRule`, found `(ctx, x, *, name) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/cudnn/fused_attention_stablehlo.py:1086:25: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/cudnn/fused_attention_stablehlo.py:1093:25: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/cudnn/fused_attention_stablehlo.py:1673:25: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/cudnn/fused_attention_stablehlo.py:1680:25: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/cudnn/scaled_matmul_stablehlo.py:304:5: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/custom_batching.py:355:39: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/custom_dce.py:442:19: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/custom_derivatives.py:952:49: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/custom_derivatives.py:1497:39: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/custom_derivatives.py:1805:37: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/custom_transpose.py:243:5: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/error_check.py:272:5: Argument to this function is incorrect: Expected `_SerializeAuxData`, found `(x) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/error_check.py:275:5: Argument to this function is incorrect: Expected `_DeserializeAuxData`, found `(x) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/export/_export.py:504:5: Argument to this function is incorrect: Expected `_DeserializeAuxData`, found `(b) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/interpreters/mlir.py:2559:44: Argument to this function is incorrect: Expected `LoweringRule`, found `(ctx, x) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/interpreters/mlir.py:3058:33: Argument to this function is incorrect: Expected `LoweringRule`, found `(ctx, *x, *, axes, axis_index_groups) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/control_flow/for_loop.py:278:31: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/control_flow/loops.py:1392:24: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/control_flow/solves.py:493:21: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/lax.py:4193:24: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/lax.py:4351:24: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/lax.py:4970:37: Argument to this function is incorrect: Expected `LoweringRule`, found `(_, x, **__) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/lax.py:5005:39: Argument to this function is incorrect: Expected `LoweringRule`, found `(_, x, **__) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/lax.py:6309:24: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/lax.py:8457:32: Argument to this function is incorrect: Expected `LoweringRule`, found `(ctx, x) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/lax.py:8875:31: Argument to this function is incorrect: Expected `LoweringRule`, found `(ctx, x, y) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/linalg.py:907:5: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/linalg.py:1260:13: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/linalg.py:1550:30: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/linalg.py:1690:5: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/linalg.py:1883:30: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/linalg.py:2304:5: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/linalg.py:2668:45: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/special.py:661:3: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/special.py:682:34: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/special.py:687:24: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/special.py:694:24: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/special.py:704:24: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/windowed_reductions.py:817:50: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/windowed_reductions.py:821:50: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/windowed_reductions.py:825:50: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/lax/windowed_reductions.py:1066:49: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/pallas/fuser/fusible.py:80:35: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/pallas/primitives.py:345:42: Argument to this function is incorrect: Expected `LoweringRule`, found `(_, x, **__) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/pallas/primitives.py:359:39: Argument to this function is incorrect: Expected `LoweringRule`, found `(_, x, **__) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/prng.py:960:21: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/random.py:1302:40: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/random.py:1305:40: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/random.py:2738:40: Argument to this function is incorrect: Expected `LoweringRule`, found `(_, k) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/state/discharge.py:607:37: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/_src/state/primitives.py:763:21: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/key_reuse/_core.py:243:5: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/key_reuse/_core.py:256:5: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/api.py:114:35: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:233:40: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:362:42: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:495:40: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:599:42: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:923:44: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:928:27: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:931:27: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:1085:5: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:1293:46: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:1378:45: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcoo.py:1564:47: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcsr.py:303:42: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcsr.py:380:40: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcsr.py:454:40: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/bcsr.py:790:25: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/coo.py:258:39: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/coo.py:371:41: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/coo.py:495:38: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/coo.py:615:38: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/csr.py:281:39: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/csr.py:401:41: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/csr.py:507:38: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/csr.py:614:38: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- error[lint:invalid-argument-type] jax/experimental/sparse/nm.py:243:35: Argument to this function is incorrect: Expected `LoweringRule`, found `(...) -> Unknown`
- Found 5566 diagnostics
+ Found 5487 diagnostics

zulip (https://github.com/zulip/zulip)
- error[lint:invalid-argument-type] zerver/tests/test_decorators.py:128:25: Argument to this function is incorrect: Expected `_GetResponseCallable`, found `(request) -> Unknown`
- error[lint:invalid-argument-type] zerver/tests/test_decorators.py:1609:25: Argument to this function is incorrect: Expected `_GetResponseCallable`, found `(request) -> Unknown`
- error[lint:invalid-argument-type] zerver/tests/test_middleware.py:247:25: Argument to this function is incorrect: Expected `_GetResponseCallable`, found `(_) -> Unknown`
- error[lint:invalid-argument-type] zerver/tests/test_middleware.py:256:25: Argument to this function is incorrect: Expected `_GetResponseCallable`, found `(_) -> Unknown`
- error[lint:invalid-argument-type] zerver/tests/test_middleware.py:265:25: Argument to this function is incorrect: Expected `_GetResponseCallable`, found `(_) -> Unknown`
- Found 5926 diagnostics
+ Found 5921 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-05-03 12:16_

Primer results all look as expected! Here's what the `jax` hits look like: `LoweringRule` is a callback protocol defined here:
- https://github.com/jax-ml/jax/blob/43b784c3c7b961134bbdb2fdaa56a168540926cc/jax/_src/interpreters/mlir.py#L838-L843
- The `register_lowering` function has a parameter annotated as accepting an instance of `LoweringRule` here: https://github.com/jax-ml/jax/blob/43b784c3c7b961134bbdb2fdaa56a168540926cc/jax/_src/interpreters/mlir.py#L851-L852
- And `jax` usually passes `lambda` functions into that parameter at callsites. We currently just infer a plain `Callable` type for `lambda`s, so we previously complained about that, but we now understand that the lambda has a type consistent with the callback protocol: https://github.com/jax-ml/jax/blob/43b784c3c7b961134bbdb2fdaa56a168540926cc/jax/_src/ad_checkpoint.py#L816

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:1491 on 2025-05-03 15:38_

It's obviously fine to leave these as TODO for now -- but I'm not sure I understand where our implementation is lacking that makes them not pass yet?

---

_@carljm approved on 2025-05-03 15:38_

---

_@AlexWaygood reviewed on 2025-05-03 15:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:1491 on 2025-05-03 15:42_

We still don't consider the types of protocol members at all when doing assignability or subtyping checks between any type `T` (a protocol type or otherwise) and a protocol type `P`. We still only check whether `T` _has_ the members specified by `P`; we do not yet check whether the types of the members on `T` are correct. This leads to many false negatives, such as the ones that are TODO'd here.

I'm working on it! Still not totally sure what the best design is when it comes to property members and method members, though. And want to add more tests first.

---

_Merged by @AlexWaygood on 2025-05-03 15:43_

---

_Closed by @AlexWaygood on 2025-05-03 15:43_

---

_Branch deleted on 2025-05-03 15:43_

---
