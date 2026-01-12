```yaml
number: 4396
title: "`self` arguments to methods should not have type annotations"
type: issue
state: closed
author: rra
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-05-12T18:41:42Z
updated_at: 2024-11-12T10:47:06Z
url: https://github.com/astral-sh/ruff/issues/4396
synced_at: 2026-01-12T15:54:44Z
```

# `self` arguments to methods should not have type annotations

---

_@rra_

Currently, Ruff does not appear to exclude the `self` argument to class methods from its check for type annotations. For example, this Python file:

```python
class Foo:
    def bar(self, something: str) -> str:
        return something + "else"
```

reports:

```
% ruff check --select 'ANN' foo.py 
foo.py:2:13: ANN101 Missing type annotation for `self` in method
Found 1 error.
```

However, `self` should not have a type annotation; it's unnecessary (it adds no useful type information; the type of self is already known by the type checker because the method is defined on a class) and explicitly recommended against in the mypy documentation. See, for example, https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html#classes (note the comment on `deposit`).

---

_Comment by @charliermarsh on 2023-05-12 18:54_

I believe `ANN101` and `ANN102` are their own rules for this reason -- so that you can turn them off. Whether they should exist _at all_ is probably a different question, but the encoding was inherited from the originating plugin.

---

_Label `question` added by @charliermarsh on 2023-05-12 18:59_

---

_Comment by @rra on 2023-05-12 19:10_

Ah, I see this is controversial upstream as well (https://github.com/sco1/flake8-annotations/issues/80).

I think `ANN101` is always incorrect and indeed would like the linter to check the inverse: complain if `self` has a type annotation, since the type annotation for `self` makes the code more fragile under refactoring and should be omitted. But I totally understand if you don't want to diverge from the original upstream plugin, and it doesn't look like the plugin author is interested in changing this.

---

_Comment by @zanieb on 2023-05-12 19:13_

Now that there's a [`Self` type](https://peps.python.org/pep-0673/) it's a bit more reasonable to expect a type annotation here for consistency with other function parameters.

---

_Comment by @rra on 2023-05-12 19:15_

Incidentally, flake8-annotations links to https://peps.python.org/pep-0484/#annotating-instance-and-class-methods as an example of when this typing might be necessary, but I believe this example is now obsolete now that `Self` has been added. The example in PEP 484:
```python
T = TypeVar('T', bound='Copyable')
class Copyable:
    def copy(self: T) -> T:
        # return a copy of self
```
can now be written much simpler as:
```python
class Copyable:
    def copy(self) -> Self:
        # Return a copy of self
```

---

_Comment by @rra on 2023-05-12 19:17_

> Now that there's a [`Self` type](https://peps.python.org/pep-0673/) it's a bit more reasonable to expect a type annotation here for consistency with other function parameters.

Agreed that at least makes it less fragile and turns it into a question of style about which people will have different opinions. I modify my desire for a linter for `self` typing to one that complains about any type other than `Self`. :)

---

_Comment by @charliermarsh on 2023-05-12 19:18_

Ah interesting. Yeah I've definitely done this before for generics:

```py
T = TypeVar('T', bound='Copyable')
class Copyable:
    def copy(self: T) -> T:
        # return a copy of self
```

It's neat that it's no longer required.



---

_Comment by @charliermarsh on 2023-05-12 19:19_

I think we need to do some grepping to see if any projects are actually relying on this rule, or whether we can just remove it.

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:13_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:13_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:13_

---

_Comment by @bersbersbers on 2024-02-01 22:32_

Duplicate of #7709, I guess

---

_Comment by @charliermarsh on 2024-02-01 22:38_

Ah that's right! This rule is removed in v0.2.0.

---

_Closed by @charliermarsh on 2024-02-01 22:38_

---

_Added to milestone `v0.2.0` by @charliermarsh on 2024-02-01 22:38_

---

_Comment by @ssbarnea on 2024-10-15 13:53_

Does anyone know an reverse-ANN001 that can be used to remove mistakenly added type to self?

I am wondering why on a specific project where I use ruff 0.7.3, I still see:
```
ANN001 Missing type annotation for function argument `self`
```

Created new bug for that https://github.com/astral-sh/ruff/issues/14295


---
