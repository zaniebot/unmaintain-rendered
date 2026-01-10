```yaml
number: 1211
title: support multi-step bidirectional type inference
type: issue
state: closed
author: KotlinIsland
labels:
  - wish
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-09-20T06:40:31Z
updated_at: 2025-11-04T20:45:27Z
url: https://github.com/astral-sh/ty/issues/1211
synced_at: 2026-01-10T02:06:25Z
```

# support multi-step bidirectional type inference

---

_Issue opened by @KotlinIsland on 2025-09-20 06:40_

### Summary

```py
def f1() -> list[object]:
    return [1, 2, 3]  # lovely, bidirectional type inference uses the return type to infer `list[object]`

def f2() -> list[object]:
    result = [1, 2, 3]
    return result  # error: bi-directional type inference does not look ahead, so the inferred type is `list[int]`
```

note that there are two issues preventing the actual error from surfacing:
- return position is not taken into account for constraint solving
- `[1, 2, 3]` is inferred as `list[Unknown | int]`, which is assignable to `list[object]`

---

_Label `generics` added by @AlexWaygood on 2025-09-20 12:01_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-09-20 12:01_

---

_Renamed from "support multi step bi-directional type inference" to "support multi-step bidirectional type inference" by @AlexWaygood on 2025-09-22 12:48_

---

_Label `wish` added by @carljm on 2025-09-22 22:03_

---

_Comment by @carljm on 2025-09-22 22:09_

It looks like pyright/mypy/pyrefly all have the same behavior as ty here. I think it is quite challenging to do what's requested here with good performance. So while I don't disagree it would be very nice, I doubt this will be a priority anytime soon. The workaround is at least simple: explicitly annotate `result: list[object] = [1, 2, 3]`.

---

_Comment by @ibraheemdev on 2025-09-22 22:16_

I don't think the first case is even bidirectional type inference, we infer the list literal independently as `list[Unknown | int]` and then ensure check with `list[object]`.

---

_Comment by @carljm on 2025-09-22 22:17_

@ibraheemdev Yes, you're right. Though I think we should add "return position" as another case where we do use bidirectional inference.

---

_Comment by @carljm on 2025-09-22 22:18_

(Also, in fact ty doesn't currently error on either of these examples, because we prefer `list[Unknown | int]` inference, precisely in order to avoid these kinds of issues. That may change, though.)

---

_Comment by @KotlinIsland on 2025-09-22 23:33_

personally, i can't comprehend the `list[Unknown | int]`, are you planning on any way to disable this? in my opinion it results in the type checker being unusable

---

_Comment by @DetachHead on 2025-09-23 00:19_

Yeah I’m quite concerned about this as well. I remember seeing this behavior mentioned elsewhere.  Correct me if I’m wrong because i can’t find that discussion anymore but it’s for projects gradually adopting a type checker, as in you don’t want hundreds of type errors to be reported on existing code that already works, right? And I believe it was mentioned that there will be a strictness setting to report an error on `Any` / `Unknown` types for people who don’t like this behavior. 

But doesn’t that mean users who want full strictness will have to annotate *everything* to opt out of this behavior, and therefore can’t rely on type inference at all? IMO there are other ways to achieve the same goal without compromising for people who want full type safety. For example [a baseline file](https://docs.basedpyright.com/latest/benefits-over-pyright/baseline/).

I realize that people like us who want the type checker to be as strict as possible are in the minority, but this sounds like ty will be a downgrade for us, which is a shame because everything else astral has made has been a massive improvement over the existing solutions.

If gradual adoption of type safety is the goal, what does that mean for people who complete the “gradual” journey and end up at the same place as us? Their experience will be degraded by being forced to annotate everything, which is a major reason why people don’t like the idea of type checkers in the first place. 

---

_Comment by @carljm on 2025-09-23 00:58_

"Getting lots of false positives on working code" and "having to add annotations for things that seem 'obvious'" are both reasons people dislike Python type checkers; it's difficult to balance the two correctly.

ty aims to be a usable LSP for Python code that is untyped and whose author has no intention to type it or use a type checker at all. Baseline files are not a useful solution for such a user.

I suspect that `list[Unknown | int]` is in practice much more usable than you think it is. The type `Unknown | int` is indistinguishable from the type `int`, when you try to use a value of that type; they are only different when you try to assign something to a destination explicitly annotated as `Unknown | int`. When you pull an element from a `list[Unknown | int]` and try use it in a way that would be an error for an `int`, you will get exactly the same errors as you would if the list were typed as `list[int]`.

The difficulty is that to make this sound we would need to update the type of the list every time you put something new into it (since we will allow you to put non-int things into it, since you never declared that you wanted it limited to ints), and this may be impractical with good performance. For that reason, I do think we will likely add an option to, or even default to, inferring the type of un-annotated generic container literals without the union with `Unknown`.

---

_Comment by @KotlinIsland on 2025-09-23 02:04_

> I suspect that `list[Unknown | int]` is in practice much more usable than you think it is.

> The type `Unknown | int` is indistinguishable from the type `int`

i think that `list[Unknown | int]` is in practice absolutly not what i would ever want, as it is inherantly unsafe. i'm interested in writing safe code, maintaining safe programs, and using tools that provide me with a garuntee of that safety

```py
a = [1]
a.append("i'm not an int")
a.pop() + 1

b = [1]
c: list[object] = b
c.append("im not an int")
```

> we would need to update the type of the list every time you put something new into it

this results in an extreme amount of complication:
```py
a = [1]
b = a
a.append("")
reveal_type(a)  # list[int | str]
reveal_type(b)  # list[int]
```

another example of this `Unknown` injection being completely unviable is:
```py
class A:
    length = 100

def get_length() -> int | None:
    return None

def f(a: A):
    a.length + 1

a = A()
a.length = get_length()  # oops!
f(a)
```

so if i'm interested in having type inference of even simple things like integer literals, i have to forgo all type safety

maybe `ty` isn't the project for me then

seeing as `ty` diverges from all other python type checkers so drastically, i think it would be valuable to include your quote in the readme of the project:

> ty aims to be a usable LSP for Python code that is untyped and whose author has no intention to type it or use a type checker at all.

---

_Comment by @carljm on 2025-09-23 02:07_

> this results in an extreme amount of complication

Yes, which is why I said (in the same paragraph) that it's probably not feasible and we'll likely default to the behavior you're looking for :) still navigating the right balance to meet the needs of lots of different users.

---

_Comment by @KotlinIsland on 2025-09-23 02:11_

> still navigating the right balance to meet the needs of lots of different users.

if there was a `gradual = false` configuration option, then it would resolve `Any` and all issues i have with the current direction

---

_Comment by @DetachHead on 2025-09-23 02:29_

> I do think we will likely add an option to, or even default to, inferring the type of un-annotated generic container literals without the union with `Unknown`.

thanks, that'll address my concerns, so i've opened #1240 for this

---

_Comment by @ibraheemdev on 2025-11-04 20:45_

See https://github.com/astral-sh/ty/issues/1473 for our plans here.

---

_Closed by @ibraheemdev on 2025-11-04 20:45_

---
