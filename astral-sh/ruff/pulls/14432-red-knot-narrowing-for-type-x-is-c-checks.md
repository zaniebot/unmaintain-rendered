```yaml
number: 14432
title: "[red-knot] Narrowing for `type(x) is C` checks"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/type-x-is-c-narrowing
created_at: 2024-11-18T12:27:20Z
updated_at: 2024-11-18T15:21:48Z
url: https://github.com/astral-sh/ruff/pull/14432
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Narrowing for `type(x) is C` checks

---

_@sharkdp_

## Summary

Add type narrowing for `type(x) is C` conditions (and `else` clauses of `type(x) is not C` conditionals):

```py
if type(x) is A:
    reveal_type(x)  # revealed: A
else:
    reveal_type(x)  # revealed: A | B
```

closes: #14431, part of: #13694

## Test Plan

New Markdown-based tests.

---

_Label `red-knot` added by @sharkdp on 2024-11-18 12:27_

---

_Review requested from @carljm by @sharkdp on 2024-11-18 12:27_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-18 12:27_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-18 12:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:51 on 2024-11-18 12:31_

```suggestion
from typing import Literal

class IsEqualToEverything(type):
    def __eq__(cls, other) -> Literal[True]:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:9 on 2024-11-18 12:32_

```suggestion
def get_a_or_b() -> A | B:
    return A()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:28 on 2024-11-18 12:32_

```suggestion
def get_a_or_b() -> A | B:
    return A()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:52 on 2024-11-18 12:32_

```suggestion
def get_a_or_b() -> A | B:
    return A()
```

---

_@sharkdp reviewed on 2024-11-18 12:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:63 on 2024-11-18 12:34_

Both pyright and mypy are less strict here and infer `A` instead of `A | B`. This is unsound, as can be demonstrated by returning `B()` from `get_a_or_b()`, and observing that this branch is taken.

---

_Comment by @codspeed-hq[bot] on 2024-11-18 12:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david/type-x-is-c-narrowing)

### Merging #14432 will **not alter performance**

<sub>Comparing <code>david/type-x-is-c-narrowing</code> (2e7cd69) with <code>main</code> (3642381)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:42 on 2024-11-18 12:39_

I think we _would_ be able to narrow in something like this, though, because here we can statically determine that the object returned by `get_a_or_b()` must be an instance of a class with `type` as its metaclass (since both `A` and `B` are `@final`):

```py
from typing import final

@final
class A: ...

@final
class B: ...

def get_a_or_b() -> A | B:
    return A()

x = get_a_or_b()

if type(x) != A:
    reveal_type(x)
```

Worth a TODO comment?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:288 on 2024-11-18 12:42_

```suggestion
                    let symbol = self
                        .symbols()
                        .symbol_id_by_name(id)
                        .expect("Should always have a symbol for every Name node");
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:330 on 2024-11-18 12:43_

```suggestion
                        .is_some_and(|c| c.class.is_known(self.db, KnownClass::Type))
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:334 on 2024-11-18 12:45_

Although it gets tedious to write this out every time -- maybe we should have a helper method on the symbol table that calls `.expect()` for us with this error message.

```suggestion
                            let symbol = self
                                .symbols()
                                .symbol_id_by_name(id)
                                .expect("Should always have a symbol for every Name node");
```

---

_@AlexWaygood approved on 2024-11-18 12:47_

Nice!

---

_@AlexWaygood reviewed on 2024-11-18 12:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:42 on 2024-11-18 12:50_

In fact, I think narrowing would also be safe if both classes had `@final` metaclasses, since the metaclass of a subclass of `A` must either be exactly equal to the metaclass of `A` or must be a proper subclass of the metaclass of `A`. E.g. narrowing would also be safe, I think, for something like this:

```py
from typing import final

@final
class Meta(type): ...

class A(metaclass=Meta): ...
class B(metaclass=Meta): ...

def get_a_or_b() -> A | B:
    return A()

x = get_a_or_b()

if type(x) != A:
    reveal_type(x)
```

