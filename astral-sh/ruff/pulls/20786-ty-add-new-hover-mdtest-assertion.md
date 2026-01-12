```yaml
number: 20786
title: "[ty] Add new `hover` mdtest assertion"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - ty
assignees: []
base: main
head: dcreager/hover-mdtest
created_at: 2025-10-09T12:24:30Z
updated_at: 2026-01-05T13:36:38Z
url: https://github.com/astral-sh/ruff/pull/20786
synced_at: 2026-01-12T15:57:09Z
```

# [ty] Add new `hover` mdtest assertion

---

_@dcreager_

This PR adds a new `hover` mdtest assertion. This exercises the same logic as the hover LSP action. It's useful for when `reveal_type` would display a different infered type. (`reveal_type` is not transparent to bidirectional typing, so inserting a `reveal_type` call can sometimes _change_ the type that we infer for an expression!)

I used Claude to help write large parts of this, but I have reviewed each step and the resulting diff in detail.

---

_Review requested from @carljm by @dcreager on 2025-10-09 12:24_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-09 12:24_

---

_Label `internal` added by @dcreager on 2025-10-09 12:24_

---

_Review requested from @sharkdp by @dcreager on 2025-10-09 12:24_

---

_Review requested from @MichaReiser by @dcreager on 2025-10-09 12:24_

---

_Label `ty` added by @dcreager on 2025-10-09 12:24_

---

_Comment by @github-actions[bot] on 2025-10-09 12:26_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-09 12:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-09 12:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/hover.md`:86 on 2025-10-09 12:39_

This example confuses me somewhat, because the introduction to this mdtest suite states that

> You can use the `hover` assertion to test the infered type of an expression

But `dict[str, int]` is _not_ the inferred type of `{}` here. The inferred type of `{}` here _is_ `dict[Unknown, Unknown]` (the answer `reveal_type` gave), but we allow `{}` to be passed into the `x` parameter of `f` because `dict[Unknown, Unknown]` is assignable to `dict[str, int]`, the annotation for the `x` parameter.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/hover.md`:26 on 2025-10-09 12:42_

nit: I feel like the `↓` makes it a little hard to write hover assertions in mdtest, because I'd end up having to copy and paste it every time or open up a Unicode pallette 😄 is there an ASCII character we could use instead? One of `+`, `!` or `|` might work?

---

_@AlexWaygood reviewed on 2025-10-09 12:43_

Interesting! I haven't reviewed the new `ty_test` code thoroughly yet, but would you be able to give some more examples of where the current behaviour of `reveal_type` is problematic?

---

_@sharkdp reviewed on 2025-10-09 13:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/hover.md`:26 on 2025-10-09 13:07_

It is the year 2025, Alex. Let us have some nice things.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/hover.md`:86 on 2025-10-09 13:46_

As of https://github.com/astral-sh/ruff/pull/20635, we're doing bidirectional type checking for call arguments, and using the parameter annotation as the type context. And as of https://github.com/astral-sh/ruff/pull/20523, we're using the type context to infer a more precise type for dictionary literals. So `dict[str, int]` really is the infered type of `{}` here, when it's passed directly in as the argument. But when passing it to `reveal_type`, the parameter annotation is just `_T`, and so we don't have the more precise type context.

That might suggest that we want to propagate the type context through these kinds of inner calls (not just for `reveal_type`), but even then, with bidi checking it's now the case that we can have different infered types for the same expression depending on how its used, and it's useful to have an assertion that cannot influence that type context in any way.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/hover.md`:26 on 2025-10-09 13:48_

Yeah I had worried about that part. I went with the down arrow partly because I agree with David's half-snark, and partly because I thought that it was more important to optimize here for readability than writability, and the down arrow is visually much clearer.

---

_@dcreager reviewed on 2025-10-09 13:49_

---

_@AlexWaygood reviewed on 2025-10-09 14:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/hover.md`:26 on 2025-10-09 14:01_

> more important to optimize here for readability than writability

