```yaml
number: 12877
title: Warn about un-annotated attributes in dataclass definition
type: issue
state: closed
author: adigitoleo
labels:
  - rule
  - great writeup
assignees: []
created_at: 2024-08-14T05:39:03Z
updated_at: 2025-02-15T15:08:14Z
url: https://github.com/astral-sh/ruff/issues/12877
synced_at: 2026-01-10T11:09:54Z
```

# Warn about un-annotated attributes in dataclass definition

---

_Issue opened by @adigitoleo on 2024-08-14 05:39_

This is a feature request. The proposal is for a warning which would be triggered if a dataclass definition contains un-annotated attribute declarations. Attributes `d` and `e` in the following example would trigger the warning:

```python
from dataclasses import dataclass

@dataclass
class Foo:  # Public API of a package.
    a: float = 4.2
    b: int = 42
    c: tuple = (4.2, 42)

@dataclass
class Bar(Foo):  # User-defined subclass.
    d = 999
    e = "foo"
```

**Pitch**

By default, all attributes defined in a dataclass definition are class attrtibutes. Those annotated with a type also become instance attributes, unless their annotation is `typing.ClassVar`. This leads to the potentially surprising behaviour that un-annotated attributes are automatically _only class attributes_. Doing `Bar(d=1)` or `Bar(e="bar")` using the above definition leads to an "unexpected keyword argument" error, which itself does not immediately indicate (to novice Python programmers) that `d` and `e` are class attributes. This is especially dangerous when considering to offer a dataclass as public API for users to subclass (comments in the example).

I propose that ruff could at least prompt developers to clarify that they intend for an attribute to be a class attribute by using `typing.ClassVar`. Type annotations are frequently described as "optional", however in this case, by leaving them out, the semantics of the attribute declaration changes. The decision on whether to prompt the addition of "optional" type annotations is also being discussed in https://github.com/astral-sh/ruff/issues/5243, which is about the related [RUF012](https://docs.astral.sh/ruff/rules/mutable-class-default/) rule. However, that rule only covers the case of mutable class attributes (if `e` were a list, in the above example), and its scope extends beyond dataclasses. Because the use of dataclasses arguably implies the use of annotations, I think it is reasonable to warn when they are missing.

I'm not sure if this would be best proposed as a Pylint warning, or as a Ruff-only rule, but considering the classification of RUF012 I chose to propose it here.

I also opened a possibly premature issue [over at mypy](https://github.com/python/mypy/issues/17666) and started a discussion on [the forum](https://discuss.python.org/t/optionally-disallow-un-annotated-class-attributes-for-dataclass/60708/11).

---

_Label `rule` added by @MichaReiser on 2024-08-21 06:45_

---

_Label `needs-triage` added by @MichaReiser on 2024-08-21 06:45_

---

_Comment by @dylwil3 on 2024-11-14 06:08_

I'm in favor of adding this rule - It seems like a clear "gotcha" to me.

It should not be difficult to implement (there is already an `is_dataclass` helper method, and the logic for recommending a `ClassVar` annotation exists in RUF012). The documentation should include a reference to this part of the Python docs: https://docs.python.org/3/library/dataclasses.html#class-variables

The hardest part will be figuring out which number RUF rule it will be since there are a lot of open PRs out right now for those... 😅 

---

_Label `needs-triage` removed by @dhruvmanila on 2024-11-18 06:26_

---

_Label `great writeup` added by @MichaReiser on 2024-11-18 08:04_

---

_Closed by @dylwil3 on 2025-02-15 15:08_

---

_Closed by @dylwil3 on 2025-02-15 15:08_

---
