```yaml
number: 16004
title: "[red-knot] Unpacking and for loop assignments to attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/more-attribute-assignments
created_at: 2025-02-06T21:52:30Z
updated_at: 2025-02-07T11:19:41Z
url: https://github.com/astral-sh/ruff/pull/16004
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Unpacking and for loop assignments to attributes

---

_Pull request opened by @sharkdp on 2025-02-06 21:52_

## Summary

* Support assignments to attributes in more cases:
  - assignments in `for` loops
  - in unpacking assignments
* Add test for multi-target assignments (these were already working, but untested)
* Add tests for all other possible assignments to attributes that could possibly occur (in decreasing order of likeliness):
  - augmented attribute assignments
  - attribute assignments in `with` statements
  - attribute assignments in comprehensions
  - Note: assignments to attributes in named expressions are not syntactically allowed

closes #15962

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2025-02-06 21:52_

---

_Review requested from @carljm by @sharkdp on 2025-02-06 21:52_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-06 21:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-06 21:52_

---

_Renamed from "[red-knot] For loop and unpacking assignments for attributes" to "[red-knot] Unpacking and for loop assignments to attributes" by @sharkdp on 2025-02-06 21:53_

---

_@sharkdp reviewed on 2025-02-06 21:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:246 on 2025-02-06 21:54_

This was already working, just added a test.

---

_@sharkdp reviewed on 2025-02-06 21:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:341 on 2025-02-06 21:56_

The TODO message was not quite right here, I think `Unknown | int` / `Unknown | str` are the correct types here. There is no declaration.

The attributes could technically also be unbound. We don't discover this in other cases either, though.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:354 on 2025-02-06 21:57_

I don't know *why* anyone would do something like this, but while we're at it …

(Mypy and pyright support this)

---

_@sharkdp reviewed on 2025-02-06 21:57_

---

_@sharkdp reviewed on 2025-02-06 21:57_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:376 on 2025-02-06 21:57_

And this is even weirder....

(Mypy and pyright support this)

---

_@sharkdp reviewed on 2025-02-06 22:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/attribute_assignment.rs`:30 on 2025-02-06 22:01_

With the new variants, this now looks similar to `CurrentAssignment`, but there are differences, and also, attribute assignments are not supported in all scenarios. For example, the target in a *name*d expression can only be a … *name*! So something like `(self.x := 1)` is not legal.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:341 on 2025-02-06 22:02_

Yes, determining that an attribute is definitely-initialized is a complex problem that I think we should defer for now.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:345 on 2025-02-06 22:03_

```suggestion
#### Attributes defined in comprehensions
```

---

_@sharkdp reviewed on 2025-02-06 22:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4259 on 2025-02-06 22:06_

I need to think about this again and maybe write a comment here, but I think it would be a bug if we would fail to look up `attribute_expression_id`, so I made this a hard error instead of doing `if let Some(…inferred_ty) = ….get(…) { … }`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:376 on 2025-02-06 22:06_

FWIW, this case is closely related to the "nested function" case, which is tested at the very end of this test file.

---

_@AlexWaygood reviewed on 2025-02-06 22:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4240 on 2025-02-06 22:15_

I expect they would still be reported by normal type inference, no? So I'm not sure any special handling is needed here, thus maybe we don't need a TODO either?

Maybe ideally we'd add some tests for cases where diagnostics occur either in iteration, or more generally in type inference of the RHS of a `self.x = ...` assignment, and verify that we do surface those diagnostics.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4259 on 2025-02-06 22:16_

Yes, the `attribute_expression_id` should always be the ID of a target of the unpacking, and unpacking should always populate a type for each target. I guess the latter is not really comprehensively guaranteed at the moment; it depends on the `match target` in `Unpacker::unpack_inner` being comprehensive over all possible target forms. But as long as it covers all the forms that we ever create an assignment for in semantic index building, it will be fine.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:203 on 2025-02-06 22:18_

Doesn't really matter, but I think this could be just `pub(super)`?

---

_@carljm approved on 2025-02-06 22:19_

Love it!

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-02-07 07:54_

---

_@MichaReiser approved on 2025-02-07 07:56_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4259 on 2025-02-07 08:37_

Yeah, that's correct. The unpacker should populate for each target variable even in the case of length mismatch. I think that wasn't the case in the first iteration but is now guaranteed by this loop:

https://github.com/astral-sh/ruff/blob/a00483a7856cfe5b8c1c59a0e5633a4906d865df/crates/red_knot_python_semantic/src/types/unpacker.rs#L167-L174

Not related to this PR but I think we should also update other places where unpacking is done to consider this a hard error, currently it uses `unwrap_or(Type::unknown())`.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:77 on 2025-02-07 08:39_

Can we add test cases when the unpacking target is a starred attribute? Like:
```py
In [9]: class Foo:
   ...:     def __init__(self):
   ...:         self.x = (1, 2)
   ...: 
   ...: 
   ...: foo = Foo()
   ...: print(foo.x)
   ...: 
   ...: (a, *foo.x) = (1, 2, 3)
   ...: print(foo.x)
(1, 2)
[2, 3]
```

Ignore if it's already present but I didn't find it in the diff of this PR.

---

_@dhruvmanila reviewed on 2025-02-07 08:39_

---

_@dhruvmanila reviewed on 2025-02-07 08:41_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:77 on 2025-02-07 08:41_

(We don't yet support inferring the type here because it requires generic support over list)

---

_@dhruvmanila reviewed on 2025-02-07 08:44_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4259 on 2025-02-07 08:44_

We _could_ make this an invariant on Unpacker and update the return type of `get` to `Type` to avoid the `.expect` calls to be spread across similar to how we have methods that directly indexes the inference result.

---

_@sharkdp reviewed on 2025-02-07 09:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4259 on 2025-02-07 09:27_

> currently it uses `unwrap_or(Type::unknown())`.

Yes, that's the main reason why I was hesitant. I can look into that.

---

_@sharkdp reviewed on 2025-02-07 09:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4240 on 2025-02-07 09:55_

> I expect they would still be reported by normal type inference, no?

I also hoped that would be the case, but no. We emit the `non-iterable` diagnostic from `infer_for_statement_definition`, and attribute assignments are not definitions. I added a test to demonstrate this.

~~We have a similar problem where we do not yet identify cases where (normal) assignments to attributes conflict the declared type of that attribute. We have existing tests for this as well.~~

(Edit: no, I already implemented this :smile:)

I'll also open a new ticket to address both of these issues.

> or more generally in type inference of the RHS of a `self.x = ...` assignment

The general case works fine. We only fail to emit diagnostics if they originate from `infer_…_definition` calls, which do not apply to attribute assignments. I added the following test case:
```py
class C:
    def __init__(self) -> None:
        # error: [too-many-positional-arguments]
        self.x: int = len(1, 2, 3)
```

---

_Merged by @sharkdp on 2025-02-07 10:30_

---

_Closed by @sharkdp on 2025-02-07 10:30_

---

_Branch deleted on 2025-02-07 10:30_

---

_@sharkdp reviewed on 2025-02-07 11:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4259 on 2025-02-07 11:19_

https://github.com/astral-sh/ruff/pull/16018

---
