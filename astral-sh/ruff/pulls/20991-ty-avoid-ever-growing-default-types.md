```yaml
number: 20991
title: "[ty] Avoid ever-growing default types"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-1402
created_at: 2025-10-20T11:56:36Z
updated_at: 2025-12-14T15:18:04Z
url: https://github.com/astral-sh/ruff/pull/20991
synced_at: 2026-01-12T15:57:14Z
```

# [ty] Avoid ever-growing default types

---

_@sharkdp_

## Summary

We currently panic in the seemingly rare case where the type of a default value of a parameter depends on the callable itself:
```py
class C:
    def f(self: C):
        self.x = lambda a=self.x: a
```

Types of default values are only used for display reasons, and it's unclear if we even want to track them (or if we should rather track the actual value). So it didn't seem to me that we should spend a lot of effort (and runtime) trying to achieve a theoretically correct type here (which would be infinite).

Instead, we simply replace *nested* default types with `Unknown`, i.e. only if the type of the default value is a callable itself.

closes https://github.com/astral-sh/ty/issues/1402

## Test Plan

Regression tests


---

_Label `ty` added by @sharkdp on 2025-10-20 11:56_

---

_Comment by @github-actions[bot] on 2025-10-20 14:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-20 14:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-10-20 15:00_

---

_Review requested from @carljm by @sharkdp on 2025-10-20 15:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-20 15:00_

---

_Review requested from @dcreager by @sharkdp on 2025-10-20 15:00_

---

_Comment by @AlexWaygood on 2025-10-20 16:58_

> Types of default values are only used for display reasons,

We do also check whether the type of the default value is assignable to the type of the annotation (if there is an annotation), right?

It looks like we still emit an error for something like this on your branch, but it might be worth adding a test for it:

```py
class A:
    def __init__(self: "A"):
        def f(a: int = self.f): ...
        self.f = f
```

---

_Comment by @sharkdp on 2025-10-20 18:01_

> We do also check whether the type of the default value is assignable to the type of the annotation (if there is an annotation), right?

Yes, correct. Type inference for the actual default value did not change. And we still perform that check. Only the type inside the generated signature changes.

---

_Comment by @sharkdp on 2025-10-20 18:05_

> but it might be worth adding a test for it

Done

---

_Comment by @MichaReiser on 2025-10-21 11:31_

> We currently panic in the seemingly rare case where the type of a default value of a parameter depends on the callable itself:

Is this a too-many-iterations panic?

---

_Comment by @sharkdp on 2025-10-21 11:32_

> > We currently panic in the seemingly rare case where the type of a default value of a parameter depends on the callable itself:
> 
> Is this a too-many-iterations panic?

Yes

---

_@MichaReiser approved on 2025-10-21 11:39_

I don't have a good sense for whether there's a better solution but i agree with the sentiment, that it's probably not worth spending too much time on. 

I was worried that it could regress signature completions in the LSP but this isn't the case

---

_Label `bug` added by @MichaReiser on 2025-10-21 11:39_

---

_Comment by @AlexWaygood on 2025-10-21 12:47_

I think the _better_ solution (which David gestures at in his PR description) would be not to store the type of the parameter default as part of the function signature at all. I think our current UX here is somewhat bad: if you have a function like this in your source code:

```py
def f(x: bool = True) -> None: ...
```

