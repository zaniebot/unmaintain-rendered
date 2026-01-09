---
number: 16297
title: "RUF045 added in Ruff 0.9.7 should ignore `ClassVar` fields"
type: issue
state: open
author: kkom
labels:
  - rule
assignees: []
created_at: 2025-02-21T08:35:27Z
updated_at: 2025-03-12T08:07:49Z
url: https://github.com/astral-sh/ruff/issues/16297
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF045 added in Ruff 0.9.7 should ignore `ClassVar` fields

---

_Issue opened by @kkom on 2025-02-21 08:35_

### Description

According to Python's docs class variables should be ignored by the dataclass mechanism: https://docs.python.org/3/library/dataclasses.html#class-variables

Which means that I think they don't need an explicit type hint in cases like this:

```python
@dataclass
class MyBaseClass:
    class_var_field: ClassVar[int]

@dataclass
class MySubClass(MyBaseClass):
    class_var_field = 7
```

But ruff now catches `class_var_field = 7` as a lint violation.

---

_Referenced in [astral-sh/ruff#16299](../../astral-sh/ruff/pulls/16299.md) on 2025-02-21 10:30_

---

_Label `bug` added by @dylwil3 on 2025-02-21 12:21_

---

_Comment by @MichaReiser on 2025-02-21 15:05_

I'm not very familiar with data classes but I'm not sure if this is a bug. It does seem to me that the `MySubClass.class_var_field` and `MyBaseClass.class_var_field` are two different fields. 

```py
>>> from typing import ClassVar
... from dataclasses import dataclass
...
... @dataclass
... class MyBaseClass:
...     class_var_field: ClassVar[int]
...
... @dataclass
... class MySubClass(MyBaseClass):
...     class_var_field = 7
...     other_field: int = 8
...
>>> MyBaseClass.class_var_field
Traceback (most recent call last):
  File "<python-input-34>", line 1, in <module>
    MyBaseClass.class_var_field
AttributeError: type object 'MyBaseClass' has no attribute 'class_var_field'
>>> MySubClass.class_var_field
7
```

But I might be wrong here. Even if that's not the case. I still think that we want to warn here because it may, after all, have been the intention to create an instance variable here. 

---

_Comment by @InSyncWithFoo on 2025-02-21 15:30_

@MichaReiser The point of this rule is to avoid attributes that look like dataclass fields but are not fields:

```pycon
>>> from typing import ClassVar
>>> from dataclasses import dataclass
>>>
>>> @dataclass
... class MyBaseClass:
...     class_var_field: ClassVar[int]
...
>>> @dataclass
... class MySubClass(MyBaseClass):
...     class_var_field = 7  # Not part of the generated `__init__`
...     other_field: int = 8
...
>>> help(MySubClass.__init__)
Help on function __init__ in module __main__:

__init__(self, other_field: __dataclass_type_other_field__ = 8) -> __dataclass___init___return_type__
    Initialize self.  See help(type(self)) for accurate signature.
```

`ClassVar` annotations, inherited or not, help clarifying the intention of the attribute. It signifies that the user is already aware of the attribute and so the attribute should not be reported.

---

_Comment by @MichaReiser on 2025-02-21 16:14_

The part I don't see his how inheritance is relevant here. I can remove the base class and get the exact same fields for `MySubClass`:

```
... @dataclass
... class MySubClass:
...      class_var_field = 7
...      other_field: int = 8

__init__(self, other_field: int = 8) -> None
    Initialize self.  See help(type(self)) for accurate signature.
```


---

_Comment by @InSyncWithFoo on 2025-02-21 16:18_

@MichaReiser Inheritance is indeed not the point.

* The rule prevents accidental class variables.
* To avoid this, the user can make their intention explicit by adding `ClassVar`.
* `ClassVar` annotations are inheritable in the type system, so an inherited one should be deem equally explicit.


---

_Comment by @MichaReiser on 2025-02-24 13:13_

I talked to @AlexWaygood because I couldn't find any reference for how type checkers handle `ClassVar` for "inherited" class attributes and he confirmed what you both point out, they handle such attributes as class vars. 

That means type checkers can clearly determine whether a field is an instance attribute or a class attribute. 


However, as my *not knowing* has shown, using explicit `ClassVar` annotations can help readers understand whether a field is an instance or class attribute without having to click through the entire class hierarchy. 

Reading through the original issue (https://github.com/astral-sh/ruff/issues/12877), I get the impression that the focus is mainly the second -- improve clarity for readers. In fact, the `ClassVar` attribute technically is never necessary. Attributes without an annotation always are class variables and using `ClassVar` is mainly to improve readability. That's why I think that making this exception for inherited attributes goes against the rule's intent.

@adigitoleo what's your take on this as the person who originally requested the addition of the rule?

---

_Comment by @InSyncWithFoo on 2025-02-24 13:46_

> Attributes without an annotation always are class variables and using `ClassVar` is mainly to improve readability.

Small correction: Attributes defined <em>with default values</em> create class variables, regardless of `@dataclass` and annotations.

```pycon
>>> @dataclass
... class C:
...     a: int = 42
...
>>> C.a
42
>>> inspect.signature(C)
<Signature (a: int = 42) -> None>
```

---

_Comment by @adigitoleo on 2025-02-24 14:00_

That's essentially correct. The rule was added in response to the issues I raised around the arguably surprising behaviour of un-annotated variables in dataclass definitions. There is a detailed pitch in the linked issue, and some discussion in the forum (link in the OP of that issue).

Although it is valid syntax, I found it to be a stumbling block, especially when trying to offer a dataclass as public API for (novice) users to subclass. Using the explicit annotation for class variables makes the behaviour obvious.

Put another way: `ClassVar` being optional implicitly makes type annotations for instance variables mandatory. There is no way to avoid using at least _some_ annotations with dataclass variables. It is therefore my opinion (shared, I hope, by those who merged the new rule) that the least surprising dataclass definition, for readers (& maintainers) will necessarily use annotations for all variables.

Note that there are also other rules in the RUF category that trigger on valid, but "distasteful" syntax.

---

_Comment by @adigitoleo on 2025-02-24 14:32_

Oh, I see now that the point being discussed here is maybe more specifically this one:

> `ClassVar` annotations are inheritable in the type system, so an inherited one should be deemed equally explicit

I'm still leaning towards ruff taking the conservative stance that inherited `ClassVar` are not "equally explicit", because they do not explicitly appear in the subclass definition. However, I am sympathetic to the concern that this will add more verbosity in some cases ("duplication" of the `ClassVar` annotation on the subclass). My feeling is that the benefit of encouraging `ClassVar` annotations regardless of inheritance outweighs this concern. When using lots of inherited dataclass class variables in a codebase, I would prefer to switch off RUF045 in the linter configuration.

---

_Comment by @kkom on 2025-03-08 16:57_

@adigitoleo – I see! If it is indeed true there is no functional difference between an "inherited" `ClassVar` and an "implied" one, I'm happy with biasing toward explicitness.

---

_Label `bug` removed by @MichaReiser on 2025-03-12 08:07_

---

_Label `rule` added by @MichaReiser on 2025-03-12 08:07_

---
