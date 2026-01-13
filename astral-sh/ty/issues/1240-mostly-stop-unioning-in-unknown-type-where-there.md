```yaml
number: 1240
title: "(mostly) stop unioning in `Unknown` type where there is no type annotation"
type: issue
state: open
author: DetachHead
labels:
  - type-inference
assignees: []
created_at: 2025-09-23T02:28:53Z
updated_at: 2026-01-13T07:26:30Z
url: https://github.com/astral-sh/ty/issues/1240
synced_at: 2026-01-13T08:22:54Z
```

# (mostly) stop unioning in `Unknown` type where there is no type annotation

---

_@DetachHead_

the current default behavior is unsafe, see https://play.ty.dev/b92b01eb-09da-48ea-8e4b-02812d27a803
```py
a = [1] # inferred as list[Unknown | int]
a.append("i'm not an int") # no error
a.pop() + 1 # runtime crash
```

as mentioned in https://github.com/astral-sh/ty/issues/1211#issuecomment-3322024482:

> I do think we will likely add an option to, or even default to, inferring the type of un-annotated generic container literals without the union with `Unknown`.

---

_Comment by @KotlinIsland on 2025-09-23 02:50_

i believe this case is essential:

```py
class A:
    length = 100  # inferred as Unknown | Literal[100]

def get_length() -> int | None:
    return None

def f(a: A):
    a.length + 1

a = A()
a.length = get_length()  # oops!
f(a)
```

---

_Label `type-inference` added by @sharkdp on 2025-09-23 09:13_

---

_Label `wish` added by @sharkdp on 2025-09-23 09:13_

---

_Comment by @sharkdp on 2025-09-23 09:24_

As a note for readers who are not aware about the full context: our current behavior is very much intentional and documented [here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md) (with an example similar to the one in the second post). This does not mean that we are opposed to future changes or opt-ins to stricter behavior. I just want to emphasize that this is not a bug.

There is a tradeoff between "fewer false positives on *correct* untyped code", and "fewer false negatives on *incorrect* untyped code". And ty currently chooses to lean more towards the former behavior (more so than mypy and pyright).

Also, please note that both issues above *can* be caught by adding a single type annotation (`a: list[int]` and `length: int` / `length: int | None`).

---

_Comment by @DetachHead on 2025-09-23 13:30_

> Also, please note that both issues above _can_ be caught by adding a single type annotation (`a: list[int]` and `length: int` / `length: int | None`).

as i said in https://github.com/astral-sh/ty/issues/1211#issuecomment-3321964618, i don't think this is an optimal solution:

> But doesn’t that mean users who want full strictness will have to annotate *everything* to opt out of this behavior, and therefore can’t rely on type inference at all?

> I realize that people like us who want the type checker to be as strict as possible are in the minority, but this sounds like ty will be a downgrade for us, which is a shame because everything else astral has made has been a massive improvement over the existing solutions.
> 
> If gradual adoption of type safety is the goal, what does that mean for people who complete the “gradual” journey and end up at the same place as us? Their experience will be degraded by being forced to annotate everything, which is a major reason why people don’t like the idea of type checkers in the first place.

i'm glad you guys are open to adding an option to change this behavior, i just figured it was worth re-iterating my stance from the other issue for those that didn't see that converation

---

_Comment by @KotlinIsland on 2025-09-25 00:18_

this case is essential

as this is not `@Todo`, i am assuming that it is intentional, and not incomplete functionality
```py
class C:
    def f(self):
        self  # Unknown
```

---

_Comment by @carljm on 2025-09-25 00:31_

> i am assuming that it is intentional, and not incomplete functionality

It is just incomplete functionality and unrelated to this issue. It doesn't use `@Todo` simply because it dates back to before the existence of that type. There's ongoing work in progress to fix this, it just uncovers some other issues that needed resolution first.

---

_Comment by @ibraheemdev on 2025-11-04 20:51_

The plan outlined in https://github.com/astral-sh/ty/issues/1473 would mean that we don't need to union in `Unknown`, and instead extend bidirectional inference to ensure type-safety at all uses of the collection (or any generic class). However, we may still want a strict option to opt out of that behaviour and error more eagerly.

