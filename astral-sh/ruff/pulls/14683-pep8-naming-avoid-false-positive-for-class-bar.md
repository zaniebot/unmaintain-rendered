```yaml
number: 14683
title: "[`pep8-naming`] Avoid false positive for `class Bar(type(foo))` (`N804`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/fix-14675
created_at: 2024-11-29T18:34:38Z
updated_at: 2024-12-04T11:20:51Z
url: https://github.com/astral-sh/ruff/pull/14683
synced_at: 2026-01-10T20:42:27Z
```

# [`pep8-naming`] Avoid false positive for `class Bar(type(foo))` (`N804`)

---

_Pull request opened by @ntBre on 2024-11-29 18:34_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Fixes #14675, which reports N804 (`invalid-first-argument-name-for-class-method`) for this code:

```python
foo = {}

class Bar(type(foo)):
    def foo_method(self):
        pass
```

I don't really love having to capture `maybe` in the closure, but this was the best way I could come up with to avoid propagating the new `IsMetaclass` return type through `any_base_class`. Another option along these lines, which at least avoids changing the `Fn`s to `FnMut`, is to let `maybe` be a `RefCell<bool>` and mutate it that way.

## Test Plan

<!-- How was it tested? -->
`cargo test` with the example above added to `N804.py`.


---

_Review requested from @AlexWaygood by @ntBre on 2024-11-29 18:34_

---

_Comment by @github-actions[bot] on 2024-11-29 18:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:126 on 2024-11-29 18:49_

```suggestion
#[derive(Debug, PartialEq, Eq, Clone, Copy)]
pub enum IsMetaclass {
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-29 19:07_

> I don't really love having to capture maybe in the closure

hmm, yeah, this does feel a bit... icky 😄

I wonder if a better solution (though it's a bit more of a refactor) would be to split up `any_base_class()` into two functions: one that just handles iterating through all superclasses recursively (`iter_base_classes()` or `iter_superclasses()`), and one that checks if a condition holds true for any of the superclasses (that one could still be called `any_base_class()`). That way `is_metaclass()` could use the lower-level `iter_base_classes()` function directly rather than having to awkwardly work around the fact that the quite high-level `any_base_class` function doesn't really have quite the API we'd like it to have here.

WDYT?

---

_@AlexWaygood reviewed on 2024-11-29 19:07_

---

_Comment by @AlexWaygood on 2024-11-29 19:07_

Also thanks for jumping on this! :D

---

_@ntBre reviewed on 2024-11-29 19:10_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-29 19:10_

I think that's a great idea, I'll give it a try!

---

_Label `bug` added by @AlexWaygood on 2024-11-29 19:11_

---

_@MichaReiser reviewed on 2024-11-29 19:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-29 19:13_

Just some extra data point if it helps to avoid a lot of work: I don't mind that it it captures the variable

---

_@ntBre reviewed on 2024-11-29 22:25_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-29 22:25_

This was a little harder than I thought. I've added an `iter_base_classes` function, but it's not truly an iterator now since I build up a `Vec` of base classes and then call `into_iter`. I'll keep thinking about this, but at least the API has the shape I was going for.

---

_@MichaReiser reviewed on 2024-11-30 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-30 15:08_

I kind of prefer the old solution. It overall felt simpler and avoiding the extra allocation is nice too

---

_@AlexWaygood reviewed on 2024-11-30 17:26_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-30 17:26_

Yeah, writing a recursive iterator seems annoyingly hard :(

Sorry for leading you down the wrong track here @ntBre. My bad -- your first solution probably was best!

---

_@ntBre reviewed on 2024-11-30 19:32_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-30 19:32_

No worries, it was interesting to try! I've reverted to the original.

One other thing I just noticed, should `maybe` also be `true` for the subscript case? I only handled the `type(foo)` example directly from the issue, but should `type[foo]` work as well? That's just a one-line code change, but I can update the tests too.

---

_@AlexWaygood reviewed on 2024-11-30 22:35_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-30 22:35_

> should `maybe` also be `true` for the subscript case

I don't think so -- `type[str]`, `type[int]`, etc. always behaves the same as bare `type` in the context of a class's bases, and if a class inherits from `type` then it's always a metaclass

---

_@AlexWaygood reviewed on 2024-11-30 22:36_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:142 on 2024-11-30 22:36_

Even subscripts that are "invalid" from a typing perspective behave the same way:

```pycon
>>> class bar(type[int, str]): pass
... 
>>> bar.__bases__
(<class 'type'>,)
```

---

_@AlexWaygood approved on 2024-11-30 22:36_

Thanks!

---

_Merged by @AlexWaygood on 2024-11-30 22:37_

---

_Closed by @AlexWaygood on 2024-11-30 22:37_

---

_Branch deleted on 2024-11-30 22:43_

---

_Comment by @alexhenman on 2024-12-04 11:20_

Thanks all for your very speedy work getting this fixed!

---
