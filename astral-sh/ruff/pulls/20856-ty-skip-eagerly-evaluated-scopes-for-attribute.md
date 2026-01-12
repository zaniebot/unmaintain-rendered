```yaml
number: 20856
title: "[ty] Skip eagerly evaluated scopes for attribute storing"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: comprehension-assign-attr-unresolved
created_at: 2025-10-14T07:00:32Z
updated_at: 2025-11-11T22:45:41Z
url: https://github.com/astral-sh/ruff/pull/20856
synced_at: 2026-01-12T15:57:11Z
```

# [ty] Skip eagerly evaluated scopes for attribute storing

---

_@Glyphack_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix https://github.com/astral-sh/ty/issues/664

This PR adds support for storing attributes in comprehension scopes (any eager scope.)

For example in the following code we infer type of `z` correctly:

```py
class C:
    def __init__(self):
        [None for self.z in range(1)]
reveal_type(C().z) # previously [unresolved-attribute] but now shows Unknown | int
```

The fix works by adjusting the following logics:

To identify if an attriute is an assignment to self or cls we need to check the scope is a method. To allow comprehension scopes here we skip any eager scope in the check.
Also at this stage the code checks if self or the first method argument is shadowed by another binding that eager scope to prevent this:

```py
class D:
    g: int

class C:
    def __init__(self):
        [[None for self.g in range(1)] for self in [D()]]
reveal_type(C().g) # [unresolved-attribute]
```

When determining scopes that attributes might be defined after collecting all the methods of the class the code also returns any decendant scope that is eager and only has eager parents until the method scope.

When checking reachability of a attribute definition if the attribute is defined in an eager scope we use the reachability of the first non eager scope which must be a method. This allows attributes to be marked as reachable and be seen.


There are also which I didn't add support for:

```py
class C:
    def __init__(self):
        def f():
            [None for self.z in range(1)]
        f()

reveal_type(C().z) # [unresolved-attribute]
```

In the above example we will not even return the comprehension scope as an attribute scope because there is a non eager scope (`f` function) between the comprehension and the `__init__` method

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "Skip eagerly evaluated scopes for attribute storing" to "[ty] Skip eagerly evaluated scopes for attribute storing" by @Glyphack on 2025-10-14 07:01_

---

_Label `ty` added by @AlexWaygood on 2025-10-14 07:09_

---