Also note that our current behavior is somewhat inconsistent in that we only add `Unknown` to collection literals, and not generic class constructors in general. It's not clear we want to change that right now, as it tends to work well in practice, and will eventually be solved by https://github.com/astral-sh/ty/issues/1473 .

---

_Label `bidirectional inference` added by @ibraheemdev on 2025-11-04 20:53_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 00:27_

---

_Label `wish` removed by @carljm on 2025-11-13 00:28_

---

_Removed from milestone `Stable` by @carljm on 2025-12-26 17:40_

---

_Added to milestone `Pre-stable 1` by @carljm on 2025-12-26 17:40_

---

_Label `bidirectional inference` removed by @carljm on 2026-01-08 18:05_

---

_Assigned to @carljm by @carljm on 2026-01-09 15:29_

---

_Comment by @carljm on 2026-01-09 17:44_

Even prior to implementing #1473, we now plan to remove the union-with-Unknown behavior in almost all cases (and not even keep an option to enable it broadly). For now we will still have `list[Unknown]` for `mylist = []`, but that will be fixed by #1473. But `mylist = [1, 2, 3]` will be inferred as `list[int]` (not `list[Unknown | int]`) and an `x = 1` attribute will be inferred as an attribute of type `int` (not `Unknown | int`).

The one case where we plan to keep the union-with-Unknown is for singleton types (most notably `None`). In effect this will be another form of literal-promotion, where we "promote" `None` to `Unknown | None`, because it's very unlikely that you want an instance attribute whose value can only ever be `None`, or a list that can only ever contain `None`. (If you do want that, of course it is still possible via explicit annotation.) We will have an opt-in diagnostic to warn in the cases where this promotion occurs, for those users who want to ensure they don't ever get this `Unknown` inserted.

If there are users reading this who are concerned about this change, or feel that you have use cases that will be negatively affected, please reach out so we can discuss your use case!

---

_Renamed from "add an option to disable `Unknown` type from from being added where there is no type annotation" to "(mostly) stop unioning in `Unknown` type where there is no type annotation" by @carljm on 2026-01-09 17:45_

---

_Comment by @KotlinIsland on 2026-01-10 05:04_

@carljm this sounds really perfect. I'm really glad to see that the decision makers here have the right direction 

---

_Comment by @AndBoyS on 2026-01-10 09:04_

> Even prior to implementing [#1473](https://github.com/astral-sh/ty/issues/1473), we now plan to remove the union-with-Unknown behavior in almost all cases (and not even keep an option to enable it broadly). For now we will still have `list[Unknown]` for `mylist = []`, but that will be fixed by [#1473](https://github.com/astral-sh/ty/issues/1473). But `mylist = [1, 2, 3]` will be inferred as `list[int]` (not `list[Unknown | int]`) and an `x = 1` attribute will be inferred as an attribute of type `int` (not `Unknown | int`).
> 
> The one case where we plan to keep the union-with-Unknown is for singleton types (most notably `None`). In effect this will be another form of literal-promotion, where we "promote" `None` to `Unknown | None`, because it's very unlikely that you want an instance attribute whose value can only ever be `None`, or a list that can only ever contain `None`. (If you do want that, of course it is still possible via explicit annotation.) We will have an opt-in diagnostic to warn in the cases where this promotion occurs, for those users who want to ensure they don't ever get this `Unknown` inserted.
> 
> If there are users reading this who are concerned about this change, or feel that you have use cases that will be negatively affected, please reach out so we can discuss your use case!

It will still be nice to have as an opt-in option for untyped code, since it will be a source of some false positives (from my experience, it will usually come from list of custom objects, like [CustomClass1(), CustomClass2(), ...])

---

_Comment by @carljm on 2026-01-13 02:44_

@AndBoyS can you provide some realistic example code where you think you would use this option?

I have a hard time imagining cases where you'd really want a global option that affects all of your code, rather than just annotating one specific container for which a too-narrow type is being inferred.

(EDIT: I guess you did say "untyped code". #1473 should also remove many of the cases where this would be needed)

---

_Comment by @AndBoyS on 2026-01-13 07:26_

@carljm yeah, I meant module-wise for example

---