Why do we display the signature on hover as `(x: bool = Literal[True]) -> None`? That honestly just seems incorrect -- it should just be `(x: bool = True) -> None`, as it is in the source code (that's what pyright does -- try hovering over `print` in VSCode). This implies that rather than storing the default value's type in the signature, we should ideally just grab the slice of the source code that the parameter default occupies and store it as a `Box<str>` in the function's signature.

This PR doesn't regress UX, so I'm okay with it as a temporary measure. But it does also add _some_ additional complexity to our codebase (by introducing a new `TypeMapping` variant), and I do wonder how hard it would be to just implement the "better solution" (which feels like it would also solve a significant UX painpoint).

---

_@AlexWaygood approved on 2025-10-21 12:47_

---

_Comment by @sharkdp on 2025-10-21 12:58_

> This implies that rather than storing the default value's type in the signature, we should ideally just grab the slice of the source code that the parameter default occupies and store it as a `Box<str>` in the function's signature.

That's not ideal either, I think. Consider:
```py
def outer(x: int):
    def inner(y: int=x):
        pass
```

Do you want to see a default value of `x` there? I think what we could do instead would be to see if the default type is a `Literal[…]` type (that could be generalized to single-value types probably). If so, we turn it into a value and show `param = True` instead of `param = Literal[True]`. If not, we simply show `param = ...`. What do you think?

I can attempt to implement that instead of this PR.

---

_Comment by @AlexWaygood on 2025-10-21 13:01_

> Do you want to see a default value of `x` there?

Not sure I understand the example -- none of the parameters have default values in this snippet? Did you mean `y=x` rather than `y: x`?

---

_Comment by @sharkdp on 2025-10-21 13:09_

> Not sure I understand the example -- none of the parameters have default values in this snippet? Did you mean `y=x` rather than `y: x`?

yes, fixed

---

_Comment by @AlexWaygood on 2025-10-21 13:10_

> I think what we could do instead would be to see if the default type is a `Literal[…]` type (that could be generalized to single-value types probably). If so, we turn it into a value and show `param = True` instead of `param = Literal[True]`. If not, we simply show `param = ...`. What do you think?

I don't think this will do a great job for things like `tuple.index`, where `sys.maxsize` (a value that varies depending on the specific build of Python you're running) is used as the parameter default:

https://github.com/astral-sh/ruff/blob/e1cada1ec30d289acb00390058fab8bce765836e/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L2739-L2743

And what about parameter defaults that typeshed faithfully gives to us in octal, so that they will be nicely readable for IDE users?

https://github.com/astral-sh/ruff/blob/e1cada1ec30d289acb00390058fab8bce765836e/crates/ty_vendored/vendor/typeshed/stdlib/dbm/dumb.pyi#L57

While I don't think it ever occurs in typeshed, I think things like `x=(foo if some condition else bar)` are also not unusual in user code, and it's probably useful to have the full condition printed in the on-hover signature so that you can see how the default value changes under certain conditions.

---

_Comment by @AlexWaygood on 2025-10-21 13:15_

A compromise that might work quite well: if the function comes from a stub file, just grab a string slice from the source code (typeshed, for example, actually has pretty strict lint rules to stop us from using arbitrary expressions in default values; we just use `=...` for anything complicated at runtime). If it's not a stub file, we can implement your `Literal` heuristic, to avoid us possibly printing strange default values that reference global variables the user has no context on.

---

_Comment by @MichaReiser on 2025-10-21 14:15_

The issue isn't just about values the user doesn't have control over. It also means that comments, or line breaks within the default value are preserved. 

I'm not sure if this is worth prioritizing right now. It seems there are enough details at play that are worth considering in a separate PR (and later)

---

_Comment by @AlexWaygood on 2025-10-21 14:22_

> I'm not sure if this is worth prioritizing right now. It seems there are enough details at play that are worth considering in a separate PR (and later)

Yes, it does seem like pyright/pylance is doing something _quite_ sophisticated here. For this function:

```py
def f(x=[
    1,
    2,
    # some comment,
    4
]): ...
```

Pyright/pylance gives this signature when I hover over it:

<details>
<summary>Screenshot</summary>

<img width="1306" height="410" alt="image" src="https://github.com/user-attachments/assets/acf89dc2-f23f-48dd-9d6a-c793b7fe8347" />

</details>

But pyright/pylance doesn't do a _great_ job for the `sys.maxsize` or octal defaults that I showed from typeshed above (the `sys.maxsize` defaults just become `...`, and the `0o666` default becomes `438`), so there's arguably room for improvement over what pyright/pylance does too.

---

_Comment by @sharkdp on 2025-10-21 17:13_

Seeing this discussion, I feel like this change should be done separately, and probably at a later time. I'd love to work on this, but it's honestly not high priority. And it seems like there will be some things to decide. So I'm going to merge this (because solving that panic *is* a high priority), even if I fully agree with Alex's comment above. It looks to me like it'll be easy to rip this out once we switch to the value-based representation. I'll write down a todo task for me and/or create a ticket.

---

_Merged by @sharkdp on 2025-10-21 17:13_

---

_Closed by @sharkdp on 2025-10-21 17:13_

---

_Branch deleted on 2025-10-21 17:13_

---

_Comment by @AlexWaygood on 2025-10-21 18:17_

Thank you!

---