Hmmm, I'm not sure I agree. _One_ of the best things about mdtest is how readable it makes our tests, but another of the best things about mdtest is how low-friction it is to write new tests. It's so easy to add new assertions to an existing mdtest suite; it doesn't come with nearly as much boilerplate as writing all our unit tests in Rust or even Python would. And I think that's resulted in us writing lots and lots of assertions, and having a very robustly tested type checker -- great things happen when you make it easy to write tests!

---

_@AlexWaygood reviewed on 2025-10-09 14:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/hover.md`:86 on 2025-10-09 14:18_

> That might suggest that we want to propagate the type context through these kinds of inner calls (not just for `reveal_type`)

That sounds like it might make sense to me... It seems quite counterintuitive to me that hovering over the first `[""]` in the playground for this example gives a different type than hovering over the second `[""]`:

```py
from typing import reveal_type

def needs_list_of_strings(x: list[str | None]): ...

def identity[T](x: T) -> T:
    return x

needs_list_of_strings([""])
needs_list_of_strings(identity([""]))
```

https://play.ty.dev/53299738-b406-4e46-ba40-c0240225f0ae

> with bidi checking it's now the case that we can have different infered types for the same expression depending on how its used, and it's useful to have an assertion that cannot influence that type context in any way

Fair enough! The additional code in `ty_test` to make it work also isn't _free_, though -- there's a maintenance burden there, and a small cognitive burden (I now have to think about whether a `reveal_type` assertion or a `hover` assertion will be more appropriate when I'm writing my tests!). I still feel like I'd be interested in a few more examples of where the current `reveal_type` behaviour is causing issues for your generics work.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/hover.md`:26 on 2025-10-09 14:24_

I still prefer the down arrow over an ASCII character for this. It is much clearer in intent, and even if you have to fall back on copy/paste, I don't feel like that would be as much of a deterrent to writing these assertions as you suggest. (I completely agree with your argument when talking about mdtest existing in the first place — the marginal ease of writing these over the previous Rust tests is _huge_! But I don't think that argument applies with the same force when considering something like this.)

---

_@dcreager reviewed on 2025-10-09 14:25_

---

_@AlexWaygood reviewed on 2025-10-09 14:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/hover.md`:86 on 2025-10-09 14:33_

> As of #20635, we're doing bidirectional type checking for call arguments, and using the parameter annotation as the type context. And as of #20523, we're using the type context to infer a more precise type for dictionary literals. So `dict[str, int]` really is the infered type of `{}` here, when it's passed directly in as the argument. But when passing it to `reveal_type`, the parameter annotation is just `_T`, and so we don't have the more precise type context.

Thanks BTW -- I hadn't fully internalised that this was the consequence of those changes! Makes sense.

---

_Comment by @sharkdp on 2025-10-09 14:44_

> That might suggest that we want to propagate the type context through these kinds of inner calls (not just for reveal_type)

Yes, this sounds like something that we're going to want/need anyway

> but even then, with bidi checking it's now the case that we can have different infered types for the same expression depending on how its used, and it's useful to have an assertion that cannot influence that type context in any way.

… but assuming that we *would* already propagate the type context through a `reveal_type` call (which acts like an identity function), we could also solve this use case with `reveal_type`, right? Then there would be no need for hover-assertions anymore? You could then do:

```py
x: list[Literal[1]] = reveal_type([1, 1])  # revealed: list[Literal[1]]
y: list[int] = reveal_type([1, 1])  # revealed: list[int]
```

---

_Comment by @dcreager on 2025-10-09 17:07_

> assuming that we _would_ already propagate the type context through a `reveal_type` call (which acts like an identity function), we could also solve this use case with `reveal_type`, right? Then there would be no need for hover-assertions anymore? You could then do:

Yes, but... :joy: 

The particular example I want to write a (hover) test for is this:

```py
def f[T](x: dict[str, T]) -> None: ...

