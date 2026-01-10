```yaml
number: 13801
title: "[red-knot] revert change to emit fewer division by zero errors"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/revert-div-zero-change
created_at: 2024-10-17T18:52:20Z
updated_at: 2024-10-17T20:26:30Z
url: https://github.com/astral-sh/ruff/pull/13801
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] revert change to emit fewer division by zero errors

---

_Pull request opened by @carljm on 2024-10-17 18:52_

This reverts https://github.com/astral-sh/ruff/pull/13799, and restores the previous behavior, which I think was the most pragmatic and useful version of the divide-by-zero error, if we will emit it at all.

In general, a type checker _does_ emit diagnostics when it can detect something that will definitely be a problem for some inhabitants of a type, but not others. For example, `x.foo` if `x` is typed as `object` is a type error, even though some inhabitants of the type `object` will have a `foo` attribute! The correct fix is to make your type annotations more precise, so that `x` is assigned a type which definitely has the `foo` attribute.

If we will emit it divide-by-zero errors, it should follow the same logic. Dividing an inhabitant of the type `int` by zero may not emit an error, if the inhabitant is an instance of a subclass of `builtins.int` that overrides division. But it may emit an error (more likely it will). If you don't want the diagnostic, you can clarify your type annotations to require an instance of your safe subclass.

Because the Python type system doesn't have the ability to explicitly reflect the fact that divide-by-zero is an error in type annotations (e.g. for `int.__truediv__`), or conversely to declare a type as safe from divide-by-zero, or include a "nonzero integer" type which it is always safe to divide by, the analogy doesn't fully apply. You can't explicitly mark your subclass of `int` as safe from divide-by-zero, we just semi-arbitrarily choose to silence the diagnostic for subclasses, to avoid false positives.

Also, if we fully followed the above logic, we'd have to error on every `int / int` because the RHS `int` might be zero! But this would likely cause too many false positives, because of the lack of a "nonzero integer" type.

So this is just a pragmatic choice to emit the diagnostic when it is very likely to be an error. It's unclear how useful this diagnostic is in practice, but this version of it is at least very unlikely to cause harm.


---

_Label `red-knot` added by @carljm on 2024-10-17 18:52_

---

_Review requested from @MichaReiser by @carljm on 2024-10-17 18:52_

---

_Review requested from @AlexWaygood by @carljm on 2024-10-17 18:52_

---

_@AlexWaygood reviewed on 2024-10-17 18:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:29 on 2024-10-17 18:54_

Maybe we should keep this test, but assert the opposite thing, and paste in your great PR description there as a prose introduction to why we're making the assertion the way we are?

---

_Comment by @AlexWaygood on 2024-10-17 18:56_

> The correct fix is to make your type annotations more precise, so that `x` is assigned a type which definitely has the `foo` attribute.

Hmm, this doesn't really apply for the "divide-by-zero" case though, right? There's no way to annotate an int subclass to indicate to the type checker that `__truediv__` never raises `ZeroDivisionError`

---

_Comment by @github-actions[bot] on 2024-10-17 19:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-10-17 19:10_

> Hmm, this doesn't really apply for the "divide-by-zero" case though, right? There's no way to annotate an int subclass to indicate to the type checker that `__truediv__` never raises `ZeroDivisionError`

No, you're right, it doesn't apply precisely, because this isn't expressible in type annotations. But it can still sort of apply, if we just generally decide not to emit the error on subtypes of `int` and `float` (which was the previous behavior.)

The other issue here is that really if we followed the above logic fully, we'd emit a divide-by-zero warning on every `int / int`, because zero is an inhabitant of `int`, so it might raise the error. But this is impractical, because most of the time it won't be zero, and there isn't a "nonzero integer" type available in the Python type system.

So if we go back to the previous behavior, we're really making a pragmatic decision about what is more likely, so we can emit a diagnostic that's really outside the type system. We're saying that "usually" something typed as `int` is an int, and if it's a custom subclass you can reasonably type it as your custom subclass instead, and then we'll just not emit this diagnostic. And usually an integer is not zero, so we won't emit this diagnostic unless we know the divisor is zero for sure.

If we are going to emit divide-by-zero diagnostics at all, I still think that's the right pragmatic tradeoff. But I'm also not sure how _useful_ that is -- if you are dividing by a literal zero, it'll always fail at runtime, and it's likely pretty obvious in the source code as well, so how useful is this diagnostic, really? So all this discussion is also just making me feel tempted to rip out this diagnostic entirely, and just say this is outside the type system. Which I think is the only pedantically correct thing to do here.

---

_Comment by @AlexWaygood on 2024-10-17 19:22_

> But I'm also not sure how _useful_ that is -- if you are dividing by a literal zero, it'll always fail at runtime

Right... but it could be in a branch rarely taken. To invent a silly scenario, it might be a branch that's only taken if the date indicates it's a leap year or whatever. You might accidentally leave in a `1/0` you added while debugging, then get really annoyed when three years later your code crashes because you left it in. (More fool you for having bad test coverage, I guess, but it _would_ be cool if we could catch that!)

So, I guess you have persuaded me that this revert is a good idea, and that the behaviour we had yesterday was a useful one: in situations where the type is inferred as being an `int`, (and not some subclass of `int`), we emit the diagnostic; but if we infer it as being an instance of any subclass of `int`, we do not.

(FWIW, an argument in favour of your position is that neither mypy nor pyright complains about `1 / 0`.)

---

_Comment by @AlexWaygood on 2024-10-17 19:23_

(And I think you are correct that, pedantically, this is outside the type system!)

---

_Comment by @carljm on 2024-10-17 19:54_

I suppose in principle you can use overloads to annotate something like this:

```
@overload
def __truediv__(self, divisor: Literal[0]) -> NoReturn: ...
@overload
def __truediv__(self, divisor: int) -> float: ...
```

But this still wouldn't give a clear diagnostic.

---

_@AlexWaygood reviewed on 2024-10-17 20:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:53 on 2024-10-17 20:00_

I think you need to define `MyInt` (this PR deletes it currently)

---

_@carljm reviewed on 2024-10-17 20:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:53 on 2024-10-17 20:10_

It's defined four lines above here, on line 51.

It doesn't actually implement `__truediv__` to do anything different. I think that's a distraction, since it's irrelevant to the logic we actually implement, which just always silences the diagnostic for all subclasses.

---

_Comment by @carljm on 2024-10-17 20:11_

I would still lean toward "it's not worth emitting this as a diagnostic at all." But I've updated the PR to implement (and explain and test) what I think the best / most pragmatic version of the diagnostic is.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:53 on 2024-10-17 20:11_

> It's defined four lines above here, on line 51.

🤦 sorry, trying to do too many things at once

---

_@AlexWaygood reviewed on 2024-10-17 20:11_

---

_@AlexWaygood approved on 2024-10-17 20:11_

---

_Merged by @carljm on 2024-10-17 20:17_

---

_Closed by @carljm on 2024-10-17 20:17_

---

_Branch deleted on 2024-10-17 20:17_

---
