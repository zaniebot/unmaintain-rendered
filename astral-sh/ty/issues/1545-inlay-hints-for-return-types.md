```yaml
number: 1545
title: inlay hints for return types
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-14T08:51:44Z
updated_at: 2025-12-09T00:49:22Z
url: https://github.com/astral-sh/ty/issues/1545
synced_at: 2026-01-12T15:54:25Z
```

# inlay hints for return types

---

_@MichaReiser_

Requires https://github.com/astral-sh/ty/issues/128

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:51_

---

_Label `server` added by @MichaReiser on 2025-11-14 08:51_

---

_Comment by @Gankra on 2025-12-08 23:13_

Hmm I wonder if we really need #128. At the start of the issue David suggests a naive approach, and I think that makes perfect sense for the purposes of purely informational inlay hints? Notably it's "safer" to ship just the inlay hint because it has no semantics in the typechecker's inferences. But if double-clickable could help you add missing types to code quickly (or just quickly scan the signature line for info on what this function actually does).

---

_Comment by @AlexWaygood on 2025-12-08 23:17_

Wouldn't it be confusing if we displayed an inlay hint showing that we "knew" what the return type "actually" was, but then inferred the function as returning `Unknown` everywhere from inside the type checker?

---

_Comment by @carljm on 2025-12-08 23:59_

The hardest problem in #128 was always recursion, which is not a problem we could have handwaved away for the LSP, since it would cause crashes, not just wrong results. Now I think we're at a place with our recursion handling where we can do #128 (and @mtshiba is working on it) but I don't think doing it properly is hard enough that it makes sense to do a simpler version just for inlay hints.

The main relevant question (in terms of "naive approach" vs "full approach") is what you'd want to see as the inlay hint on `A.foo` return type here:

```py
class A:
    def foo(self):
        return None

class B(A):
    def foo(self):
        return 1
```

The naive approach would be to show `None` for `A.foo` and `Literal[1]` for `B.foo`. If you bake both of those, then you have a Liskov violation.

If we use this naive approach in #128, then we'll emit a Liskov violation on your untyped code, which is wrong (you may always use `.foo()` in a way that recognizes its implicit return type as `int | None`) -- or if we suppress the Liskov diagnostic, then we'll just make unsound inferences, because `None` is simply wrong as the return type for `A.foo`, since the type `A` includes instances of `B`.

In #128 we plan to infer `Unknown | None` for `A.foo` to avoid this problem.

We could consider having type inference and inlay hints use different inferred return types, but I'd be pretty concerned about that being confusing.

---

_Comment by @Gankra on 2025-12-09 00:32_

> We could consider having type inference and inlay hints use different inferred return types, but I'd be pretty concerned about that being confusing.

Yes, this is what I was considering. It would also make recursion not a problem (a recursive call would just see the actual signature and conclude you return Unknown).

I don't find the Liskov argument that persuasive -- it's already the case that many inlay hints (basically every one that contains Literal) is the wrong one to bake. The bake feature is opportunistically useful but not like, guaranteed useful.

Even when the annotation isn't what you want baked, it still can help you scan the code and understand what you're looking at.

---

_Comment by @carljm on 2025-12-09 00:36_

> It would also make recursion not a problem (a recursive call would just see the actual signature and conclude you return Unknown)

Oh, good point; as long as we don't actually do RTI in type inference, there's no recursion problem.

> I don't find the Liskov argument that persuasive

The Liskov thing wasn't intended as an argument, just an explanation of why the actual RTI types are a little more complex.

The actual argument is "showing a different inlay hint than the return type we will use in type inference is confusing."

---

_Comment by @AlexWaygood on 2025-12-09 00:37_

To me an inlay hint strongly implies "this is the type the type checker has inferred and is going to use to infer types elsewhere". That's just not true if we still infer all unannotated functions as returning `Unknown` in the type checker. I think _I_ would be really confused by this as a ty user

---

_Comment by @Gankra on 2025-12-09 00:45_

Maybe I'm too compiler-brained, because I would never assume a compiler does externally-visible function signature inference... that would be global inference, no one would ever consider doing global inference!

*stares into the middle distance*

---

_Comment by @carljm on 2025-12-09 00:49_

It's only global inference if you _actually_ handle subclass overrides precisely (rather than just inserting some unions with Unknown on inferred return types for methods), or if you treat unannotated functions as implicitly generic if an unannotated parameter flows into an unannotated return type (pyright actually does a version of this, but only for small functions...). Otherwise it's still just fairly naive local inference, just one level deeper.

---