# TODO: should be dict[str, Unknown]
# ↓ hover: dict[str, T@f]
f({})
```

In a perfect world, I should be able to do this:

```py
# TODO: should be dict[str, Unknown]
# revealed: dict[str, T@f]
f(reveal_type({}))
```

But this example is more complex than the annotated assignments you mention. We don't just need to propagate the type context through from the outer call to the inner call; we also need to solve the specialization inference for both calls simultaneously (`T@f` and `_T@reveal_type`). (Or maybe allow that the inner one can refer to typevars of the outer one?)

I agree that we need to make that work — but it also seemed worth having an escape hatch that lets us test these expressions without relying on all of that machinery working correctly.

Maybe the escape hatch is to write that test over in `ty_ide`, where we already have hover tests that use `<CURSOR>` to indicate where the hover is triggered? But that also seemed less than ideal, since the test wouldn't really be exercising the LSP action; it's a test of our type inference behavior, which seems to properly belong in a `ty_python_semantic` mdtest.

---

_Comment by @dcreager on 2025-10-09 17:09_

> Maybe the escape hatch is to write that test over in `ty_ide`

Or alternatively, maybe the escape hatch is just to hover in the playground and write in the PR description that I did that

---

_Comment by @sharkdp on 2025-10-09 17:21_

I am certainly not opposed to this approach here, but it *does* feel like a rather large addition (judging from the line diff alone) for a seemingly small use case.

> over in `ty_ide`, where we already have hover tests that use `<CURSOR>` to indicate where the hover is triggered

Maybe there is actually interest in porting those tests to your framework here? That would certainly be a strong argument in favor.

---

_Comment by @AlexWaygood on 2025-10-09 17:32_

> I am certainly not opposed to this approach here, but it does feel like a rather large addition (judging from the line diff alone) for a seemingly small use case.

(I'm also a little surprised that mypy, for example, has never needed this feature in its test suite. Mypy has a test framework that's pretty darn similar to mdtest, and a test suite that's much bigger than ours, but it still just uses `reveal_type` nearly everywhere)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/hover.md`:1 on 2025-10-10 06:31_

Should these tests be in `ty_ide`? I'd find it confusing that a change in `ty_ide` can break a test in `ty_python_semantic`. It would probably take me a while to realize that a test in `ty_python_semantic` isn't failing because of a change in `ty_python_semantic` (which would be the first place where I go look), but because of a change in `ty_ide`.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/hover.md`:29 on 2025-10-10 06:36_

I don't have a good suggestion as an alternative, but a downside of the `arrow` is that I'd always have to copy paste it from somewhere because I don't know how to type it on a keyboard. 

---

_@MichaReiser reviewed on 2025-10-10 06:36_

Having the ability to write hover tests in mdtests is nice. However, it adds a bit of confusion: Am I supposed to write a mdtest-hover test or a ty_ide hover test when making changes to hover and why? Do we need both? 

> Maybe there is actually interest in porting those tests to your framework here? That would certainly be a strong argument in favor.

Having the ability to write them in mdtests would certainly be nice. However, they often test for more than just the type (e.g. doc comments). This makes it difficult to replace them entirely unless we support snapshotting the entire hover but that doesn't seem that urgent right now.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/hover.md`:29 on 2025-10-10 06:47_

see https://github.com/astral-sh/ruff/pull/20786#discussion_r2416666750 :-)

---

_@sharkdp reviewed on 2025-10-10 06:47_

---

_@MichaReiser reviewed on 2025-10-10 06:57_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/hover.md`:26 on 2025-10-10 06:57_

> It is the year 2025, Alex. Let us have some nice things.

Shouldn't it be an emoji then? ⬇️


---

_Comment by @Gankra on 2026-01-05 13:32_

Woah I completely missed this PR! I have occasionally briefly pined for this but I think ultimately mdtests end up being the wrong format for hover tests, which want to be much more (inline-)snapshoty (all of this is "ui" and so you want to see any changes to "ui"). The ty_ide tests are getting really unwieldy because from sheer volume of tests shoved in one file, but I think the solution there is just to split off a `tests/` dir with dirs and files.

---

_Comment by @MichaReiser on 2026-01-05 13:36_

>  The ty_ide tests are getting really unwieldy because from sheer volume of tests shoved in one file, but I think the solution there is just to split off a tests/ dir with dirs and files.

Agree, we could even have fixtures discovered similar to what we do in the formatter's cursor tests instead of having to write normal rust tests

---