_Comment by @github-actions[bot] on 2025-10-15 19:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-15 19:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_hooks.py:154:48: error[missing-argument] No argument provided for required parameter `y`
- tests/test_next_gen.py:142:14: error[unresolved-attribute] Object of type `dataclasses.Field` has no attribute `validator`
- tests/test_next_gen.py:380:14: error[unresolved-attribute] Object of type `dataclasses.Field` has no attribute `validator`
- Found 584 diagnostics
+ Found 581 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@Glyphack reviewed on 2025-10-15 19:34_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/src/semantic_index/builder.rs`:191 on 2025-10-15 19:34_

This function is also used at https://github.com/Glyphack/ruff/blob/8d53802bc7e9566a9087fd3e3a460345e40a7e6d/crates/ty_python_semantic/src/semantic_index/builder.rs#L1681 and I think this new behavior should be there as well.
I'm not happy with the name though, this now returns either method or an eager child scope of the class.

---

_Marked ready for review by @Glyphack on 2025-10-15 19:37_

---

_Review requested from @carljm by @Glyphack on 2025-10-15 19:37_

---

_Review requested from @AlexWaygood by @Glyphack on 2025-10-15 19:37_

---

_Review requested from @sharkdp by @Glyphack on 2025-10-15 19:37_

---

_Review requested from @dcreager by @Glyphack on 2025-10-15 19:37_

---

_Comment by @Glyphack on 2025-10-15 19:39_

There is no ecosystem change detected by mypy primer? With the amount of custom logic added I expected to have more effect. Do you think I should simplify the logic even more since there's not much impact?

Other solution could be only support children eager scopes and don't go into decendants. This would turn a lot of loops into if statements.

---

_Comment by @sharkdp on 2025-10-16 08:15_

> There is no ecosystem change detected by mypy primer? With the amount of custom logic added I expected to have more effect. Do you think I should simplify the logic even more since there's not much impact?

I would expect the impact to be bigger when combined with type-of-`self`-in-method bodies, no? Maybe we could test that opening a draft PR that merges both. Not strictly necessary though.

> Other solution could be only support children eager scopes and don't go into decendants. This would turn a lot of loops into if statements.

I don't really see a reason not to support that if you already implemented it correctly.

---

_Comment by @sharkdp on 2025-10-16 08:16_

Thank you for working on this? Could you please also add some tests with generic methods like described [here](https://github.com/astral-sh/ty/issues/664#issuecomment-3380666555)? I'd like to see that we correctly skip the type params scope when detecting methods in classes.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-16 08:59_

---

_Comment by @Glyphack on 2025-10-16 15:10_

Thanks. I didn't think of the impact there, I was still hoping to reduce the false positives like [this](https://play.ty.dev/5f89a8f7-2d3e-456e-82e5-c6be2c9fbe62). So let's keep the current implementation.

I went over the tests again added more examples of what is supported.

---

_Comment by @sharkdp on 2025-10-22 15:00_

I merged this branch into the type-of-self PR and compared it with the type-of-self PR alone. This seems to remove 4 diagnostics from the ecosystem:

type-of-self:

<img width="547" height="73" alt="image" src="https://github.com/user-attachments/assets/09926e07-ce6e-46c2-877c-5deee16b2242" />

type-of-self + this:

<img width="536" height="72" alt="image" src="https://github.com/user-attachments/assets/dd1bda6a-4a84-4e70-8ca6-016a3144418d" />

I did this to evaluate whether I need to prioritize this, or if it can land separately afterwards. After seeing these results, I'm going with the latter.

But we'll come back to this eventually.

---

_Comment by @carljm on 2025-11-06 14:18_

Just rebased this (and updated some tests) to get an updated ecosystem report.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:454 on 2025-11-06 15:14_

It's not totally clear to me if this is the right behavior or not. We can't guarantee that these attributes will ever be initialized. We can definitely say that they _might_ be.

For reference, this behavior [matches pyright](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFD0DGFZAzm1AMIBc9UAqABNCwKFSqokMCQAo2hCsACUfQeuGiowWav4aDAYiiEQ4EDygBtAK4oibMBVJCAtGRgwQSAEY2YhAC6%2BgbqVgB0kdq4UApK4WSYKNYAjIHBIRoiYmi6aqHqxqbmlrb2hI7OhG4eXr7%2BQZkFEVHAMXHA4T5JqemMBbnKjESklDSIhLJcuglDI%2BTUtJPTyl1zJAvjdFMzTMpAA). [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=fe330769b4d8a148d2c801648eca02fb) finds both attributes and gives them type `Any`. [Pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN2UYANxiooAfSbEYAHXRyAxlFRw4dAMKI5dHXUwwwdceNYQGxgBRwYUMAEot6Xc70G6YCw%2B0ufAYjowlNSUiHQA2gCu6IJwuFAimAC0qAwMlBDYEQwwALrePs5hhMXuvHTWtoSonE5hAIw5efJOBfqGbJ6OBS7%2BgcGhkdHwcQnJqemZ2U3dhcWEpZTlNmCE2DXhDU35Lh12cnKCImKSzDAW6p5Ve0NHElJnF3ar14eid6fnlwrXIAA0IFloHASORECB-ABVBjQMykdxRBTQ3DoOD7LBuMC8GgpcToCI0bCBCz4UKsBh2OiJAB85TSXV0ggYEUoTjAMhAADl8YSQnRgPgAL7suR-EBkQRgKCkQgMWhQCj%2BAAKpAlUvKGBwBDoCmRkDYzJSEGRhDk-gAyjAYHQABapYhwRAAekd4oMUsIvDYjpg6EdmFwCjgjp16D1BqRvoWdFQQlQ0FQ2Fg2t1EH1lENyLouGIEeBcjIDGtyMSIkocCNTgAvHR2QBmQh1ABMwvQIAF-1QiIgIgAYtAYBQ0Fg8EQyG2gA) finds both attributes and gives them type `int`. No type checker attempts to distinguish these cases based on trying to determine whether the nested function is ever called; this is quite complex to do in general and I doubt we should attempt it either.

Overall I think the current behavior in this PR is fine and this case is probably rare (and it doesn't show up in the ecosystem report.) I don't think we need TODOs to revisit it; we'll revisit it only if there are real user reports.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:452 on 2025-11-06 15:17_

I don't think we need a TODO specifically here, because I don't think we will ever attempt to distinguish `f()` / `self.a` from `g()` / `self.b` here, based on call-graph analysis.
```suggestion
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:192 on 2025-11-06 15:31_

```suggestion
    /// Returns the scope ID of the current scope if the current scope
    /// is a method inside a class body or an eagerly executed scope inside a method.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index.rs`:1231 on 2025-11-06 15:42_

It's not clear to me that this test and the below test couldn't be equally effective as mdtests (and/or aren't already effectively duplicated by mdtests in this PR.) But these seem harder to maintain. Do you feel like they are important?

---

_@carljm approved on 2025-11-06 15:43_

---

_@Glyphack reviewed on 2025-11-07 06:57_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/src/semantic_index.rs`:1231 on 2025-11-07 06:57_

Applied.
I had to change `semantic_index.rs` to track attributes and adjust the reachability analysis in `class.rs`. During development I used this test to ensure the `semantic_index` logic was working fine and then updated the other file at that point I didn't know how to validate it with an mdtest. So now that we have the full implementation we can get rid of this.

---

_@Glyphack reviewed on 2025-11-07 07:05_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/resources/mdtest/attributes.md`:454 on 2025-11-07 07:05_

I haven't worked with call graph analysis and didn't think about it's complexity. If an issue was reported for this I would follow it up.

---

_Merged by @carljm on 2025-11-11 22:45_

---

_Closed by @carljm on 2025-11-11 22:45_

---

_Comment by @carljm on 2025-11-11 22:45_

Thank you!

---
