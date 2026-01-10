```yaml
number: 1473
title: better inference for generics not fully constrained at their construction
type: issue
state: open
author: carljm
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-11-03T21:58:50Z
updated_at: 2026-01-10T08:59:17Z
url: https://github.com/astral-sh/ty/issues/1473
synced_at: 2026-01-10T11:36:23Z
```

# better inference for generics not fully constrained at their construction

---

_Issue opened by @carljm on 2025-11-03 21:58_

The canonical case of this is an empty list literal, but in everything that follows, we should assume it is generalized to all generic classes, not special-cased for builtin collection types.

We discussed these two code examples:

```py
def a() -> list[int]:
    x = []
    y = x
    return y

def b() -> list[int | str]:
    x = []
    x.append(1)
    x.append("foo")
    return x
```

[Today in both of these cases](https://play.ty.dev/e0499334-8edf-45bb-8533-59ce00b60549) we infer `list[Unknown]` at the list construction, and don't emit any diagnostics on either of these examples. So we avoid false positives by using gradual types, but this is also quite unsound: if the return type of `b` were `list[bytes]`, we still wouldn't emit any diagnostics!

(Similarly, for non-empty list literals without a declared type, we infer the union of `Unknown` with the elements present, which is compatible with the gradual guarantee and avoids false positives, but is similarly unsound with respect to later constraints on the type of the list.)

[Pyright in non-strict mode](https://pyright-play.net/?pyrightVersion=1.1.405&code=CYUwZgBAhgFAlBAtAPggGwJYGcAuBtDAOxwF0AuAWACgJaIAPCAXgjxOrogE9mGO6ATiBwBXAYW7VqoSACN4SVJlwFiEAD4RcA8v1qMWbPQwB0UAA7mQhYDACMcY-TOXrtgERgA9l-eOagsJiEvRAA) effectively does the same thing we do today. In strict mode it adds some diagnostics warning that there's an `Unknown` type (for which the solution is "add an annotation.")

[Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=01e6b81029f45bfde3eb8e95da5d021b) similarly just asks you to add an annotation in the first case, since it has nothing to go on, but in the second case it uses the (first, and only the first) append to infer a type for the list (promoting literal types), so it infers `list[int]` and then errors on the second append, and on the return (because `list[int]` is not assignable to `list[int | str]`.)

[Pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSImMYABKgBQCUNAtAHw1QRwAuA2hHQ8AuogA66GlJr4aAXhp9hE6TVLyZK6QCcYPAK7bJpCRKq1sTVhy68BQmgB8avbaK1TZCpR5mFUxMQwWPQAjIy%2BRAFBIWLguLhxEZI6eoaS%2BCAANCD6PNBwJOSIIADENACq%2BVw86mD66ADG%2BbjocKZY1DRguNoAtqg8APro%2Bn3YMNr0%2BIg0gjzM7C482uIpUroGRt1xAHJjE6s0wPgAvnES2SBkumBQpIQ8uH1QFOUACqS39y4YOAQ0RqtSAAc0MgwgrUIEnKAGUYDAaAALHg8YhwRAAekxN2o90IvRBmOCmMwuEacExQPQoPBLXQmO6vToADdUNBUNhYIDgRAwdoIa0aLhiPTChIyDwka0WCzJnBIZIFHEAMyEUIAJgu6BApxyqGaEDlADFoDAKGgsHgiGRdUA) does not include the "please add an annotation" or "there's an unknown type!" diagnostics that mypy and pyright-strict have on the first example. On the second example, it behaves the same as mypy. Its handling [is not special-cased to builtin collection types or methods, but works for any generic class and its methods.](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSIAxlKnHAAQDCA2gCoC6iAOuvX-Zhhh6qTJgAUcGFDAAaetJgBbRPVYBKbr347itODx6DhlcZp47K9ALxMzF-pUKiJARnUO%2BAJxgA3GKhQAPoALqTEMOKU6iCyIACuIdBwJOSIIADE9ACqSVAQYfRg8eiUSbjoBuhGQkW4XkqoIUHo8UrYMF7i%2BKoQ6CHq9AC0AHz0cCFeWjo%2BIfFevGBcIABybR1T9MD4AL7LPLEgZD5gUKSEIbhKUBRZAAqkJ2fjGDgE9JQVkADm800QFUIPCyAGUYDB6AALEIhYhwRAAegRxyEZ0I9W%2BCJg6ARmFwlDgCM%2B6B%2Bf3KOLqXhEvlQ0FQ2FgHy%2BEF%2BXn%2BFXouGI5JSPDIIUhFSG-i8cABvFsywAzIRXAAmfboEA7OKoMoQfwAMWgMAoaCweCIZGVQA) It [gives up if the list is aliased at all](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSIAxlKnHAAQDCA2gCoC6iAOuvX-Zhhh6qTJgAUcGFDAAaetJgBbRPVYBKbr347itODx6DhlcZp47K9ALxMzF-phv1KDvpUKiJARnVv6AE4wAG4wqFAA%2BgAupMQw4pTqILIgAK5R0HAk5IggAMT0AKoZUBAx9GCp6JQZuOgG6EZCFbgBSqhREeipStgwAeL4qhDoUer0ALQAfPRwUQFaOkFRqQG8YFwgAHI9fQv0wPgAvps8ySBkQWBQpIRRuEpQFAUACqRXN7MYOAQudZAAc1WHQgdUIPAKAGUYDB6AALKJRYhwRAAelRlyEN0IrQBqJg6FRmFwlDgqMo-wgQICILqqJaAREwVQ0FQ2Fgf3QgOBtV4uGIvKyPDIUThdQmoQCcFBvFsmwAzIRvAAmU7oEBHFKoGoQUIAMWgMAoaCweCIZA1QA).

Some characteristics it seemed like we felt were desirable for our future handling of these cases (we didn't list these explicitly, I'm inferring from the direction of the conversation):

1. Errors are attributed to a reasonable location.
2. Hover shows a comprehensible type for the list that helps identify the problem when there is an error.
3. Gradual guarantee: if there is no annotation on the list creation, we shouldn't invent limitations on what you are allowed to put into the list, based on what you happen to put in first.
4. Our approach isn't broken by aliasing.

We focused on two primary potential strategies, which are ultimately two different implementation paths to a similar user experience:

## Back-propagate type contexts

In this approach, we would track in semantic indexing, for each definition, all the later type contexts which may apply to it. So in `a()` we'd track that `x = []` has a later type context from `y = x`, and that that definition of `y` in turn has a type context from `return y` (which is the return annotation). In `b()` we'd track that `x = []` has multiple later type contexts from `x.append(1)`, from `x.append("foo")`, and from `return x`.

So in `a()` we would follow the chain and arrive at the single type context `list[int]`, and thus use that as the type of `x` right at `x = []`.

In `b()` we would unify all three type contexts and arrive at the solution `list[int | str]` which works for all three of them.

If the type contexts for a definition don't unify, we just have to make a best-effort choice (prefer the first context? prefer the "widest" context?), naturally resulting in some diagnostics later on.

One challenge of this approach is avoiding big increase in memory usage from the additional def -> context mappings we'd have to store in semantic indexing. Another challenge is finding the right level of "context" to store in that mapping for all cases. For example, in `x.append(1)` we need the entire statement, not just the `x` expression (we can't traverse up our AST). Similarly in `foo(x)` we'd need the entire call expression.

## Synthesize type variables and build up constraint sets, solve them for the scope

In this approach we'd synthesize a typevar `T` at `x = []` so the type of `x` is `list[T]`, and then as we type-check the body of the function, collect constraints on the type of `T`. So `x.append(1)` would add a `T :> Literal[1]` constraint and `return x` would add a `T == int | str` constraint (due to invariance of `list`). At the end of inferring the scope, we'd solve this constraint set. If it doesn't unify, we'd have to then figure out where to place diagnostics (which would mean that we needed to also collect the range-source of each constraint, and there might be a challenge here with preserving that information when simplifying the constraint set). If it does unify we'd have to record our solution so that hover shows the solution rather than the synthesized typevar.


---

_Label `generics` added by @carljm on 2025-11-03 21:58_

---

_Label `bidirectional inference` added by @carljm on 2025-11-03 21:58_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 00:30_

---

_Comment by @npip99 on 2025-12-30 19:41_

<img width="698" height="70" alt="Image" src="https://github.com/user-attachments/assets/ad573c98-28ef-46a0-a9bb-a7008300c435" />

I mean I don't really need to make a big argument for this but the above screenshot is obviously and clearly unusable for basically any project.

---

There should really be a parameter that enforces the following:

### _Every variable must have a non-Unknown type specified at initialization time_
- If it's an empty array, it should be an error. Explicitly annotate list[Any] if that's what you meant.
- If it's an explicit array [1, 2, 3], or a list comprehension, it should infer the common type.
- No "back propagation" ever, you should be able to deduce the type by reading the file top-to-bottom.

IMO, this should be the default and other options need not even exist, I don't see how the current behavior would ever be useful (No other typechecker to my knowledge does this). But, if `list[X | Unknown]` and "assume array type via what's .add'ed later on" is desired behavior and/or default, that's could still work as long as an option for the above semantics still _exists_. I would never not use such an option.

My suggestion does have the drawback that empty arrays need explicit types. I don't think this is a huge tradeoff as empty arrays are fairly rare. At least, in proper type-safe functional code, everything should be list comprehensions, rarely will you modify an array. Even if you do write a lot of imperative-style array building, it's not worse than the current method, which also requires explicitly annotating `list[X]` at every declaration in order to get type-safe code.

---

_Assigned to @ibraheemdev by @ibraheemdev on 2026-01-09 06:50_

---

_Comment by @AndBoyS on 2026-01-10 08:59_

> <img alt="Image" width="698" height="70" src="https://private-user-images.githubusercontent.com/8939474/531080762-ad573c98-28ef-46a0-a9bb-a7008300c435.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjgwMzQ5MjMsIm5iZiI6MTc2ODAzNDYyMywicGF0aCI6Ii84OTM5NDc0LzUzMTA4MDc2Mi1hZDU3M2M5OC0yOGVmLTQ2YTAtYTliYi1hNzAwODMwMGM0MzUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDExMCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjAxMTBUMDg0MzQzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MzMyZTgzOGEwMTUwMTFkZmIwMGVmMDE4MWUxZGY2MWMyZDkzMTRkYTc1OWIxMWMwYTBhZjQ2MjNhMWZkY2FjMSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.NycMJrwvjVKAOVaZTykMsj1BssKMArgJoEVrGRuiMCo">
> I mean I don't really need to make a big argument for this but the above screenshot is obviously and clearly unusable for basically any project.
> 
> There should really be a parameter that enforces the following:
> 
> ### _Every variable must have a non-Unknown type specified at initialization time_
> * If it's an empty array, it should be an error. Explicitly annotate list[Any] if that's what you meant.
> * If it's an explicit array [1, 2, 3], or a list comprehension, it should infer the common type.
> * No "back propagation" ever, you should be able to deduce the type by reading the file top-to-bottom.
> 
> IMO, this should be the default and other options need not even exist, I don't see how the current behavior would ever be useful (No other typechecker to my knowledge does this). But, if `list[X | Unknown]` and "assume array type via what's .add'ed later on" is desired behavior and/or default, that's could still work as long as an option for the above semantics still _exists_. I would never not use such an option.
> 
> My suggestion does have the drawback that empty arrays need explicit types. I don't think this is a huge tradeoff as empty arrays are fairly rare. At least, in proper type-safe functional code, everything should be list comprehensions, rarely will you modify an array. Even if you do write a lot of imperative-style array building, it's not worse than the current method, which also requires explicitly annotating `list[X]` at every declaration in order to get type-safe code.

As a mypy user this does make sense for me, but ty's approach is about minimizing friction when using untyped code
Feels like there should just be a gradual guarantee and strict mode, the first being used when you have old untyped code, and the strict mode for new code where you don't need Unknown type.

As for the original post, second approach seems more straight-forward, we can even have Unknown in this

```
x = []  # list[Unknown]
x.append(1)  # list[Unknown | int]
```

I like the idea of explaining how type was constructed, maybe we can even have extra action in lsp like "explain type", where ty will show all code that determined the type

---
