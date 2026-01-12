```yaml
number: 685
title: "Incomplete narrowing with terminal statement inside nested `if`s"
type: issue
state: closed
author: abhijeetbodas2001
labels:
  - narrowing
  - control flow
assignees: []
created_at: 2025-06-19T22:08:27Z
updated_at: 2025-12-11T19:51:43Z
url: https://github.com/astral-sh/ty/issues/685
synced_at: 2026-01-12T15:54:23Z
```

# Incomplete narrowing with terminal statement inside nested `if`s

---

_@abhijeetbodas2001_

### Summary

Consider this example:

```py
def _(val: int | None):
  if val is None:
    if True:
      return

  reveal_type(val) # revealed: int | None, should be just `int`
```

We expect that `val` should be narrowed to just `int` at the end. That is also what `pyright` and `mypy` do.

However, for that to happen, we currently depend on the branch with the `val is None` narrowing constraint _not_ being merged into the main flow.

The decision to not merge is taken only when the body of the outer `if` has a reachability of `ALWAYS_FALSE` _at the time of building the semantic index_, as explained in the code below:

https://github.com/astral-sh/ruff/blob/ce0a32aadb16c9e2aea74013b5f0e105c2737253/crates/ty_python_semantic/src/semantic_index/use_def.rs#L956-L970
```rust
    pub(super) fn merge(&mut self, snapshot: FlowSnapshot) {
        // As an optimization, if we know statically that either of the snapshots is always
        // unreachable, we can leave it out of the merged result entirely. Note that we cannot
        // perform any type inference at this point, so this is largely limited to unreachability
        // via terminal statements. If a flow's reachability depends on an expression in the code,
        // we will include the flow in the merged result; the reachability constraints of its
        // bindings will include this reachability condition, so that later during type inference,
        // we can determine whether any particular binding is non-visible due to unreachability.
        if snapshot.reachability == ScopedReachabilityConstraintId::ALWAYS_FALSE {
            return;
        }
        if self.reachability == ScopedReachabilityConstraintId::ALWAYS_FALSE {
            self.restore(snapshot);
            return;
        }
```


Which basically means, that the body of the outer `if` needs to have a bare terminal statement, for `None` to be removed from `val`'s initial type of `int | None`.

But in the above case, the terminal statement here, `return`, itself has a reachability constraint of `True`. In fact, in place of `True` any expression which statically evaluates to a truthy value will result in incomplete narrowing like above. But because we do not have access to type information at the time of building the semantic index, we cannot decide _not_ to merge for such scenarios.

This issue also affects the `ReturnsNever` constraints that we use for handling calls like `sys.exit()` --  [see this TODO in the tests](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/terminal_statements.md#type-narrowing).

To correctly narrow down `val`'s type during type inference phase, one way could be the following:
During semantic index building, whenever we encounter a terminal statement, before marking the branch as unreachable, take the current reachability, negate it, and add the result as a narrowing constraint to the currently tracked bindings.
That way, during type inference, the inferred type of `val` would be the one which does not allow any terminal statements to be hit.


---

_Renamed from "Incomplete narrowing with terminal statement nested inside multiple `if`s" to "Incomplete narrowing with terminal statement inside nested `if`s" by @abhijeetbodas2001 on 2025-06-19 22:09_

---

_Label `narrowing` added by @carljm on 2025-06-19 22:11_

---

_Label `control flow` added by @sharkdp on 2025-06-23 11:56_

---

_Added to milestone `GA` by @carljm on 2025-07-23 22:55_

---

_Comment by @dedebenui on 2025-11-16 14:30_

I've encountered something similar, but I'm not sure if it stems from the same problem or not. Let me know if I should open another issue.

```python
from typing import reveal_type


def compute_properties(peak_power: float | None = None, energy: float | None = None) -> float:
    if peak_power is None and energy is None:
        raise ValueError("please provide either peak power or energy")
    ... 
    if peak_power is None:
        reveal_type(energy)  # int | float | None
        peak_power = energy / 2

    return peak_power
```

Ty says that `energy` in `energy / 2` could still be `None` and therefore it's not safe to `/ 2`, but logically, `energy` is `not None` in that case. Of course I can rewrite this with nested `if`s, but I prefer the clarity of short-circuiting at the start of the function rather than 2 levels of `if` deep.

---

_Comment by @carljm on 2025-11-17 15:42_

@dedebenui That's a different issue, and it's expected behavior; all other type checkers (mypy, pyright, pyrefly) give the same result as ty there. After the initial two lines we cannot conclude anything about the type of `peak_power` or `energy`, because we don't know which one of them is not-None. After `if peak_power is None` in principle we can know that `energy` must therefore not be `None`, but this requires tracking an implication relationship between the type of `peak_power` and the type of `energy` (or internally rearranging the control flow using a technique called "splitting and merging"), which is a level of control-flow analysis that no Python type checker currently does.

I wouldn't rule out the possibility that someday we might like to tackle this, so you're welcome to file an issue for it, but it's not likely to make it onto the roadmap in the short or medium term.

One alternative workaround to rearranging control flow is just to use `assert energy is not None` to inform the type checker of what you already know to be true.

---

_Comment by @carljm on 2025-12-11 19:51_

Discussed this with @sharkdp this morning and decided that this really is the same issue as #690, so closing as duplicate.

---

_Closed by @carljm on 2025-12-11 19:51_

---
