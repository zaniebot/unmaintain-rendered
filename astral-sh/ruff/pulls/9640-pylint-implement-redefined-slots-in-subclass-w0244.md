```yaml
number: 9640
title: "[`pylint`] Implement `redefined-slots-in-subclass` (`W0244`)"
type: pull_request
state: merged
author: tsugumi-sys
labels:
  - rule
  - accepted
  - preview
assignees: []
merged: true
base: main
head: impl-redefined-slots-in-subclass-W0244
created_at: 2024-01-25T10:25:23Z
updated_at: 2025-01-17T15:54:16Z
url: https://github.com/astral-sh/ruff/pull/9640
synced_at: 2026-01-10T20:34:00Z
```

# [`pylint`] Implement `redefined-slots-in-subclass` (`W0244`)

---

_Pull request opened by @tsugumi-sys on 2024-01-25 10:25_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

- Implementation of [redefined-slots-in-subclass / W0244](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/redefined-slots-in-subclass.html).
- Related to #970

## Test Plan

<!-- How was it tested? -->

```
cargo test
```


---

_Converted to draft by @tsugumi-sys on 2024-01-25 10:33_

---

_Comment by @github-actions[bot] on 2024-01-25 11:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @tsugumi-sys on 2024-01-25 11:05_

---

_@MichaReiser had review dismissed on 2024-03-06 15:38_

Hy @tsugumi-sys. Thanks for working on this rule. It seems that the code currently fails to compile. Would you mind taking a look why that is?

---

_Review requested from @MichaReiser by @tsugumi-sys on 2024-03-07 06:11_

---

_Label `rule` added by @MichaReiser on 2024-03-07 08:08_

---

_Comment by @tsugumi-sys on 2024-03-10 13:27_

@MichaReiser 
I fixed the compile error :)

---

_Label `accepted` added by @MichaReiser on 2024-04-05 10:25_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/redefined_slots_in_subclass.py`:6 on 2024-04-29 08:42_

Can you add some more examples for this? Like what would happen if there's another class in the hierarchy?

```
class Grandparent:
    __slots__ = ("a", "b")


class Parent(Grandparent):
    pass


class Child(Parent):
    __slots__ = ("c", "a")
```

Pylint does trigger for the above code but I don't think Ruff would do the same.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/codes.rs`:302 on 2024-04-29 08:42_

All new rules should go in preview.

```suggestion
        (Pylint, "W0244") => (RuleGroup::Preview, rules::pylint::rules::RedefinedSlotsInSubclass),
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/module.rs`:14 on 2024-04-29 08:51_

I think it would be more useful to run this on an individual class otherwise it won't check the classes which are nested in other blocks. For example:

```py
class Random:
    class Parent:
        __slots__ = ("a", "b")

    class Child(Parent):
        __slots__ = ("c", "a")
```

This poses a challenge of getting the base classes but it can be done via the semantic model. You can find an example here:

https://github.com/astral-sh/ruff/blob/b0d6fd7343e503f1ec2917db339fe88639d84911/crates/ruff_python_semantic/src/analyze/class.rs#L77-L95

* There's a `bases` method which returns a slice of all the arguments for a class
* It would be useful to use `map_subscript` as mentioned as it will allow you to go to a generic class like `Parent[T]`

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/redefined_slots_in_subclass.rs`:129 on 2024-04-29 08:54_

As mentioned earlier, I think we'd need to update this method to consider individual class and climb up the hierarchy to determine if there are any conflicts with any of the names in `__slots__`.

---

_@dhruvmanila requested changes on 2024-04-29 08:55_

---

_Label `preview` added by @dhruvmanila on 2024-04-29 08:55_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-10-04 05:44_

---

_Comment by @dylwil3 on 2024-12-17 05:21_

@tsugumi-sys I've got your branch up-to-date with main (with as few changes as possible). I also moved the proposed rule to preview.

Would you be interested in revisiting this PR and addressing dhruv's comments? I'd be happy to help if you run into any trouble. Let me know either way, and thanks for implementing this rule!

---

_Comment by @tsugumi-sys on 2024-12-17 06:00_

@dylwil3 
I'm sorry for not getting around to this PR for so long.
I'd like to challenge this PR again :)

---

_Comment by @dylwil3 on 2024-12-17 06:07_

> I'm sorry for not getting around to this PR for so long.

No need to apologize! Just wanted to check in, not scold 😄 Glad to hear you're interested in revisiting.

Feel free to ping me with any questions or when you're ready for a review, and thanks again!

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-01-16 20:05_

---

_Comment by @dylwil3 on 2025-01-16 20:12_

Hey @tsugumi-sys , since there weren't too many changes to make I went ahead and made them. Assuming @dhruvmanila agrees that this fixes the previous concerns, we can get this merged in. Thanks so much for your contribution!

---

_Comment by @tsugumi-sys on 2025-01-17 03:02_

@dylwil3  
Hi, I'm so sorry but I'm extremely busy recently, and it would be helpful if you take over this PR :)

---

_@dhruvmanila reviewed on 2025-01-17 04:56_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/redefined_slots_in_subclass.rs`:124 on 2025-01-17 04:56_

Unrelated to this PR, this is interesting as it seems like a confusing behavior given the name of the function (`any_super_class`).

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/redefined_slots_in_subclass.rs`:127 on 2025-01-17 04:56_

```suggestion
		slots_members(&super_class.body).contains(slot)
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/redefined_slots_in_subclass.rs`:49 on 2025-01-17 05:00_

Not sure if we need the brackets, you'll also need to update the snapshots
```suggestion
        format!("Redefined slots '{name}' in subclass")
```

Additionally, I think we can improve the message by including the name of the subclass in which Ruff found the original definition. This can be done in a follow-up PR as I think it'll involve either updating `any_super_class` or adding a `iter_super_class` function which iterates over all the classes.

---

_@dhruvmanila approved on 2025-01-17 05:01_

Thank you! This looks good

---

_@dylwil3 reviewed on 2025-01-17 15:25_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/redefined_slots_in_subclass.rs`:124 on 2025-01-17 15:25_

It's confusing but I think technically correct since any class is a superclass of itself

```console
>>> class Foo:...
...
>>> issubclass(Foo,Foo)
True
```

I like your idea of implementing `iter_super_class`. Then we can leave `any_super_class` as is, but use `iter_super_class` with a `skip` to get this behavior.

---

_Comment by @dylwil3 on 2025-01-17 15:47_

@tsugumi-sys no worries at all! you did the bulk of the work, so not much left on my end. thank you!

---

_Merged by @dylwil3 on 2025-01-17 15:54_

---

_Closed by @dylwil3 on 2025-01-17 15:54_

---
