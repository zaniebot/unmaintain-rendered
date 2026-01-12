```yaml
number: 15561
title: "[`red-knot`] No `cyclic-class-def` diagnostics for subclasses of cyclic classes"
type: pull_request
state: merged
author: wooly18
labels:
  - ty
assignees: []
merged: true
base: main
head: cyclic-classes
created_at: 2025-01-17T22:22:24Z
updated_at: 2025-01-20T13:37:04Z
url: https://github.com/astral-sh/ruff/pull/15561
synced_at: 2026-01-12T15:55:51Z
```

# [`red-knot`] No `cyclic-class-def` diagnostics for subclasses of cyclic classes

---

_@wooly18_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #14223 

## Test Plan

`cargo nextest run`


---

_Marked ready for review by @wooly18 on 2025-01-17 22:33_

---

_Review requested from @carljm by @wooly18 on 2025-01-17 22:33_

---

_Review requested from @MichaReiser by @wooly18 on 2025-01-17 22:33_

---

_Review requested from @AlexWaygood by @wooly18 on 2025-01-17 22:33_

---

_Review requested from @sharkdp by @wooly18 on 2025-01-17 22:33_

---

_Comment by @github-actions[bot] on 2025-01-17 22:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @wooly18 on 2025-01-17 22:54_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-17 23:08_

---

_Marked ready for review by @wooly18 on 2025-01-17 23:10_

---

_@carljm reviewed on 2025-01-17 23:27_

This looks great, thank you for the contribution!

The algorithm looks good, my only comments are on naming and clarity. Apologies, this is slightly bike-sheddy. I think the `Option<bool>` return type (and its semantics: `Some` if there is a cycle, else `None`, inner bool is true if the class itself is a participant in the cycle) are too non-obvious. I would prefer an explicit `InheritanceCycle` enum with variants `Inherited` and `Participant` (with doc comments clarifying the meaning of each), and return `Option<InheritanceCycle>` instead. We can define a method like `is_participant()` to make the usage of this enum in the `if` test for the diagnostic a bit more ergonomic.

I'm also not sure that `is_involved_in_cyclic_definition` adds enough clarity to justify its length. I'm guessing you renamed from `is_cyclically_defined` because that seems to imply the definition of this class itself is part of the cycle? I agree with that. I'd suggest maybe just `inheritance_cycle()` as the method name, leading to usages looking like `class.inheritance_cycle().is_some()`, which reads pretty naturally?

@AlexWaygood wrote this code and may also have thoughts, not sure if he'll be able to offer them before Monday, though.

---

_Comment by @wooly18 on 2025-01-18 07:18_

Thanks for the feedback! The name changes sound good to me and I'm happy to make them soon.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:3664 on 2025-01-20 06:20_

```suggestion
    const fn is_participant(self) -> bool {
        matches!(self, InheritanceCycle::Participant)
    }
```

---

_@dhruvmanila reviewed on 2025-01-20 06:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3654 on 2025-01-20 08:49_

Using `///` over `//` has the advantage that the text shows up as documentation. It's otherwise considered an inline comment.

```suggestion
    /// The class is cyclically defined and is a participant in the cycle.
    /// i.e., it inherits either directly or indirectly from itself.
    Participant,
    /// The class inherits from a class that is a `Participant` in an inheritance cycle,
    /// but is not itself a participant.
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4064 on 2025-01-20 08:50_

```suggestion
        /// Return `true` if the class is cyclically defined.
        ///
        /// Also, populates `visited_classes` with all base classes of `self`.
```

---

_@MichaReiser approved on 2025-01-20 08:52_

Nice

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4098 on 2025-01-20 12:19_

This function now feels like it's neither really a pure function (where you only have to worry about the return value) nor a function that only has side effects. You have to pay attention to the value the function returns, _and_ you have to pay attention to the mutations it's made to the `visited_classes` set.

To me it feels like it would be a bit clearer if we got rid of the return value, and instead passed in a mutable `bool` value? Usually I prefer pure functions, but here that's not possible. Something like this? (It passes all our tests for me locally)

```rs
    /// Return this class' involvement in an inheritance cycle, if any.
    ///
    /// A class definition like this will fail at runtime,
    /// but we must be resilient to it or we could panic.
    #[salsa::tracked]
    fn inheritance_cycle(self, db: &'db dyn Db) -> Option<InheritanceCycle> {
        /// Recursively search this class's bases searching for an inheritance cycle
        /// and determining the cycle's participants if one exists.
        fn search_for_cycle<'db>(
            db: &'db dyn Db,
            class: Class<'db>,
            classes_on_stack: &mut IndexSet<Class<'db>>,
            visited_classes: &mut IndexSet<Class<'db>>,
            found_cycle: &mut bool,
        ) {
            for explicit_base_class in class.fully_static_explicit_bases(db) {
                if !classes_on_stack.insert(explicit_base_class) {
                    *found_cycle = true;
                    return;
                }

                if visited_classes.insert(explicit_base_class) {
                    // If we find a cycle, keep searching to check if we can reach the starting class.
                    search_for_cycle(
                        db,
                        explicit_base_class,
                        classes_on_stack,
                        visited_classes,
                        found_cycle,
                    );
                }

                classes_on_stack.pop();
            }
        }

        let mut classes_on_stack = IndexSet::new();
        let mut visited_classes = IndexSet::new();
        let mut found_cycle = false;

        search_for_cycle(
            db,
            self,
            &mut classes_on_stack,
            &mut visited_classes,
            &mut found_cycle,
        );

        if !found_cycle {
            None
        } else if visited_classes.contains(&self) {
            Some(InheritanceCycle::Participant)
        } else {
            Some(InheritanceCycle::Inherited)
        }
    }
```

---

_@AlexWaygood reviewed on 2025-01-20 12:19_

Fantastic -- you make this look easy! Just one comment below

---

_@MichaReiser reviewed on 2025-01-20 13:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4098 on 2025-01-20 13:23_

This seems more complicated to me (I have to declare `found_cycle`, pass it, then check it). It seems much easier to get it wrong. I also generally avoid mutating primitive types in function calls, as it can be surprising. 

---

_@AlexWaygood reviewed on 2025-01-20 13:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4098 on 2025-01-20 13:30_

Alright, let's just land it as it is then :-)

---

_@AlexWaygood approved on 2025-01-20 13:31_

Thank you!

---

_Merged by @AlexWaygood on 2025-01-20 13:35_

---

_Closed by @AlexWaygood on 2025-01-20 13:35_

---
