```yaml
number: 21130
title: "[ty] Reachability and narrowing for enum methods"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/narrowing-for-enum-methods
created_at: 2025-10-29T20:29:02Z
updated_at: 2025-10-30T14:38:59Z
url: https://github.com/astral-sh/ruff/pull/21130
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Reachability and narrowing for enum methods

---

_Pull request opened by @sharkdp on 2025-10-29 20:29_

## Summary

Adds proper type narrowing and reachability analysis for matching on non-inferable type variables bound to enums. For example:

```py
from enum import Enum

class Answer(Enum):
    NO = 0
    YES = 1

    def is_yes(self) -> bool:  # no error here!
        match self:
            case Answer.YES:
                return True
            case Answer.NO:
                return False
```

closes https://github.com/astral-sh/ty/issues/1404

## Test Plan

Added regression tests


---

_Label `ty` added by @sharkdp on 2025-10-29 20:29_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-29 20:29_

---

_Comment by @github-actions[bot] on 2025-10-29 20:32_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-29 20:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/proxy/layers/http/_events.py:127:17: error[type-assertion-failure] Argument does not have asserted type `Never`
- Found 1890 diagnostics
+ Found 1889 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/mosaic_gpu/core.py:1527:24: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `TMEMLayout`
- Found 2545 diagnostics
+ Found 2544 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-10-30 13:19_

---

_Review requested from @carljm by @sharkdp on 2025-10-30 13:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-30 13:19_

---

_Review requested from @dcreager by @sharkdp on 2025-10-30 13:19_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-30 13:19_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-30 13:19_

---

_@AlexWaygood reviewed on 2025-10-30 13:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/match.md`:304 on 2025-10-30 13:24_

We do indeed

---

_Comment by @github-actions[bot] on 2025-10-30 13:25_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 1 | 0 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **0** | **2** | **0** |

**[Full report with detailed diff](https://david-narrowing-for-enum-met.ecosystem-663.pages.dev/diff)** ([timing results](https://david-narrowing-for-enum-met.ecosystem-663.pages.dev/timing))


---

_@sharkdp reviewed on 2025-10-30 13:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/match.md`:304 on 2025-10-30 13:36_

Glad you like it, I was planning on leaving this in there.

---

_@AlexWaygood reviewed on 2025-10-30 13:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:795 on 2025-10-30 13:44_

nit: it seems to compile fine if we just use references here?

```suggestion
                    .map(|lit| lit.name(self.db))
                    .chain(std::iter::once(enum_literal.name(self.db))
```

---

_@AlexWaygood reviewed on 2025-10-30 13:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:796 on 2025-10-30 13:50_

I think this can be an `FxHashSet`, which is a slighlty simpler datastructure and possibly slightly faster? I don't think deterministic ordering is necessary here

```suggestion
                    .collect::<FxHashSet<_>>();
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:816 on 2025-10-30 13:53_

```suggestion
        // ~Literal[YES]`, which *is* always equivalent to `Never`. This means that subsequent patterns can never
```

---

_@AlexWaygood reviewed on 2025-10-30 14:08_

This fixes the issue for methods on enums, but there appear to be lots of other cases where you can have a TypeVar bound to a union (or type equivalent to a union) where we also exhibit similar issues with reachability and narrowing. These are all on this branch:

```py
from typing import assert_never, Literal

def f[T: bool](x: T) -> T:
    match x:
        case True:
            return x
        case False:
            return x
        case _:
            reveal_type(x) # T@f & ~Literal[True] & ~Literal[False]
            assert_never(x)  # false-positive error

def g[T: Literal["foo", "bar"]](x: T) -> T:
    match x:
        case "foo":
            return x
        case "bar":
            return x
        case _:
            reveal_type(x)  # T@g & ~Literal["foo"] & ~Literal["bar"]
            assert_never(x)  # false-positive error

def h[T: int | str](x: T) -> T:
    if isinstance(x, int):
        return x
    elif isinstance(x, str):
        return x
    else:
        reveal_type(x)  # T@h & ~int & ~str
        assert_never(x)  # false-positive error
```

---

_@AlexWaygood approved on 2025-10-30 14:36_

---

_Merged by @sharkdp on 2025-10-30 14:38_

---

_Closed by @sharkdp on 2025-10-30 14:38_

---

_Branch deleted on 2025-10-30 14:38_

---