---

_Comment by @github-actions[bot] on 2024-11-18 12:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-11-18 12:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:331 on 2024-11-18 12:56_

This might help optimise things slightly? The more we can do just by looking at the AST, the cheaper things will be:

```suggestion
                    if arguments.len() != 1 {
                        continue;
                    }

                    let callable_ty =
                        inference.expression_ty(callable.scoped_expression_id(self.db, scope));

                    if callable_ty
                        .into_class_literal()
                        .is_some_and(|c| c.class.is_known(self.db, KnownClass::Type))
```

---

_@sharkdp reviewed on 2024-11-18 12:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:9 on 2024-11-18 12:59_

What is the idea behind this?

---

_@AlexWaygood reviewed on 2024-11-18 13:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:9 on 2024-11-18 13:01_

Eventually we'll emit errors on functions like this, warning the user that the function is annotated as returning `A | B` but actually returns `None`. When we add that check, we'll have to update a lot of mdtests. The more functions like this we add to our mdtests that have empty bodies, the more work it'll be :P

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:333 on 2024-11-18 13:02_

This panics with an index out of bounds if I make this modification to your tests:

```diff
--- a/crates/red_knot_python_semantic/resources/mdtest/narrow/type.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/narrow/type.md
@@ -10,7 +10,7 @@ def get_a_or_b() -> A | B: ...
 
 x = get_a_or_b()
 
-if type(x) is A:
+if type(object=x) is A:
     reveal_type(x)  # revealed: A
 else:
     # It would be wrong to infer `B` here. The type
```

Which is an invalid call, but we shouldn't panic on it

```
---- mdtest__narrow_type stdout ----
thread 'mdtest__narrow_type' panicked at crates/red_knot_python_semantic/src/types/narrow.rs:333:43:
index out of bounds: the len is 0 but the index is 0
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    mdtest__narrow_type

test result: FAILED. 96 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.67s
```

---

_@AlexWaygood requested changes on 2024-11-18 13:02_

---

_@AlexWaygood reviewed on 2024-11-18 13:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:333 on 2024-11-18 13:04_

This is because you check `arguments.len() == 1` above, but that will be true if there's exactly one _keyword_ argument and 0 positional arguments: https://github.com/astral-sh/ruff/blob/1f07880d5c410bcd5192a1bd20e8f7d59bc4aab4/crates/ruff_python_ast/src/nodes.rs#L3777-L3781

Here you're indexing into the _positional_ arguments

---

_@sharkdp reviewed on 2024-11-18 13:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:51 on 2024-11-18 13:07_

Hm, if this returns `Literal[True]`, an ideal type checker could even detect the `type(x) == A` condition to be statically `True`; and the `type(x) != A` condition to be statically `False`. That wouldn't change anything, but this is not what I wanted to demonstrate in this test. Everything below is correct, even if this returns `bool`, right?

The fact that this method does return literal `True` is just one way to demonstrate what I wanted to show.

---

_@AlexWaygood reviewed on 2024-11-18 13:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:51 on 2024-11-18 13:08_

Ahh, fair point. Consider this suggestion withdrawn!

---

_@sharkdp reviewed on 2024-11-18 13:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:9 on 2024-11-18 13:22_

Hm, okay. I don't really like it though. It adds superfluous and potentially confusing code to the tests (*"why does it say `A | B` but then always return `A`?"*). And I could imagine a (hypothetical) type checker that would issue a diagnostic for code like this (*"Consider using a narrower return type…"*).

---

_@AlexWaygood reviewed on 2024-11-18 13:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:9 on 2024-11-18 13:23_

yeah. Well -- soon enough we'll have inference for function parameter types, and then we can just have a function parameter annotated as `A | B` in most situations like this in our tests :-)

---

