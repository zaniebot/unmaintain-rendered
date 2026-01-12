```yaml
number: 13758
title: "[red knot] Fix narrowing for '… is not …' type guards, add '… is …' type guards"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/type-narrowing-updates
created_at: 2024-10-15T08:42:47Z
updated_at: 2024-10-15T20:47:22Z
url: https://github.com/astral-sh/ruff/pull/13758
synced_at: 2026-01-12T15:55:45Z
```

# [red knot] Fix narrowing for '… is not …' type guards, add '… is …' type guards

---

_@sharkdp_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Fix a bug with `… is not …` type guards.
 
  Previously, in an example like
  ```py
  x = [1]
  y = [1]
  
  if x is not y:
      reveal_type(x)
  ```
  we would infer a type of `list[int] & ~list[int] == Never` for `x` inside the conditional (instead of `list[int]`), since we built a (negative) intersection with the type of the right hand side (`y`). However, as this example shows, this assumption can only be made for singleton types (types with a single inhabitant) such as `None`.
- Add support for `… is …` type guards.

closes #13715

## Test Plan

Moved existing `narrow_…` tests to Markdown-based tests and added new ones (including a regression test for the bug described above). Note that will create some conflicts with https://github.com/astral-sh/ruff/pull/13719. I tried to establish the correct organizational structure as proposed in [this comment](https://github.com/astral-sh/ruff/pull/13719#discussion_r1800188105)

---

_Review requested from @carljm by @sharkdp on 2024-10-15 08:42_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-15 08:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-15 08:42_

---

_Label `red-knot` added by @sharkdp on 2024-10-15 08:43_

---

_@sharkdp reviewed on 2024-10-15 08:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is_not.md`:39 on 2024-10-15 08:44_

Ideally, this should be `list[int]`, but I guess that is being tracked elsewhere, so I didn't add a TODO comment.

---

_@sharkdp reviewed on 2024-10-15 08:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/match.md`:17 on 2024-10-15 08:45_

