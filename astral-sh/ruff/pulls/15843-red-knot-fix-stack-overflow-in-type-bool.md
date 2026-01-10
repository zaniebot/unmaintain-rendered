```yaml
number: 15843
title: "[red-knot] Fix Stack overflow in Type::bool "
type: pull_request
state: merged
author: mishamsk
labels:
  - ty
assignees: []
merged: true
base: main
head: 15672-fix-stack-overflow-in-type-bool-evaluation
created_at: 2025-01-31T04:05:45Z
updated_at: 2025-02-05T16:47:52Z
url: https://github.com/astral-sh/ruff/pull/15843
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Fix Stack overflow in Type::bool 

---

_Pull request opened by @mishamsk on 2025-01-31 04:05_

## Summary

This PR adds `Type::call_bound` method for calls that should follow descriptor protocol calling convention. The PR is intentionally shallow in scope and only fixes #15672

Couple of obvious things that weren't done:

* Switch to `call_bound` everywhere it should be used
* Address the fact, that red_knot resolves `__bool__ = bool` as a Union, which includes `Type::Dynamic` and hence fails to infer that the truthiness is always false for such a class (I've added a todo comment in mdtests)
* Doesn't try to invent a new type for descriptors, although I have a gut feeling it may be more convenient in the end, instead of doing method lookup each time like I did in `call_bound`

## Test Plan

* extended mdtests with 2 examples from the issue
* cargo neatest run 


---

_Review requested from @carljm by @mishamsk on 2025-01-31 04:05_

---

_Review requested from @MichaReiser by @mishamsk on 2025-01-31 04:05_

---

_Review requested from @AlexWaygood by @mishamsk on 2025-01-31 04:05_

---

_Review requested from @sharkdp by @mishamsk on 2025-01-31 04:05_

---

_Label `red-knot` added by @dhruvmanila on 2025-01-31 04:12_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/not.md`:142 on 2025-02-03 21:59_

I don't think this case is important enough to test in two different places.

I think it belongs better where you've put it (in `truthiness.md`). But really, most of this file should be moved over to `truthiness.md`, this file should really just test type negation and nothing else. It would probably be better to either move all the instance `__bool__` tests over at once, or none of them.

Best would probably be to move them in a separate PR, rather than in this one.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2163 on 2025-02-03 22:02_

This should handle `Type::SubclassOf` as well -- but so should `Type::call`, and it doesn't either. So this can probably be left to a separate PR to fix.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/truthiness.md`:54 on 2025-02-03 22:05_

Nit: prefer Markdown prose paragraph before the code block over comments in the code block, for general commentary like this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/truthiness.md`:76 on 2025-02-03 22:09_

Nit: make this and the above two separate test headings (`# __bool__ is bool` and `# conditional __bool__`) with two separate code blocks.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/truthiness.md`:59 on 2025-02-03 22:19_

I think this TODO is not quite describing the situation accurately. We already correctly implement that `bool` called with no arguments returns `Literal[False]`. And your PR does correctly implement that calling the `bool` class as a method, since it is not a descriptor, calls `bool` with no arguments, which returns `Literal[False]`.

But since `BoolIsBool.__bool__` has no declared type, we model that it could be reassigned to any type, so we give it the type `Unknown | Literal[bool]` instead of the type `Literal[bool]`. And `Unknown` has ambiguous truthiness, so the result is that we still infer `bool`, rather than `Literal[false]`, here.

Since this result is not wrong, and the reasons we arrive at it are complex to explain (and are already explained in detail in other tests), I would just remove this TODO entirely here. The main point of this test is that we can handle this situation without a panic.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2196 on 2025-02-03 22:22_

Since we don't have tests for descriptor protocol (and this PR doesn't add any), and we may want to design our descriptor protocol support a bit more holistically (not just specific to bound calls), I think we should not add support for it here. Instead we should just simplify this code to assume non-descriptor, with a TODO comment for descriptor support.
```suggestion
                // TODO descriptor protocol. For now, assume non-descriptor and call without `self` argument.
                self.call(db, arguments)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2200 on 2025-02-03 22:24_

Nit: can we group this down with the default not-callable case and explicitly mark them with a comment as "cases that duplicate `Type::call`"?

I think it may be worth duplicating these cases here, rather than deferring to `Type::call`, since they are very simple, and it will be less efficient to defer to `Type::call`. But I'd like to be clear on which cases should be kept in exact sync.

---

_@carljm requested changes on 2025-02-03 22:25_

Thank you for the PR! I think the `call_bound` abstraction makes sense, and is needed to handle this case correctly. Some inline comments.

---

_@mishamsk reviewed on 2025-02-04 01:36_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/unary/not.md`:142 on 2025-02-04 01:36_

sure, agree. I erred on the side of caution and didn't want to impose my view on how tests should be structured. At least not with my 4th PR ;-) 

I removed this specific case from `not.md` and left it only in the `truthiness.md` file. I can move the rest in a separate follow-up or as a flyby with other stuff

---

_@mishamsk reviewed on 2025-02-04 01:37_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2163 on 2025-02-04 01:37_

Happy to do that in a follow-up. See my comment regarding descriptor protocol below