_@sharkdp reviewed on 2024-11-18 13:26_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/type.md`:42 on 2024-11-18 13:26_

Thanks. Added a TODO comment.

---

_Comment by @sharkdp on 2024-11-18 13:31_

Ah, the performance regression is probably from removing this (now insufficient) check:
```rs
        if !left.is_name_expr() && comparators.iter().all(|c| !c.is_name_expr()) {
            // If none of the comparators are name expressions,
            // we have no symbol to narrow down the type of.
            return None;
        }
```

I'll look into it.

Edit: fixed now by introducing a similar check.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:359 on 2024-11-18 13:53_

What about something like this. It's a bit more verbose, but it moves some cheaper checks higher up (micro-optimising things), improves type safety a little (by using `let ... else` rather than indexing), and reduces the nesting a bit:

```rs
               ast::Expr::Call(ast::ExprCall {
                    range: _,
                    func: callable,
                    arguments:
                        ast::Arguments {
                            args,
                            keywords,
                            range: _,
                        },
                }) => {
                    if !rhs_ty.is_class_literal() {
                        continue;
                    }

                    if !keywords.is_empty() {
                        continue;
                    }

                    let [ast::Expr::Name(ast::ExprName { id, .. })] = &**args else {
                        continue;
                    };

                    let is_valid_constraint = if is_positive {
                        op == &ast::CmpOp::Is
                    } else {
                        op == &ast::CmpOp::IsNot
                    };

                    if !is_valid_constraint {
                        continue;
                    }

                    let callable_ty =
                        inference.expression_ty(callable.scoped_expression_id(self.db, scope));

                    if callable_ty
                        .into_class_literal()
                        .is_some_and(|c| c.class.is_known(self.db, KnownClass::Type))
                    {
                        let symbol = self
                            .symbols()
                            .symbol_id_by_name(id)
                            .expect("Should always have a symbol for every Name node");
                        constraints.insert(symbol, rhs_ty.to_instance(self.db));
                    }
                }
```

Also, could you maybe add a test for the "only a single argument passed to `type()`, but it's a keyword argument" edge case (that an earlier version of this PR panicked on)?

---

_@AlexWaygood approved on 2024-11-18 13:54_

---

_@sharkdp reviewed on 2024-11-18 14:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:359 on 2024-11-18 14:37_

> Also, could you maybe add a test for the "only a single argument passed to `type()`, but it's a keyword argument" edge case (that an earlier version of this PR panicked on)?

Yes, done.

> What about something like this. It's a bit more verbose, but it moves some cheaper checks higher up (micro-optimising things), improves type safety a little (by using `let ... else` rather than indexing), and reduces the nesting a bit:

I'm not sure. I think I prefer the version with fewer early returns and a more "natural" order of checks. Do you have any evidence that this is faster? Which checks do you consider to be more expensive than others? Even the type-based checks are just lookups of types that have previously been inferred, right?

---

_@AlexWaygood reviewed on 2024-11-18 14:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:359 on 2024-11-18 14:43_

> Do you have any evidence that this is faster? Which checks do you consider to be more expensive than others? Even the type-based checks are just lookups of types that have previously been inferred, right?

no firm evidence, no. My experience working on the Ruff linter, however, has been that checking the structure of the AST is usually much cheaper than looking up cached information in a `HashMap`, which I believe is what both the symbol-table lookup and the `class.is_known()` calls are doing under the hood here.

Still, it might be that for our purposes the difference in performance there is just a rounding error, since red-knot is likely to be significantly slower across the board than a linter that can only do single-file analysis (which is what Ruff currently is). I'm certainly happy to leave it like it is for now and revisit this later if it shows up in profiles.

---

_@sharkdp reviewed on 2024-11-18 15:00_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:359 on 2024-11-18 15:00_

I used your approach and moved a two checks into the match statement

---

_Merged by @sharkdp on 2024-11-18 15:21_

---

_Closed by @sharkdp on 2024-11-18 15:21_

---

_Branch deleted on 2024-11-18 15:21_

---
