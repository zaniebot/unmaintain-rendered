```yaml
number: 590
title: add instance attribute for annotated assignment without RHS in method
type: issue
state: closed
author: carljm
labels:
  - attribute access
assignees: []
created_at: 2025-06-06T00:16:24Z
updated_at: 2025-07-02T15:31:13Z
url: https://github.com/astral-sh/ty/issues/590
synced_at: 2026-01-10T02:07:36Z
```

# add instance attribute for annotated assignment without RHS in method

---

_Issue opened by @carljm on 2025-06-06 00:16_

We do not currently infer that `Foo` has an instance attribute `x` for something like this:

```py
class Foo:
    def __init__(self):
        self.x: int

f = Foo()

# Type `Foo` has no attribute `x` (unresolved-attribute)
reveal_type(f.x)  # revealed: Unknown
```

If you're not actually assigning to `self.x`, I don't really know why you'd do this rather than annotating it in the class body, which seems more explicit:

```py
class Foo:
    x: int
```

But [popular libraries such as pytest](https://github.com/pytest-dev/pytest/blob/3e30eae05fcf37880db1db842d022d873cb2166d/src/_pytest/fixtures.py#L399-L407) do this, it appears to be supported by [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=4cd4ceb8f42b1d8342e30df4d50535aa) and [pyright](https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&code=MYGwhgzhAEBiD28BcAoa7oBMCmAzaA%2BgQJYB2xALkQBQTYi4CUqGr0dDAdAB5LRkUUKfAF44iaoxQAnbADdsYEAQoBPAA7ZquHoyA), and it doesn't seem any less safe than annotating an instance attribute in the class body without assigning to it. So I think we should add support for this pattern.

 _Originally posted by @AlexWaygood in [#509](https://github.com/astral-sh/ty/issues/509#issuecomment-2943691503)_

---

_Label `attribute access` added by @carljm on 2025-06-06 00:16_

---

_Comment by @carljm on 2025-06-06 00:17_

One question here might be: should we also support `self.x: int` in a method other than `__init__`?

I guess it probably doesn't matter much in practice, and the most consistent / simplest answer will be "yes".

---

_Comment by @erictraut on 2025-06-06 00:31_

> I don't really know why you'd do this rather than annotating it in the class body

Pyright treats an annotated `self.x` as an instance-only variable whereas the annotated variable in the class body is treated as "an instance variable that can be shadowed by a class variable", so the two have different meanings for pyright. We recently discussed the idea of standardizing the treatment of `x: int` in a class body as an instance-only variable if it isn't assigned a value, but that's different from how pyright has historically treated this. I'm open to that idea, but I'd like to see it fleshed out and formalized in the typing spec before I make such a change in pyright. It will potentially cause backward compatibility pain for some pyright (and mypy) users, so I wouldn't want to make the change and then fail to reach a consensus on the standard.

> One question here might be: should we also support self.x: int in a method other than __init__? I guess it probably doesn't matter much in practice, and the most consistent / simplest answer will be "yes".

I think that's the answer all of the other type checkers arrived at as well. This would be another thing that would be good to formalize in the spec.

---

_Comment by @ilius on 2025-06-06 04:31_

> One question here might be: should we also support `self.x: int` in a method other than `__init__`?

Yes, specially if that method is called in `__init__`.

But `__init__` should be prioritized.
I think `mypy` prioritizes the method that comes first (not runs first), and not `__init__` and I think that's a bug.

For example this code:
```python
class A:
	def initVars(self):
		self.a = None

	def __init__(self):
		self.a: str | None

	def getA(self) -> str | None:
		return self.a
```

Running `mypy instance-attr.py --check-untyped-defs` gives:

```
instance-attr.py:6: error: Attribute "a" already defined on line 3  [no-redef]
Found 1 error in 1 file (checked 1 source file)
```



---

_Comment by @ilius on 2025-06-06 04:38_

I mean if an attribute is declared or assigned in `__init__`, it should not try to infer type of that attribute from assignments in other methods, even if they come before `__init__`.

If the attribute is **declared twice**, you can do one of these

- Ignore the type in non-`__init__` one, or the second one if neither are `__init__`.
- Check if the types match.
- Give error like `mypy`.



---

_Comment by @carljm on 2025-06-06 16:46_

Unlike mypy, our model here doesn't depend at all on ordering of methods.

I also don't think we will implement a prioritization of methods.

The rule is this: if the attribute is never declared, its type is the union of all observed RHS values assigned to it, plus `Unknown` (the latter representing that since it is not declared, external assignments of some other type are also valid.)

If the attribute is declared, that is its type, and all RHS values assigned to it must be assignable to that type.

If the attribute is declared multiple places, all of them must declare the same type, or its an error.

---

_Comment by @sharkdp on 2025-07-02 15:31_

Closing this as a duplicate of #698 because that has some more information on how to implement it.

---

_Closed by @sharkdp on 2025-07-02 15:31_

---