---

_@mishamsk reviewed on 2025-02-04 01:38_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/type_properties/truthiness.md`:54 on 2025-02-04 01:38_

fixed

---

_@mishamsk reviewed on 2025-02-04 01:40_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/type_properties/truthiness.md`:59 on 2025-02-04 01:40_

I haven't found the explanation to this phenomenon yet, but assuming I'll continue working on red_knot I will.

Remove the comment for now as you've suggested

---

_@mishamsk reviewed on 2025-02-04 01:41_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/type_properties/truthiness.md`:76 on 2025-02-04 01:41_

fixed. I've used 3rd level headings, hopefully this is not forbidden!

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2196 on 2025-02-04 01:51_

I'd love to work on the descriptor protocol as a whole, would be fun and will allow me to better review more of the red_knot codebase. If you are fine with that, I can open a separate draft PR and outline a plan.

One obvious place to start would be: https://github.com/astral-sh/ruff/blob/0529ad67d76d01c5ca586ad9b3db8b37c1a930dc/crates/red_knot_python_semantic/src/types.rs#L4211-L4219  and implement proper handling of `property` instead of a current "workaround". This would serve as a test. + I can make a special mdtest suite for descriptors in general.

As for my code here, I do not think that there is harm in leaving it, as it will work for cases when `__get__` method is defined explicitly already, but since you suggest removing it, I'll just stash it and see if it eventually survives.

---

_@mishamsk reviewed on 2025-02-04 01:51_

---

_@mishamsk reviewed on 2025-02-04 02:01_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2200 on 2025-02-04 02:01_

yes, totally makes sense. re-order and added a comment.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/truthiness.md`:76 on 2025-02-04 20:27_

nope, that's fine! we definitely do it elsewhere already (even some fourth-level i think)

---

_@carljm approved on 2025-02-04 20:29_

Looks great, thank you!

---

_@carljm reviewed on 2025-02-04 20:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unary/not.md`:142 on 2025-02-04 20:30_

That would be great if you want to do this! I recommend separate PR, there's no real cost to more smaller PRs, and it can make it a lot easier to review this kind of change if it's not combined with functional changes.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2163 on 2025-02-04 20:36_

Sure, that would be great! I created https://github.com/astral-sh/ruff/issues/15948

---

_@carljm reviewed on 2025-02-04 20:37_

---

_@carljm reviewed on 2025-02-04 20:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2196 on 2025-02-04 20:39_

Thanks for removing this for now. I agree it's possible that later it may come back in the same or similar form, but I would rather it arrive along with tests exercising it.

I think @sharkdp was planning to tackle descriptors soon, but may be open to you working on that instead, freeing him up to tackle something else? Let's wait for him to respond before you start on it, in case he has already started on it.

It would definitely be good to open an issue with a proposed design first, so we can discuss, before opening a PR; this may save some implementation work.

---

_Merged by @carljm on 2025-02-04 20:40_

---

_Closed by @carljm on 2025-02-04 20:40_

---

_@mishamsk reviewed on 2025-02-04 23:16_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2196 on 2025-02-04 23:16_

@carljm @sharkdp I can create an outline with items that needs to be tackled in principle, a proposed step-by-step plan and draft design (or at least some thinking around it). Shall I use discussions or an issue?

---

_Branch deleted on 2025-02-04 23:16_

---

_@carljm reviewed on 2025-02-04 23:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2196 on 2025-02-04 23:21_

That sounds like a great approach! We usually use issues, not discussions. I think @sharkdp was planning to create a set of issues tomorrow for various follow-ups to his recent work on instance attributes, including descriptor protocol as one of those.

---

_@sharkdp reviewed on 2025-02-05 13:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2196 on 2025-02-05 13:42_

> @carljm @sharkdp I can create an outline with items that needs to be tackled in principle, a proposed step-by-step plan and draft design (or at least some thinking around it). Shall I use discussions or an issue?

@mishamsk If you want to work on descriptors, we would definitely appreciate it! Starting with a separate design step sounds like a good idea. Please feel free to add comments to this issue: https://github.com/astral-sh/ruff/issues/15966

I will also set up an initial draft for a descriptor test suite that we can detail further while we work on the design and the implementation (Edit: https://github.com/astral-sh/ruff/pull/15972).

---

_@sharkdp reviewed on 2025-02-05 13:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2196 on 2025-02-05 13:55_

In case you want to work on something with a smaller scope first, I created a couple of fine-grained tickets around our understanding of instance and class attributes. They are listed as sub-tasks [here](https://github.com/astral-sh/ruff/issues/14164) and are roughly ordered by priority and inter-dependencies. I marked some of these issues as ["help wanted"](https://github.com/astral-sh/ruff/issues?q=is%3Aopen+label%3Ared-knot+label%3A%22help+wanted%22).

---

_@mishamsk reviewed on 2025-02-05 16:47_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2196 on 2025-02-05 16:47_

@sharkdp thanks a lot for all the links. Helps a ton, I wouldn't have found that tracking issue myself easily.

I'll try to do both - comment on some plan rgd descriptors & pick up one of the issues from #14164 - will do at least of two later today (I am EST)

---
