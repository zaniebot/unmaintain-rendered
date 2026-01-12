```yaml
number: 16595
title: "[red-knot] Factor out shared unpacking logic"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: add-unpack-kind
created_at: 2025-03-10T06:53:31Z
updated_at: 2025-03-31T03:47:15Z
url: https://github.com/astral-sh/ruff/pull/16595
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Factor out shared unpacking logic

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
The logic for unpacking in assignment, for loops, and with items are very similar. Let's factor it out into some shared logic.

## Test Plan

<!-- How was it tested? -->
existing tests


---

_Label `red-knot` added by @AlexWaygood on 2025-03-10 09:20_

---

_Renamed from "Factor out shared unpacking logic" to "[red-knot] Factor out shared unpacking logic" by @ericmarkmartin on 2025-03-18 19:30_

---

_Comment by @github-actions[bot] on 2025-03-24 23:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @ericmarkmartin on 2025-03-25 04:07_

---

_Review requested from @carljm by @ericmarkmartin on 2025-03-25 04:07_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-03-25 04:07_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-03-25 04:07_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-03-25 04:07_

---

_@MichaReiser reviewed on 2025-03-27 12:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:961 on 2025-03-27 12:20_

I think I'd prefer the use of an `enum` here over a trait because it doesn't need to be open to extensions -- we know all instances. 



---

_Review requested from @dhruvmanila by @MichaReiser on 2025-03-27 12:22_

---

_@ericmarkmartin reviewed on 2025-03-27 21:44_

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:961 on 2025-03-27 21:44_

Doesn't the trait buy us static dispatch here? Also given that the trait is private to the module, it's only as extensible as an enum, right?

---

_@MichaReiser reviewed on 2025-03-27 21:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:961 on 2025-03-27 21:55_

It gives you inline for the specific variant by monomorphizing the code but a call to an enum also uses static dispatch without the cost of monomorphization.

---

_Review request for @sharkdp removed by @sharkdp on 2025-03-28 12:51_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:916 on 2025-03-28 15:21_

The use of a `OnceCell` to avoid the inference of the expression seems unnecessarily complicated compared to testing if `targets` is empty
```suggestion
        let targets = unpackable.targets();

        if targets.is_empty() {
            return;
        }

        let value = self.add_standalone_expression(unpackable.expression());
            let current_assignment = match target {
```


But I don't think skipping the creation of a standalone expression for the `value` is correct even if we have no targets because `infer_assignment_definition` always expects the standalone expression to exist. I don't think this is something we can test today because the parser only creates an assignment statement if there's at least one target, but that could change in the future. 

What was your reason for adding the optimization in the first place? Was it to avoid adding a standalone expression for the match case? 

I'm leaning towards simplifying this method to 

```
fn add_unpackable_assignment(
        &mut self,
        unpackable: &Unpackable<'db>,
        target: &'db ast::Expr,
        value: Expression<'db>,
    ) {
```

and let the caller deal with multiple targets or how when to add the value.



---

_Comment by @dhruvmanila on 2025-03-28 15:37_

This is looking good, thanks for working on this! I hope you don't mind but I'm going to push some changes to this PR to move around a couple of things and simply a bit. I don't want to waste your time in more follow-ups as this is mainly a refactor PR. I'll put up a comment listing down the changes I made in case you are interested and then merge it.

---

_@MichaReiser reviewed on 2025-03-28 15:38_

---

_@dhruvmanila approved on 2025-03-28 17:58_

Thank you!

I've made the following changes:
* Address Micha's review feedback and removed the `OnceCell`, update the function to accept the target and value expression, let caller handle multiple targets and adding the standalone expression
* Inlined the `unpack` method as it's only being used in a single place. We can create a common method if it's going to be used in multiple places but I don't think it will be now that we have a common method to handle unpackable assignments

---

_Label `internal` added by @dhruvmanila on 2025-03-28 17:58_

---

_@ericmarkmartin reviewed on 2025-03-28 18:10_

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:916 on 2025-03-28 18:10_

I had intended it for the case where you don't have a target in the with case. When `Unpackable` was a trait, I only had an iterator over the targets, hence why I didn't check length

---

_Merged by @dhruvmanila on 2025-03-28 18:22_

---

_Closed by @dhruvmanila on 2025-03-28 18:22_

---

_Branch deleted on 2025-03-31 03:47_

---
