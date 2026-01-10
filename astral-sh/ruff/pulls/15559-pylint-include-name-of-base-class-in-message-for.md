```yaml
number: 15559
title: "[`pylint`] Include name of base class in message for `redefined-slots-in-subclass` (`W0244`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: slots-followup
created_at: 2025-01-17T22:10:20Z
updated_at: 2025-01-20T06:41:20Z
url: https://github.com/astral-sh/ruff/pull/15559
synced_at: 2026-01-10T20:05:43Z
```

# [`pylint`] Include name of base class in message for `redefined-slots-in-subclass` (`W0244`)

---

_Pull request opened by @dylwil3 on 2025-01-17 22:10_

In the following situation:

```python
class Grandparent:
  __slots__ = "a"

class Parent(Grandparent): ...

class Child(Parent):
  __slots__ = "a"
```

the message for `W0244` now specifies that `a` is overwriting a slot from `Grandparent`.

To implement this, we introduce a helper function `iter_super_classes` which does a breadth-first traversal of the superclasses of a given class (as long as they are defined in the same file, due to the usual limitations of the semantic model).

Note: Python does not allow conflicting slots definitions under multiple inheritance. Unless I'm misunderstanding something, I believe It follows that the subposet of superclasses of a given class that redefine a given slot is in fact totally ordered. There is therefore a unique _nearest_ superclass whose slot is being overwritten. So, you know, in case anyone was super worried about that... you can just chill.

This is a followup to #9640 .


---

_Label `diagnostics` added by @dylwil3 on 2025-01-17 22:10_

---

_Comment by @github-actions[bot] on 2025-01-17 22:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/redefined_slots_in_subclass.rs`:72 on 2025-01-18 04:56_

I think we should change the function name as it's now returning a `Diagnostic` and not a boolean. Maybe `check_super_slots` or something similar?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLW0244_redefined_slots_in_subclass.py.snap`:4 on 2025-01-18 04:57_

nit: "Slot `a` redefined from base class `Base`"

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/analyze/class.rs`:82 on 2025-01-18 05:01_

What's the reason for doing breadth-first? I think the original implementation did depth-first traversal.

---

_@dhruvmanila reviewed on 2025-01-18 05:02_

This looks great, thanks for following up quickly on this!

I just have one question about whether to use depth-first or breath-first traversal otherwise this looks good to go.

---

_@dylwil3 reviewed on 2025-01-18 11:19_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/analyze/class.rs`:82 on 2025-01-18 11:19_

I guess my intuition is that the superclass graph will usually be deeper than it is wide (but maybe that assumption is off), and that the width is often pretty small in any event. Does that seem right to you?

---

_@dylwil3 reviewed on 2025-01-18 15:50_

---

_Review comment by @dylwil3 on `crates/ruff_python_semantic/src/analyze/class.rs`:82 on 2025-01-18 15:50_

To save you the re-review, I'm gonna merge this in. But if you wake up in the middle of the night shouting "depth first!" I will be happy to change it back 😄 

---

_Merged by @dylwil3 on 2025-01-18 15:50_

---

_Closed by @dylwil3 on 2025-01-18 15:50_

---

_@dhruvmanila reviewed on 2025-01-20 06:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/analyze/class.rs`:82 on 2025-01-20 06:41_

Haha, no worries.

> I guess my intuition is that the superclass graph will usually be deeper than it is wide (but maybe that assumption is off), and that the width is often pretty small in any event. Does that seem right to you?

I'm not sure about this but I don't think this should necessarily affect the performance if that's your concern. One minor nit would be the usage of `VecDeque` in which the elements in memory are not guaranteed to be contiguous compared to `Vec`. This also makes the implementation inconsistent with the similar function in the same module `any_base_class`.

That said, we don't need to change anything here right now.

---
