```yaml
number: 1515
title: Extra Unknown for attribute in method
type: issue
state: closed
author: emillon
labels:
  - typing semantics
assignees: []
created_at: 2025-11-10T14:27:28Z
updated_at: 2025-12-26T18:32:08Z
url: https://github.com/astral-sh/ty/issues/1515
synced_at: 2026-01-10T01:56:40Z
```

# Extra Unknown for attribute in method

---

_Issue opened by @emillon on 2025-11-10 14:27_

### Summary

Hi,

With the following code ([playground link](https://play.ty.dev/2ee27f97-7ad8-4f4e-b22a-defeae8bf51f)):

```py
class A: pass
class B: pass

class C:

    def __init__(self, x:A|B):
        self.x = x
        reveal_type(self.x) # A | B

    def m(self) -> str:
        reveal_type(self.x) # Unknown | A | B
        match self.x:
            case A():
                return "A"
            case B():
                return "B"
```

The type of `self.x` is correctly revealed as `A|B` at the end of `__init__`. But at the beginning of `m`, it's revealed as `Unknown|A|B` which causes the `match` to be partial and an implicit return to be considered.
How can I convince `ty` that this is correct?

Things I've tried that work:
- adding `x: A|B` as an annotation in `C`, but I'd like to avoid that as our code style currently has annotations in `__init__` arguments and I'd like to avoid duplicating these

Things I've tried that don't work:
- adding `case _: assert_never(self.x)`
- making `C` `@final`
- annotating `self: "C"` in `m`

Thanks!

### Version

alpha 25

---

_Label `question` added by @sharkdp on 2025-11-10 15:12_

---

_Comment by @sharkdp on 2025-11-10 15:16_

The implicit instance attribute `x` on `C` is unannotated, which means that ty would not catch an external mutation of that attribute on an instance of `C`. Consider:
```py
c = C(A())
c.x = "something else entirely"
print("The method returns: " + c.m())  # would lead to an error at runtime
```
So adding `Unknown` to the union here is correct in this sense. It tells you that `self.x` inside `m` could be `A`, `B`, or something unknown.

To fix this, you can either add an annotation in the body of `C`, as you've mentioned, or add an annotation to the assignment directly:
```py
    def __init__(self, x: A | B):
        self.x: A | B = x
```

Once you do that, we would also catch the invalid external mutation.

Note: Inside the `__init__` method, the type is shown as `A | B`, as it is narrowed by the assignment. Inside `__init__`, we can tell for sure that `self.x` is of type `A | B`.

---

_Comment by @emillon on 2025-11-10 15:39_

Thanks for the clear explanation.

I have a followup question regarding recommended style for these kinds of annotations.

I believe that with `mypy`, you can either annotate `__init__` argument or provide a PEP526 annotation and it will be equivalent (the posted snipped works with no diagnostic in `mypy`). Combine that with `ruff`'s [`ANN001`](https://docs.astral.sh/ruff/rules/missing-type-function-argument/) and it will ensure that attributes are well typed.

The distinction that `ty` makes is sensible because of the dynamic nature of python, but I wonder if there's a recommended way to check that all attributes are annotated, without having to repeat ourselves between the PEP526 annotation and the `__init__` arguments (that's more of a `ruff` question I suppose).

Thanks

---

_Comment by @sharkdp on 2025-11-10 15:50_

> I believe that with `mypy`, you can either annotate `__init__` argument or provide a PEP526 annotation and it will be equivalent (the posted snipped works with no diagnostic in `mypy`).

Correct, other type checkers infer the attribute type from the parameter type. See also the comment in [this test](https://github.com/astral-sh/ruff/blob/8d1efe964a36f44cf6b14ef888a57ba145cb8d0e/crates/ty_python_semantic/resources/mdtest/attributes.md?plain=1#L31-L36) and the previous discussion in https://github.com/astral-sh/ruff/issues/15960.

> The distinction that ty makes is sensible because of the dynamic nature of python

I don't think our decision here is final, but I'm happy to see that you seem to acknowledge it.

> I wonder if there's a recommended way to check that all attributes are annotated, without having to repeat ourselves between the PEP526 annotation and the `__init__` arguments.

We plan to have a way to surface `Unknown` types to the user, such that annotations can be added to prevent them.

> (that's more of a `ruff` question I suppose)

As to whether or not ruff can help with this "right now", I'm not sure (cc @ntBre @amyreese).

---

_Comment by @AlexWaygood on 2025-11-10 15:58_

huh, I assumed there'd be something in https://docs.astral.sh/ruff/rules/#flake8-annotations-ann about this, but those rules only seem to tackle missing annotations for function parameters and returns at the moment

---

_Comment by @emillon on 2025-11-14 10:17_

> > The distinction that ty makes is sensible because of the dynamic nature of python
> 
> I don't think our decision here is final, but I'm happy to see that you seem to acknowledge it.

More precisely: I find that `ty`'s behavior matches python semantics more closely than `mypy` is, so this is good in terms of correctness. But in terms of developer experience, I'd expect that this dynamic aspect (attributes with dynamic type) would be the exception rather than the norm. But it looks like PEP526 annotations get us there.

> huh, I assumed there'd be something in https://docs.astral.sh/ruff/rules/#flake8-annotations-ann about this, but those rules only seem to tackle missing annotations for function parameters and returns at the moment

Yes, it looks like PEP526 annotations could subsume the corresponding parameter annotations on `__init__` (so that ANN001 does not trigger when they're absent), but maybe that's a bit too magical.

---

_Comment by @carljm on 2025-11-14 17:34_

Early on we had discussed the possibility of adding a special case that when an `__init__` parameter is annotated, and that parameter is assigned directly to an instance attribute in `__init__`, we would consider that instance attribute to be annotated (declared). This would be a little bit weird in that it would be sort of arbitrarily limited: if your `__init__` body becomes more complex, you might suddenly and unexpectedly start to get `Unknown` unioned in, when it wasn't before. But it would also give more intuitive behavior for a lot of common cases where adding the annotation on the attribute looks redundant. And I think wouldn't really violate the gradual guarantee since it still depends on the annotation existing on the `__init__` parameter.

---

_Comment by @carljm on 2025-11-14 17:35_

I'm going to put this issue into stable release because I do think we should consider ways to reduce the number of cases where we union `Unknown` into an attribute type (or even consider removing the behavior altogether.)

---

_Added to milestone `Stable` by @carljm on 2025-11-14 17:35_

---

_Label `question` removed by @MichaReiser on 2025-11-24 08:18_

---

_Label `typing semantics` added by @MichaReiser on 2025-11-24 08:18_

---

_Comment by @bradleyharden on 2025-12-25 20:09_

Here is another example to consider:

```python
from typing import Self, overload

class Block:
    pass

class IntField:

    @overload
    def __get__(self, block: None, block_type: type, /) -> Self: ...

    @overload
    def __get__(self, block: Block, block_type: type, /) -> int: ...

    def __get__(self, block: Block | None, block_type: type, /) -> int | Self:
        return 42 if block is None else self
    
class Foo(Block):
    a: IntField = IntField()
    b = IntField()

Foo.a  # IntField
Foo.b  # Unknown | IntField

foo = Foo()
foo.a  # int
foo.b  # Unknown | int
```

I was eager to try out `ty`, because of both speed and your support for intersection types, but this is probably a deal breaker for me. I have a library that makes heavy use of descriptors, and I really don't want to force users to duplicate the descriptor name for every. single. field.

To me, it seems like `IntField` is the desired annotation for `Foo.b` in the vast majority of cases. And if you truly do plan to reassign `Foo.b`, couldn't you manually annotate it as such? Why force explicit annotations for the common case when it's easy enough to opt out? Just my two cents.

---

_Comment by @carljm on 2025-12-26 17:42_

@bradleyharden thanks for the feedback. This is the conclusion I've been reaching as well; I think the gradual-guarantee approach is theoretically interesting but doesn't serve most users best in practice. See #1240 where we'd already planned to provide an option to disable this (and I think we should probably make that the default behavior as well.)

---

_Removed from milestone `Stable` by @carljm on 2025-12-26 17:42_

---

_Added to milestone `Pre-stable 1` by @carljm on 2025-12-26 17:42_

---

_Comment by @carljm on 2025-12-26 18:00_

The reason this issue is separate from #1240 was to track the idea mentioned above that we might special-case assignments to un-annotated implicit instance attributes directly from annotated `__init__` arguments. But I think this special-casing would be too narrow and fragile. It would result in small changes to code in an `__init__` method suddenly moving an instance attribute from "not widened with Unknown" to "widened with Unknown", probably surprisingly.

So I am going to close this in favor of the more general plan in #1240 

---

_Closed by @carljm on 2025-12-26 18:00_

---

_Comment by @bradleyharden on 2025-12-26 18:32_

@carljm, thanks for the extra context. I hadn't come across those issues on my own. And thanks for grappling with these issues. It seems quite difficult to add a type system to a language after the fact. There are a lot of less-than-ideal tradeoffs.

---