Copied over from [this comment](https://github.com/astral-sh/ruff/pull/13719#discussion_r1800188105) by @carljm 

---

_@sharkdp reviewed on 2024-10-15 08:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:498 on 2024-10-15 08:46_

I can try to do better here, but I'm not sure if it's worth the time.

---

_@sharkdp reviewed on 2024-10-15 08:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:511 on 2024-10-15 08:48_

What rust-analyzer auto-inserted looks so logical, I just kept it :smile:. But seriously, I don't know how to treat `Type::Todo` here. Returning `false` might be another sensible option.

---

_@sharkdp reviewed on 2024-10-15 08:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1568 on 2024-10-15 08:49_

There might be a good reason why `None` was missing so far?

---

_@MichaReiser reviewed on 2024-10-15 08:54_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:511 on 2024-10-15 08:54_

The `Todo` type is semantically identical to `Unknown` except that it indicates that its inference is still missing (helps with debugging). We can add Todo to the `Type::Unknown` branch

---

_Comment by @github-actions[bot] on 2024-10-15 08:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:498 on 2024-10-15 08:57_

We normalize unions containing a single type to just that type in:

https://github.com/astral-sh/ruff/blob/f1205177fd9a323c93be456ea3f193870780645b/crates/red_knot_python_semantic/src/types/builder.rs#L110-L116

I would have to look into e.g. `Never | None` and @AlexWaygood might know the answer but I would expect this to normalize to `Never` which I think is already happening here:

https://github.com/astral-sh/ruff/blob/f1205177fd9a323c93be456ea3f193870780645b/crates/red_knot_python_semantic/src/types/builder.rs#L77-L81

But we could add a test to prove it

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:491 on 2024-10-15 08:58_

I'm not sure if tuples are singletons but my Python knowledge isn't strong enough to link you to any resource

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1568 on 2024-10-15 09:01_

We plan to remove `Type::None` but I don't think this is relevant to the test code changes here

https://github.com/astral-sh/ruff/issues/13670

---

_@MichaReiser approved on 2024-10-15 09:02_

LGTM, but let's wait for at least one review from @AlexWaygood or @carljm to verify that it has the correct semantics

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:498 on 2024-10-15 09:06_

> We normalize unions containing a single type to just that type in:

I saw that, but wasn't sure if there was a guarantee/invariant on the `Type` enum that would enforce this?

> I would have to look into e.g. `Never | None` and @AlexWaygood might know the answer but I would expect this to normalize to `Never`

I think it should normalize to `None`? I'll check and add a test.

---

_@sharkdp reviewed on 2024-10-15 09:06_

---

_@MichaReiser reviewed on 2024-10-15 09:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:498 on 2024-10-15 09:08_

Yeah, reading through `is_subtype` I think `None` is correct.

---

_@sharkdp reviewed on 2024-10-15 09:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:491 on 2024-10-15 09:10_

> I'm not sure if tuples are singletons

My thinking is this: a tuple type is a singleton type if all of its elements are singleton types. There is one and only one inhabitant of the type `tuple[None, Literal[True]]`, for example.

Now that I think about it again, the same is true for the empty tuple. That `!tuple.elements(db).is_empty()` should be removed. There is exactly one inhabitant of the empty tuple type `tuple[]`, namely `()`.

---

_@AlexWaygood reviewed on 2024-10-15 09:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:498 on 2024-10-15 09:14_

> I think it should normalize to `None`? I'll check and add a test.

Yes. Every Python type represents a set of possible runtime values. The `None` type is a set of possible values with length 1 (the only possible runtime value that satisfies the type is the runtime object `None` itself). `Never`, meanwhile, is the bottom type, an empty type: it is a set of possible values with length 0.

The union of those two sets is exactly equal to the set that represents the `None` type; hence `None | Never` simplifies to `None` (and in fact, `T | Never` always simplifies to `T`, for any given type `T`)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:23 on 2024-10-15 09:30_

Hah! The type narrowing is indeed safe here, but this is very unidiomatic Python code (you should never use `is` to compare with an `int` in Python, as it can have surprising effects). Could you possibly add a short comment to that effect?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:489 on 2024-10-15 10:13_

As I mentioned in our call just now, this unfortunately isn't correct. Two tuples are not guaranteed to be the same object in terms of _identity_ (which literally refers to the memory address of the underlying object) even if they compare equal, and even if all of their elements are in fact references to the exact same singletons:

```pycon
>>> x = (False, True)
>>> y = (False, True)
>>> False is False
True
>>> True is True
True
>>> all(x_element is y_element for x_element, y_element in zip(x, y))
True
>>> all(id(x_element) == id(y_element) for x_element, y_element in zip(x, y))
True
>>> id(x)
4378553856
>>> id(y)
4378331520
>>> x is y
False
```

The one exception is the empty tuple, which _is_ always the same object:

```pycon
>>> x = ()
>>> y = ()
>>> id(x)
4386737520
>>> id(y)
4386737520
>>> x is y
True
```

...However... I _thought_ this was guaranteed by the language reference... but now that I check, it appears the language reference [says the opposite](https://docs.python.org/3/reference/expressions.html#parenthesized-forms):

> An empty pair of parentheses yields an empty tuple object. Since tuples are immutable, the same rules as for literals apply (i.e., two occurrences of the empty tuple may or may not yield the same object).

So I'm not sure exactly what we should do here. The empty tuple is commonly used as a sentinel value, so I highly doubt that Python will realistically ever change it so that it is no longer a singleton; the backwards-compatibility implications would be too great, even if this is not guaranteed by the language spec. Perhaps the best course of action would be to treat it as a singleton in our model, but link to the that section of the language spec and say that this isn't strictly guaranteed by the language.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:478 on 2024-10-15 10:15_

This is fine for now, but note that some `Instance` types can also be singletons:
- Instances of `types.EllipsisType` (there is only one, `...`)
- Instances of `types.NotImplementedType` (there is only one, `NotImplemented`)

We could implement this using the `KnownClass` enum -- we could add an `is_singleton` method to that enum. This could be done as as followup PR

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is_not.md`:29 on 2024-10-15 10:16_

You could possibly expand on this comment a little here to explain why the runtime semantics are the way they are

```suggestion
Non-singleton types should *not* narrow the type:
two instances of a non-singleton class may occupy different addresses in memory
even if they compare equal.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:6 on 2024-10-15 10:20_

I'm a little nervous that these tests will break once we add more type checking (`flag` is not defined here). I know it's a bit more boilerplate, but would you be able to do something like this in the tests?

```suggestion
def returns_bool() -> bool:
    return True

x = None if returns_bool() else 1
```

I _would_ suggest simply doing `x = None if bool() else 1`, but unfortunately I think that will also be prone to future breakage. I believe we (accurately) infer `bool()` as returning `Literal[False]` rather than a random instance of `bool`, so in the future we'll probably be able to statically determine that one of the two branches is always taken, which would mean we wouldn't infer a union type anymore. But the same thing doesn't apply for an arbitrary function like `returns_bool()`, where we'll just believe the return type and infer a random instance of `bool` rather than `Literal[True]`.

---

_@AlexWaygood requested changes on 2024-10-15 10:23_

Thanks! This mostly looks good, but there's an important correctness thing we need to fix involving tuples. I also left a couple of minor nitpick comments as well :-)

As well as the [language reference](https://docs.python.org/3/reference/index.html), Brett Cannon's [blog series unravelling syntactic sugar](https://snarky.ca/tag/syntactic-sugar/) is also an invaluable resource for understanding what these operators are doing under the hood. His article unravelling `is` and `is not` is one of the shorter ones, but it's here: https://snarky.ca/unravelling-is-and-is-not/

---

_@AlexWaygood reviewed on 2024-10-15 10:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1568 on 2024-10-15 10:25_

Yeah, this is fine

---

_@AlexWaygood reviewed on 2024-10-15 10:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is_not.md`:39 on 2024-10-15 10:31_

Hmm... I guess I would weakly prefer adding a short `TODO: generics` comment here, so that it's obvious to readers of the test that we are not asserting this type because we believe that it is the best type we can possibly infer here, just because it's the best we can do right now

---

_@AlexWaygood reviewed on 2024-10-15 10:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:491 on 2024-10-15 10:35_

See my comment at https://github.com/astral-sh/ruff/pull/13758#discussion_r1800865188.

> There is exactly one inhabitant of the empty tuple type `tuple[]`

nitpick: the type representing the empty tuple is spelled as `tuple[()]` :-)

---

_@sharkdp reviewed on 2024-10-15 10:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:6 on 2024-10-15 10:51_

That's actually something I noted down and wanted to ask. I just saw `flag` being used undeclared in other tests and copied that, but I also think it's not great. I actually thought about doing something like `… if random.randint(0, 1) else …`, but a better way would probably be to move all of this into a function, where the type checker can not assume anything about the value of the parameter:
```py
def f(x: Literal[1] | None):
    …
```

---

This seems like a common pattern in inference tests, so maybe we can come up with a better/shorter way to write them. I'm thinking something like
```py
x: Literal[1] | None = conjure()
```
where `conjure` would be a special function similar to Rusts `todo!()` — usually called `absurd` in type theory — of (hopefully not illegal) signature:
```py
def conjure[T]() -> T:
    ???
```
that we would somehow inject into those test examples. Not sure if that makes sense.

---

_@AlexWaygood reviewed on 2024-10-15 10:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:6 on 2024-10-15 10:58_

> a better way would probably be to move all of this into a function, where the type checker can not assume anything about the value of the parameter:

Yes, this would definitely be my preference! Unfortunately we're not yet in a place where we can do that (we need to implement #13693 first -- which shouldn't be _too_ tricky, we just haven't got to it yet). We currently infer types from return annotations but not from parameter annotations.

> This seems like a common pattern in inference tests, so maybe we can come up with a better/shorter way to write them. I'm thinking something like
> 
> ```python
> x: Literal[1] | None = conjure()
> ```

Hrm, maybe... there's also advantages to keeping the test snippets as close to real-world Python as possible, though. The wonderful thing about having tests written in Markdown is that they double as living documentation of what our current inference capabilities are, and it's great for other readers of our tests if that documentation is as clear as possible. I think implementing https://github.com/astral-sh/ruff/issues/13693 and then using function parameters for tests after that is probably my preferred solution for now, but I'm definitely open to persuasion!

---

_@sharkdp reviewed on 2024-10-15 11:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:6 on 2024-10-15 11:11_

> there's also advantages to keeping the test snippets as close to real-world Python as possible, though

True. But I would claim that those `x = None if flag else 1` lines are not ideal in terms of their self-documenting property either. Definitely took me a moment to understand what's going on when I first saw them. And we have the problem with defining `flag`. What if we had something like the following pre-defined (never mind the actual implementation), possibly extended to a variadic function, is possible:
```py
def one_of[X, Y](x: X, y: Y) -> X | Y:
    if random.randint(0, 1):
        return x
    else:
        return y
```
This could be used like
```py
x = one_of(1, None)

reveal_type(x)  # Literal[1] | None
```

(I understand that this might not yet be possible with the current state of red knot)

---

_@AlexWaygood reviewed on 2024-10-15 11:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:6 on 2024-10-15 11:15_

I'm definitely open to it! Maybe you could create a followup issue on this for discussion? (I'd definitely want @carljm's input as well, as the chief author of the test framework 😄)

> (I understand that this might not yet be possible with the current state of red knot)

yes, generic functions are definitely a little way off for now 😄

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:23 on 2024-10-15 11:21_

Hm, I understand why you would never want to do `if y is 345`, but doing `if y is x` (do these two variables refer to the exact same object) should be a valid — if obscure — use case?

I changed it to a custom class now, to make sure that this is not about something int-specific.

---

_@sharkdp reviewed on 2024-10-15 11:21_

---

_@sharkdp reviewed on 2024-10-15 11:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:489 on 2024-10-15 11:22_

> Perhaps the best course of action would be to treat it as a singleton in our model, but link to the that section of the language spec and say that this isn't strictly guaranteed by the language.

I did that for now.

---

_@AlexWaygood reviewed on 2024-10-15 11:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:23 on 2024-10-15 11:31_

_Is_ there a use case in Python for checking if two integer objects share the exact same address in memory (which is what the `is` operator does)? If you actually compare something to an integer _literal_, Python emits a `SyntaxWarning` to warn you that this is almost certainly not what you were trying to do, because you should nearly always treat one integer with value `42` the same as any other integer with value `42`:

```pycon
>>> x = 42
>>> x is 42
<python-input-1>:1: SyntaxWarning: "is" with 'int' literal. Did you mean "=="?
  x is 42
True
```

The `is` and `is not` operators are honestly sort of odd. Python attempts to abstract away the details of memory management in most cases, and for most objects it tries to discourage you from relying on details such as whether one object occupies the exact same memory address as another. And yet, the `id()` function is a builtin, and `is` and `is not` are crucial if you're using sentinel values such as `True`, `False`, `None` or `NotImplemented`.

---

_@sharkdp reviewed on 2024-10-15 11:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:23 on 2024-10-15 11:42_

> _Is_ there a use case in Python for checking if two integer objects share the exact same address in memory (which is what the `is` operator does)?

I honestly don't know. But I could imagine some generic code, maybe in a serialization or caching library, that uses `x is y` to actually check whether two objects are the exact same thing. And that generic code might be used with simple integer objects as well. Anyway, I changed it to a custom class now.

---

_@AlexWaygood reviewed on 2024-10-15 11:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:489 on 2024-10-15 11:52_

I've started a discussion on the Python Discourse forum about whether it's time to change the language reference so that this is a guaranteed property of the language: https://discuss.python.org/t/should-we-specify-in-the-language-reference-that-the-empty-tuple-is-a-singleton/67957/1

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:498 on 2024-10-15 12:00_

I think I would probably shorten the comment here to something like

```suggestion
                // A single-element union, where the sole element was a singleton,
                // would itself be a singleton type. However, unions with length < 2
                // should never appear in our model due to [`UnionBuilder::build`]
                false
```

---

_@sharkdp reviewed on 2024-10-15 12:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:478 on 2024-10-15 12:01_

:+1:

Turns out to be a bit harder than I expected, so I'm postponing it to another PR.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:478 on 2024-10-15 12:02_

Could possibly add a short TODO comment here so we don't forget about it

---

_@sharkdp reviewed on 2024-10-15 12:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:6 on 2024-10-15 12:02_

> Maybe you could create a followup issue on this for discussion

will do. I'm keeping the `flag` thing here for now, since it's used in other tests as well; unless you want me to make the `returns_bool()` change above.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:168 on 2024-10-15 12:06_

We could possibly explicitly handle the case of non-singleton `is not` narrowing here now, since we know we will never do anything here:

```suggestion
                    }
                    // Non-singletons cannot be safely narrowed using `is not`
                    ast::CmpOp::IsNot => {}
```

---

_@AlexWaygood approved on 2024-10-15 12:06_

A couple more nits, but this LGTM now overall. Thanks!

---

_@AlexWaygood reviewed on 2024-10-15 12:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:23 on 2024-10-15 12:08_

> But I could imagine some generic code, maybe in a serialization or caching library, that uses `x is y` to actually check whether two objects are the exact same thing.

Sounds like something I'd flag in a code review if I was helping maintain the library ;) anyway, using a custom class is definitely an improvement, thanks!

---

_@MichaReiser reviewed on 2024-10-15 12:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:23 on 2024-10-15 12:09_

Pytest doesn't have this but JS testing frameworks often expose a `expect(a).toBeSame(b)` and the function would do a `is` comparison internally. 

---

_@AlexWaygood reviewed on 2024-10-15 12:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:23 on 2024-10-15 12:14_

Yes, `unittest` has similar helper methods: `assertIs`, `assertIsNot`, `assertIsNone` and `assertIsNotNone`. (Pytest doesn't really need such helpers -- because of the AST rewriting it does before it runs the tests, just `assert x is y` will produce a beautiful traceback with lots of details if the test fails.) I'm not saying `is` comparisons are _useless_, just that `is` comparisons with immutable builtin types such as integers are almost always not really what you want to do, and that it's slightly strange that this functionality is exposed so prominently as builtin functions and syntax.

---

_@AlexWaygood reviewed on 2024-10-15 12:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:6 on 2024-10-15 12:16_

> I'm keeping the `flag` thing here for now, since it's used in other tests as well; unless you want me to make the `returns_bool()` change above.

yeah, find to keep it for now, we can change them all later

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:477 on 2024-10-15 12:21_

Oh! Modules are singletons (at least in our model). Each separate Python module is represented as a separate type in our model.

---

_@AlexWaygood reviewed on 2024-10-15 12:21_

---

_@AlexWaygood reviewed on 2024-10-15 12:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:477 on 2024-10-15 12:28_

(Obviously `types.ModuleType` is not a singleton -- there are multiple different Python modules out there in the wild. But `Type::Module(<the typing module>)` is a singleton, as is `Type::Module(<the functools module>)`.)

---

_@AlexWaygood approved on 2024-10-15 12:31_

---

_Merged by @sharkdp on 2024-10-15 12:49_

---

_Closed by @sharkdp on 2024-10-15 12:49_

---

_Branch deleted on 2024-10-15 12:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:478 on 2024-10-15 13:26_

> Turns out to be a bit harder than I expected, so I'm postponing it to another PR.

After trying to implement this, @AlexWaygood and me agreed to skip support for `EllipsisType`/`NotImplementedType` for now. The reason is that we can't look up the symbol types from `types`, since they are conditionally added only if the Python version is >=3.10: https://github.com/astral-sh/ruff/blob/74bf4b0653c0273af893fe81d31c126b45329ac6/crates/red_knot_vendored/vendor/typeshed/stdlib/types.pyi#L614-L620

---

_@sharkdp reviewed on 2024-10-15 13:26_

---

_@carljm reviewed on 2024-10-15 20:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is_not.md`:39 on 2024-10-15 20:38_

Not important (definitely no need to change), but FWIW my preference would have been to just avoid using a generic type entirely here, and thus avoid the need for the TODO :) There are plenty of non-generic types (e.g. `int`) that would demonstrate precisely the same issue here.

---

_@carljm reviewed on 2024-10-15 20:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:495 on 2024-10-15 20:40_

I'm not sure we'll want to stick with this conclusion, given the response to https://discuss.python.org/t/should-we-specify-in-the-language-reference-that-the-empty-tuple-is-a-singleton/67957/7 -- in particular the fact that `is` comparisons with tuples are now a syntax warning in recent Python versions, and that there are Python implementations (GraalPy) for which the empty tuple is not a singleton. But we can leave it for now, I don't think it's particularly critical either way.

---

_@AlexWaygood reviewed on 2024-10-15 20:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:495 on 2024-10-15 20:42_

yeah. Though strictly speaking the narrowing is still safe on all popular Python implementations, and probably always will be. But probably not worth carving out a special case for, given the language appears to be actively discouraging it now.

---

_@carljm reviewed on 2024-10-15 20:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:508 on 2024-10-15 20:43_

We should (but don't yet) simplify this to just `Literal[True]` in intersection builder.

---

_@carljm reviewed on 2024-10-15 20:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:509 on 2024-10-15 20:44_

This type also cannot occur in our representation, because we normalize to disjunctive normal form (union of intersections), so this would become `(None & None) | (None & int)` and then `None | (None & int)` and then (this step not yet implemented) `None | Never` and thus just `None`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:510 on 2024-10-15 20:47_

We already always normalize this to `(A & B) | (A & C) | (B & B) | (B & C)` -> `(A & B) | (A & C) | B | (B & C)`, which should (though I'm not sure if it would yet?) become `(A & C) | B`, which (if we know `A` and `C` are disjoint -- this part not yet implemented) should become just `B` -- so then would return `true` from this method :)

---

_@carljm reviewed on 2024-10-15 20:47_

---
