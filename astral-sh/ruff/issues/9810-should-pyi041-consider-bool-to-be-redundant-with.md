```yaml
number: 9810
title: "Should `PYI041` consider `bool` to be redundant with `int`?"
type: issue
state: closed
author: tylerlaprade
labels:
  - question
assignees: []
created_at: 2024-02-03T20:12:42Z
updated_at: 2025-01-08T10:35:29Z
url: https://github.com/astral-sh/ruff/issues/9810
synced_at: 2026-01-12T15:54:49Z
```

# Should `PYI041` consider `bool` to be redundant with `int`?

---

_@tylerlaprade_

Currently, this rule only flags `int | float` unions as redundant. Coming from other languages, I didn't even realize `int` was a subtype of `float`, but I learned about it because of this rule. However, I found a bug in my code where I was converting `False` to `0.0` - this was because in Python, `bool` is actually an `int`. I found that the tests for this rule very explicitly consider `int | bool` to not be a violation, however this would have prevented me from writing the bug as I did.

<img width="375" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/903c7a95-fcca-40a4-8818-aec5b0488f04">

The `bool` to `int` relation is in some ways stronger than `int` to `float` since `isinstance()` only recognizes the former:
![image](https://github.com/astral-sh/ruff/assets/5475199/f9d68a24-1e62-456d-b9e6-3343d993c429)


---

_Comment by @charliermarsh on 2024-02-05 00:33_

@AlexWaygood - I defer to you on this one.

---

_Label `question` added by @charliermarsh on 2024-02-05 00:34_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-02-05 00:48_

---

_Comment by @zanieb on 2024-02-05 05:42_

I don't think this makes sense to me, they seem more distinct than `int` / `float` regardless of the `isinstance` behavior. I'm curious to hear more opinions though!

---

_Comment by @tylerlaprade on 2024-02-05 13:39_

To me, they're very different in _intended_ usage, but surprisingly related in _actual_ effect, which threw me off, especially since the rule warned about the other scenario and not this one.

---

_Comment by @maxyz on 2024-03-15 10:12_

I just stumbled into this rule, and I think the rule is wrong.

The PEP mentioned in the rule documentation ( [PEP 3141](https://peps.python.org/pep-3141/) ) is talking about the numbers ABCs, these are, numbers.Complex, numbers.Real and numbers.Integral.

float is a particular class that implements numbers.Real, int is a particular class that implements. But they don't have a relationship between them.

Afaik, the numerical hierarchy defined in the pep-3141 can be seen/tested using:
```
import numbers

i = 1
f = 1.0

isinstance(i, int) # True
isinstance(i, float) # False

isinstance(f, int) # False
isinstance(f, float) # True

isinstance(i, numbers.Complex) # True
isinstance(i, numbers.Real) # True
isinstance(i, numbers.Integral) # True

isinstance(f, numbers.Complex) # True
isinstance(f, numbers.Real) # True
isinstance(f, numbers.Integral) # False
```

Am I missing something?

---

_Comment by @AlexWaygood on 2024-03-15 10:19_

> I just stumbled into this rule, and I think the rule is wrong.
> 
> The PEP mentioned in the rule documentation ( [PEP 3141](https://peps.python.org/pep-3141/) ) is talking about the numbers ABCs, these are, numbers.Complex, numbers.Real and numbers.Integral.

The rule isn't wrong @maxyz, but you're right that it's quite misleading for the docs to mention PEP 3141. The rule is premised on the fact that type checkers do not support PEP 3141 (and never will) -- the ABCs introduced by that PEP are wholly incompatible with Python's static-typing system and impossible to annotate in a desirable way in typeshed.

For more, see:
- https://stackoverflow.com/questions/69334475/how-to-hint-at-number-types-i-e-subclasses-of-number-not-numbers-themselv/69383462#69383462
- https://github.com/python/mypy/issues/3186
- https://discuss.python.org/t/numeric-generics-where-do-we-go-from-pep-3141-and-present-day-mypy/17155
- https://github.com/python/typeshed/pull/11353
- https://github.com/python/typeshed/pull/11375

The "numeric tower" that the docs should be linking to is this one here: https://peps.python.org/pep-0484/#the-numeric-tower. (If you think this situation is confusing, you're not alone :)

I'll make a docs PR to improve this situation.

---

_Comment by @MichaReiser on 2025-01-08 09:55_

@AlexWaygood can we close this issue now that you improved the docs?

---

_Comment by @AlexWaygood on 2025-01-08 10:35_

I never addressed @tylerlaprade's original question here!

This rule is specifically targeting the very special case that the typing spec carves out for `int`, `float` and `complex` in Python's typing system. Namely, `int` is not a subtype of `float` and `float` is not a subtype of `complex` (because `int` is not a subclass of `float` at runtime, and `float` is not a subclass of `complex`). But because the duck-type compatibility between the different classes is so strong, type checkers have a special case for these types such that when a user annotates something as `float`, `int`s should also be allwoed for that symbol. For more details, see the in-depth discussion we recently had at https://github.com/astral-sh/ruff/issues/14932.

So we could summarise the `int`/`float`/`complex` special case as "even though they're not subclasses of each other at runtime, they should be treated as if they are in type annotations". The `int`/`bool` situation is sort-of the opposite of this, though. `bool` _is_ a subclass of `int`, but this is often seen as a wart in the language that would be implemented differently if it were being designed from scratch in 2025. Even though `bool` is a subclass and subtype of `int`, many people would prefer to leave the union `int | bool` in their annotations, as `bool` _means_ something quite different to `int` to a human reader.

Another reason why we shouldn't extend the rule to `bool`/`int` is because it's hard to say where we should draw the line. There are lots of classes in Python that you might not realise are subclasses of each other; there are lots of opportunities for people to accidentally write redundant unions in type annotations. For example, some beginners in the language aren't aware that `types.NoneType` (the type of `None`) is a subclass of `object`, and therefore that `object | None` is a redundant union, since `None` is a subtype of `object`. Detecting redundant unions exhaustively without sophisticated type inference is impossible; but if we keep the rule narrowly targeted towards the `int`/`float`/`complex` special case, it's easy to see where the line should be drawn.

---

_Closed by @AlexWaygood on 2025-01-08 10:35_

---
