```yaml
number: 10317
title: "[`pylint`] Implement `class-variable-slots-conflict` (`PLE0242`)"
type: pull_request
state: open
author: chanman3388
labels: []
assignees: []
base: main
head: PLE0242
created_at: 2024-03-09T18:28:57Z
updated_at: 2024-04-25T14:14:53Z
url: https://github.com/astral-sh/ruff/pull/10317
synced_at: 2026-01-12T15:55:31Z
```

# [`pylint`] Implement `class-variable-slots-conflict` (`PLE0242`)

---

_@chanman3388_

## Summary

Add the pylint rule `class-variable-slots-conflict`.

## Test Plan

New test fixtures added

Part of #970 


---

_Comment by @chanman3388 on 2024-03-09 18:31_

This case:
```python
items = ["a"]  # TODO


class NotOk:
    __slots__ = items
    a = None
```
pylint does catch, is this something we can handle by using the deferred scopes?

---

_Comment by @github-actions[bot] on 2024-03-09 18:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-03-28 16:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/class_variable_slots_conflict.rs`:18 on 2024-03-28 16:58_

Would you mind completing the *Why is this bad* section. It would help me understand the purpose of the rule and triaging it. 

---

_@MichaReiser requested changes on 2024-03-28 16:59_

Thank's for implementing the new rule. I'm trying to understand the rule, but I couldn't find any useful explanation. 
Would you mind outlining why this specific pattern is bad or linking to some resources? The wording doesn't need to be final, but it would help me understand and triage the rule.

---

_Comment by @chanman3388 on 2024-03-28 17:33_

> Thank's for implementing the new rule. I'm trying to understand the rule, but I couldn't find any useful explanation. 
> Would you mind outlining why this specific pattern is bad or linking to some resources? The wording doesn't need to be final, but it would help me understand and triage the rule.

Hi Michael, thanks for taking a look, yes of course sorry about that. I'll add it when I next get some time.

---

_Comment by @chanman3388 on 2024-03-28 19:58_

> Thank's for implementing the new rule. I'm trying to understand the rule, but I couldn't find any useful explanation. 
> Would you mind outlining why this specific pattern is bad or linking to some resources? The wording doesn't need to be final, but it would help me understand and triage the rule.

Not that this excuses me missing it out in the docstring but I did link to this page [here](https://docs.python.org/3/reference/datamodel.html#slots). I have since found a [page](https://wiki.python.org/moin/UsingSlots) which explains the feature more fully so I'll link that and add a summary at the top level.

---

_Review requested from @MichaReiser by @MichaReiser on 2024-04-08 08:21_

---

_Comment by @chanman3388 on 2024-04-25 13:01_

> This case:
> 
> ```python
> items = ["a"]  # TODO
> 
> 
> class NotOk:
>     __slots__ = items
>     a = None
> ```
> 
> pylint does catch, is this something we can handle by using the deferred scopes?

I realised that this could well be handled by performing the lint at the module level instead, would this be sensible?

---

_@VascoSch92 reviewed on 2024-04-25 13:47_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/resources/test/fixtures/pylint/class_variable_slots_conflict.py`:1 on 2024-04-25 13:47_

Just a small question: what happens with dataclasses? For example
```python
@dataclass
class Test:
    __slots__ = ['a']
    a: int
```
is this correct or?

---

_@chanman3388 reviewed on 2024-04-25 14:00_

---

_Review comment by @chanman3388 on `crates/ruff_linter/resources/test/fixtures/pylint/class_variable_slots_conflict.py`:1 on 2024-04-25 14:00_

This is fine according to pylint.
Also: https://docs.python.org/3/library/dataclasses.html

So what you've written is fine, but:
```python
@dataclass(slots=True)
class Test:
    __slots__ = ["a"]
    a: int
```
will raise a `TypeError`.

---

_@VascoSch92 reviewed on 2024-04-25 14:14_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/resources/test/fixtures/pylint/class_variable_slots_conflict.py`:1 on 2024-04-25 14:14_

thanks for the answer

---
