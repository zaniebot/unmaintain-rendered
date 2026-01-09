---
number: 16798
title: "Expansion to PLC1802 for `len(iterable) == 0` and `>0`"
type: issue
state: open
author: DaniBodor
labels:
  - question
assignees: []
created_at: 2025-03-17T11:07:23Z
updated_at: 2025-03-17T16:01:36Z
url: https://github.com/astral-sh/ruff/issues/16798
synced_at: 2026-01-07T13:12:16-06:00
---

# Expansion to PLC1802 for `len(iterable) == 0` and `>0`

---

_Issue opened by @DaniBodor on 2025-03-17 11:07_

### Summary

[PLC1802](https://docs.astral.sh/ruff/rules/len-test/) currently catches this

Examples 1&2 (currently caught by rule PLC1802)
```py
fruits = ["orange", "apple"]
vegetables = []

if len(fruits):
    print(fruits)

if not len(vegetables):
    print(vegetables)
```

I come across the following a lot as well (potentially in botched attempt to fix the the flagged violation above), which I think should be equally flagged as a violation.

Examples 3&4 (not caught by PLC1802)
```py
fruits = ["orange", "apple"]
vegetables = []

if len(fruits) > 0:
    print(fruits)

if len(vegetables) == 0:
    print(vegetables)
```


Examples 5&6 (recommended formulation)
```py
fruits = ["orange", "apple"]
vegetables = []

if fruits:
    print(fruits)

if not vegetables:
    print(vegetables)
```


EDIT: I added numbers to the examples and the recommended way to formulate it, to make it easier to discuss them below.

---

_Label `question` added by @MichaReiser on 2025-03-17 11:43_

---

_Comment by @MichaReiser on 2025-03-17 11:44_

I think that would be beyond the rule's scope. The only thing the rule tests for is that there's an explicit comparison of the value returned by `len` with some other value. 

Could you tell me more why you think the rule should catch the two examples that you listed?



---

_Comment by @DaniBodor on 2025-03-17 12:42_

The way I interpret this rule is that there is an unnecessary check against the truthiness of the length of an iterable as opposed to the truthiness of the object itself. All 4 examples above do that, albeit in a slightly different way.

A Truthy iterable is one with a length larger than 0, while a Falsey iterable is one with length 0. Therefore, checking whether the length is Truthy or Falsey (example 1/2) is the same as checking whether the length is larger than 0 or not (example 3/4). But both of these are inherently different from the recommendation of checking whether the iterable _itself_ is Truthy or Falsey (examples 5/6). 
By checking the length, you perform one additional unnecessary operation.

I hope my explanation is clear :)

---

_Comment by @MichaReiser on 2025-03-17 13:03_

Oh I see. Yeah, `if len(fruits) > 0:` and `if fruits:` are semantically the same if `fruits` is an iterable. 

However, this is not the concern of this rule. The only thing the rule tests for is that the comparison is explicit vs. relying on the implicit `0 == False` conversion. 

Detecting cases where `len(x)` could be replaced by `if x` would have to be its own rule and requires type-inference (which ruff doesn't support today). 

---

_Comment by @ntBre on 2025-03-17 14:17_

I think it could be worth updating the examples in the [docs](https://docs.astral.sh/ruff/rules/len-test/#example) to show that an explicit comparison also *intentionally* satisfies the rule.

The examples only show the transformation `if len(x)` -> `if x` and don't account for the `if len(x) == 0` case. `if x` is also the current autofix, but it relies on the simple type inference we have now rather than full type inference.

However, the docs also say "You can either remove the call to len or **compare the length against a scalar,**" which leads to Micha's conclusion that this should be a separate rule. This wording follows the [upstream](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/use-implicit-booleaness-not-len.html) rule, so I also think this would have to be a separate rule.

---

_Comment by @DaniBodor on 2025-03-17 14:33_

I don't think that type inference is required, given that according to [Truth Value Testing - Python Docs](https://docs.python.org/3/library/stdtypes.html#truth-value-testing) _any_ object with a zero length is by definition Falsey (and vice versa).

Objects without a defined `__len__` are not covered by this rule anyway.

I do agree with @ntBre that it would probably need to be a separate rule, given that pylint doesn't include that either. Or maybe I'm just barking up the wrong tree here, and should be making my argument to pylint instead :)

---

_Comment by @ntBre on 2025-03-17 14:51_

> I don't think that type inference is required, given that according to [Truth Value Testing - Python Docs](https://docs.python.org/3/library/stdtypes.html#truth-value-testing) _any_ object with a zero length is by definition Falsey (and vice versa).

Interesting, I think we might be stricter on this than pylint because we check that the argument to `len` is a sequence. Calling `len` seems like a pretty good indicator on its own! However, I do think there is a little bit of subtlety around the `__bool__` method, which is probably the reason for our check here:

> `object.__bool__(self)`
Called to implement truth value testing and the built-in operation bool(); should return False or True. When this method is not defined, [`__len__()`](https://docs.python.org/3/reference/datamodel.html#object.__len__) is called, if it is defined, and the object is considered true if its result is nonzero. If a class defines neither` __len__()` nor `__bool__()` (which is true of the [object](https://docs.python.org/3/library/functions.html#object) class itself), all its instances are considered true.

In short, if an object has a `__bool__` implementation, it could return something different from `__len__`, in which case a naive fix for the rule could be wrong. That's the kind of type inference we need for a fully robust rule (checking if the object has defined `__bool__` and/or `__len__`).

But we already do a simple form of this inference in the current rule, so it wouldn't necessarily block a more general rule either.

---

_Comment by @DaniBodor on 2025-03-17 16:01_

I see your point about objects that have a defined `__bool__` that is different from the `__len__` result. My gut feeling is that that is in itself problematic, as that really shouldn't be the case.

As you mention, this is not so much an issue regarding my suggestion, as it is an issue regarding the rule itself, even in the current implementation. A lengthy-Falsey (or nonlengthy-Truey) object would already lead to a different outcome after the ruff fix. Given this consideration, it may not be ideal that PLC1802 is a safe fix, as this could lead to breaking changes in the case described.

---

_Referenced in [pylint-dev/pylint#10281](../../pylint-dev/pylint/issues/10281.md) on 2025-03-17 16:12_

---

_Referenced in [labcc/pylint#8](../../labcc/pylint/issues/8.md) on 2025-04-04 22:06_

---
