```yaml
number: 19508
title: "[ty] Exhaustiveness checking & reachability for `match` statements"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/match-exhaustiveness
created_at: 2025-07-23T13:00:28Z
updated_at: 2025-07-23T20:48:21Z
url: https://github.com/astral-sh/ruff/pull/19508
synced_at: 2026-01-12T15:56:41Z
```

# [ty] Exhaustiveness checking & reachability for `match` statements

---

_@sharkdp_

## Summary

Implements proper reachability analysis and — in effect — exhaustiveness checking for `match` statements. This allows us to check the following code without any errors (leads to *"can implicitly return `None`"* on `main`):

```py
from enum import Enum, auto

class Color(Enum):
    RED = auto()
    GREEN = auto()
    BLUE = auto()

def hex(color: Color) -> str:
    match color:
        case Color.RED:
            return "#ff0000"
        case Color.GREEN:
            return "#00ff00"
        case Color.BLUE:
            return "#0000ff"
```

Note that code like this already worked fine if there was a `assert_never(color)` statement in a catch-all case, because we would then consider that `assert_never` call terminal. But now this also works without the wildcard case. Adding a member to the enum would still lead to an error here, if that case would not be handled in `hex`.

What needed to happen to support this is a new way of evaluating match pattern constraints. Previously, we would simply compare the type of the subject expression against the patterns. For the last case here, the subject type would still be `Color` and the value type would be `Literal[Color.BLUE]`, so we would infer an ambiguous truthiness.

Now, before we compare the subject type against the pattern, we first generate a union type that corresponds to the set of all values that would have *definitely been matched* by previous patterns. Then, we build a "narrowed" subject type by computing `subject_type & ~already_matched_type`, and compare *that* against the pattern type. For the example here, `already_matched_type = Literal[Color.RED] | Literal[Color.GREEN]`, and so we have a narrowed subject type of `Color & ~(Literal[Color.RED] | Literal[Color.GREEN]) = Literal[Color.BLUE]`, which allows us to infer a reachability of `AlwaysTrue`.

<details>

<summary>A note on negated reachability constraints</summary>

It might seem that we now perform duplicate work, because we also record *negated* reachability constraints. But that is still important for cases like the following (and possibly also for more realistic scenarios):

```py
from typing import Literal

def _(x: int | str):
    match x:
        case None:
            pass # never reachable
        case _:
            y = 1

    y
```

</details>

closes https://github.com/astral-sh/ty/issues/99

## Test Plan

* I verified that this solves all examples from the linked ticket (the first example needs a PEP 695 type alias, because we don't support legacy type aliases yet)
* Verified that the ecosystem changes are all because of removed false positives
* Updated tests

---

_Label `ty` added by @sharkdp on 2025-07-23 13:00_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-23 13:00_

---

_Renamed from "[ty] Exhaustiveness checking / reachability modeling for `match` statements" to "[ty] Exhaustiveness checking & reachability for `match` statements" by @sharkdp on 2025-07-23 13:00_

---

_Comment by @github-actions[bot] on 2025-07-23 13:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/commands/mallocng.py:345:55: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `str`
- Found 2246 diagnostics
+ Found 2245 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/contentviews/_utils.py:50:13: error[type-assertion-failure] Argument does not have asserted type `Never`
- Found 1816 diagnostics
+ Found 1815 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/externalapis/cryptocompare.py:400:19: warning[possibly-unresolved-reference] Name `method` used when possibly not defined
- rotkehlchen/externalapis/cryptocompare.py:406:19: warning[possibly-unresolved-reference] Name `method` used when possibly not defined
- Found 1571 diagnostics
+ Found 1569 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-23 13:09_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-unresolved-reference` | 0 | 2 | 0 |
| `invalid-return-type` | 0 | 1 | 0 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **0** | **4** | **0** |

**[Full report with detailed diff](https://david-match-exhaustiveness.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-23 14:22_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-23 14:22_

---

_@sharkdp reviewed on 2025-07-23 16:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/predicate.rs`:155 on 2025-07-23 16:55_

So there are probably more efficient ways to implement this, but this linked-list approach was so unintrusive..

---

_Marked ready for review by @sharkdp on 2025-07-23 17:15_

---

_Review requested from @carljm by @sharkdp on 2025-07-23 17:15_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-23 17:15_

---

_Review requested from @dcreager by @sharkdp on 2025-07-23 17:15_

---

_@carljm approved on 2025-07-23 20:37_

This is awesome!

---

_Merged by @sharkdp on 2025-07-23 20:45_

---

_Closed by @sharkdp on 2025-07-23 20:45_

---

_Branch deleted on 2025-07-23 20:45_

---

_Comment by @charliermarsh on 2025-07-23 20:48_

Pumped for this!

---
