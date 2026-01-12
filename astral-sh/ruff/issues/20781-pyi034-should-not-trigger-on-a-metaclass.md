```yaml
number: 20781
title: "`PYI034` should not trigger on a metaclass"
type: issue
state: closed
author: tylerlaprade
labels:
  - bug
assignees: []
created_at: 2025-10-09T08:56:29Z
updated_at: 2025-10-24T13:40:27Z
url: https://github.com/astral-sh/ruff/issues/20781
synced_at: 2026-01-12T15:54:57Z
```

# `PYI034` should not trigger on a metaclass

---

_@tylerlaprade_

### Summary

This is what happens if I follow this rule in a metaclass in Python 3.14

<img width="802" height="112" alt="Image" src="https://github.com/user-attachments/assets/9e5d97ca-caec-4621-8c94-fc7241e7c113" />

### Version

0.14.0

---

_Comment by @ntBre on 2025-10-09 19:07_

Interesting, thanks for the report! I don't think this is 3.14-specific, so it has probably been around for a while. This restriction is in [PEP 673](https://peps.python.org/pep-0673/#valid-locations-for-self), for example. [pyright](https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&reportImplicitOverride=true&reportShadowedImports=true&reportUnusedCallResult=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDKApgDbABQlAxiQIYDO9UAwgBTwJECUAXOVAKgATIsCgB9cSiIB3Saxr0uUALQA%2BQqWA8oAOn1A) flags this simple case:

```py
from typing import Self


class C(type):
    def __new__(cls) -> Self: ...
```

with the error shown above. We have an `is_metaclass` helper function that might be helpful in fixing this.

---

_Label `bug` added by @ntBre on 2025-10-09 19:07_

---

_Comment by @AlexWaygood on 2025-10-15 12:36_

We actually already try not to emit this diagnostic if we can detect that the class is a metaclass: https://github.com/astral-sh/ruff/blob/c6959381f898f4ffd768b7f058ebcf904b489b6c/crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs#L144-L147

@tylerlaprade, would you be able to show us your metaclass's base classes? There are various improvements to our metaclass-detection logic that we _could_ make (#20881, by @danparizher, proposes one of them), but it's hard to know whether any of them would fix the issue you're seeing here without a bit more information.

100% bulletproof detection of metaclasses is unfortunately impossible without multifile type inference, which Ruff does not have and won't have for a while yet.

---

_Label `needs-info` added by @AlexWaygood on 2025-10-15 12:36_

---

_Comment by @tylerlaprade on 2025-10-16 07:38_

@AlexWaygood, my base class is `django.db.models.base.ModelBase`

---

_Comment by @AlexWaygood on 2025-10-16 11:10_

Thanks @tylerlaprade! In that case, #20881 won't fix this, which is sort-of what I suspected ;)

I can think of a few ways in which we could improve our metaclass-detection logic so that we understand your metaclass as being a metaclass:
1. We could add `django.db.models.base.ModelBase` to the list of "known metaclasses" here -- then we'd understand all classes that inherit from that class as being metaclasses: https://github.com/astral-sh/ruff/blob/0cc663efcd0ff2bc81f34ff3a658840f01f2a697/crates/ruff_python_semantic/src/analyze/class.rs#L337-L346
  One the one hand, this might help a lot of people who use Django. On the other hand, it would be the first non-stdlib class that we'd be adding to that list, which feels like a _bit_ of a slippery slope. If we're adding this class to that list, maybe we should do some research and see if there are any other common third-party metaclasses we should be adding to that list?

2. We could introduce another heuristic, either to our `is_metaclass` function or our PYI034 lint logic. It's sort-of obvious just from looking at your class's `__new__` method that the class is a metaclass. Any class with a `__new__` method where all the following conditions are met is almost certainly a metaclass:
   - The `__new__` method has 5 parameters
   - The second parameter is annotated with `str`
   - The third parameter is annotated with a `tuple` type of some kind
   - The fourth parameter is annotated with a `dict` type of some kind
   - The fifth parameter is a keyword-variadic parameter

3. We could add some sort of configuration option (`custom-metaclasses`?) that allows users to tell Ruff explicitly that certain classes are metaclasses, and therefore lints like PYI034 and PYI019 shouldn't be emitted on them.

These options aren't mutually exclusive, of course. @ntBre, what do you think about how to proceed?

---

_Comment by @ntBre on 2025-10-16 16:23_

Thanks Alex, that's very helpful!

(1) does feel like a possible slippery slope to me too, so I'd probably go for (2) or (3). Of those, (2) does seem nice since it could be handled without having to configure anything, assuming most metaclasses meet those criteria. I don't feel too strongly though, a new config option would be fine with me!

Are those annotations in (2) helpful for other tools, like type checkers? I'm thinking about the documentation for the rule, and if we suggest a specific annotation pattern to suppress the error, it might just be easier to use a `noqa`, unless there are other benefits from the annotations.

Does ty have a better way of checking if a class is a metaclass?

---

_Comment by @AlexWaygood on 2025-10-16 16:31_

> Are those annotations in (2) helpful for other tools, like type checkers?

Yes, it's the standard way you'd annotate the `__new__` method for a metaclass. While it's _technically_ possible to have a metaclass that doesn't inherit from `type`, nearly all metaclasses in Python inherit from `type`. And nearly all classes that inherit from `type` have `__new__` methods that mimic the second overload typeshed has for `type.__new__`, and/or have `__init__` methods that mimic the second overload typeshed has for `type.__init__`:

https://github.com/astral-sh/ruff/blob/e64d77278830954a323d227e8f9f714c1d0e4c57/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L274-L283

Note that typeshed there is being sneaky. It's not actually using `typing.Self`, it's using a custom `TypeVar` that's called `Self`, as a cheeky way to satisfy both linters and type checkers simultaneously.

> Does ty have a better way of checking if a class is a metaclass?

Yes, for ty it's trivial -- ty just needs to check if `type` is in a class's MRO to determine if it's a metaclass

---

_Comment by @ntBre on 2025-10-16 16:49_

Awesome, thanks!

I guess I'd lean toward improving our heuristic based on annotations for now, and then hopefully one day we'll inherit the precise check from ty.

---

_Comment by @AlexWaygood on 2025-10-16 16:50_

yup, and we can always add the config option in the future if it turns out that this is still causing too many false positives.

---

_Label `needs-info` removed by @MichaReiser on 2025-10-20 08:13_

---

_Closed by @ntBre on 2025-10-24 13:40_

---
