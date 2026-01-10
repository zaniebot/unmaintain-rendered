```yaml
number: 2153
title: "Type checking: intersection of `dataclasses.field.{default, default_factory}` values is not detected properly"
type: issue
state: closed
author: Thibaulltt
labels:
  - narrowing
assignees: []
created_at: 2025-12-22T10:27:40Z
updated_at: 2025-12-22T19:18:53Z
url: https://github.com/astral-sh/ty/issues/2153
synced_at: 2026-01-10T01:56:41Z
```

# Type checking: intersection of `dataclasses.field.{default, default_factory}` values is not detected properly

---

_Issue opened by @Thibaulltt on 2025-12-22 10:27_

### Summary

Here is a small example file:

```python
from dataclasses import dataclass, field, fields, MISSING


@dataclass
class Foo:
	name: str = field(default='foo')
	value: int = field(default=23)


foo = Foo()
for field_ in fields(foo):
	if field_.default is MISSING and field_.default_factory is MISSING:
		raise ValueError()
	_fdef = field_.default_factory() if field_.default is MISSING else field_.default
	print(f'Field <{field_.name}> default is {_fdef!s}')

```

In a `dataclasses.dataclass`, a `field`'s `default` and `default_factory` members can be one of two things: the special type `_MISSING_TYPE` (made available under the `MISSING` attribute of the module) and a defined value of type `Any` or `Callable`.

From your explanation of your [type intersection feature](https://docs.astral.sh/ty/features/type-system/#intersection-types) (very nice feature, BTW! this bug non-withstanding) it seems like I should be able to 'restrict' the type of those attributes by checking their value. Thus, I perform a check on line 12 and raise an error if they are both `MISSING`. Thus, one of them should be `not MISSING`.

But here's the output of `ty check main.py` (no configuration needed):

```
error[call-non-callable]: Object of type `_MISSING_TYPE` is not callable
  --> main.py:14:10
   |
12 |     if field_.default is MISSING and field_.default_factory is MISSING:
13 |         raise ValueError()
14 |     _fdef = field_.default_factory() if field_.default is MISSING else field_.default
   |             ^^^^^^^^^^^^^^^^^^^^^^^^
15 |     print(f'Field <{field_.name}> default is {_fdef!s}')
   |
info: Union variant `_MISSING_TYPE` is incompatible with this call site
info: Attempted to call union type `_DefaultFactory[Any] | _MISSING_TYPE`
info: rule `call-non-callable` is enabled by default

Found 1 diagnostic
```

Is this a bug, or simply a non-supported use case ? It seems your type intersection feature _should_ work in this case, since `MISSING` is defined as a type*, and I explicitly check it using the `if <var> is <type>` expression.

----

\* It is actually defined as an instance of the `_MISSING_TYPE` class (see below), but changing MISSING for its actual _MISSING_TYPE type does not change the result.

```python
# A sentinel object to detect if a parameter is supplied or not.  Use
# a class to give it a better repr.
class _MISSING_TYPE:
    pass
MISSING = _MISSING_TYPE()
```

### Version

ty 0.0.5

---

_Label `narrowing` added by @AlexWaygood on 2025-12-22 14:46_

---

_Comment by @carljm on 2025-12-22 17:33_

Thanks for the report!

The problem here is that the way you are checking both `field_.default` and `field_.default_factory` would require the type checker to track a relationship between the type of those two attributes. After the first `if` we would have to remember "if `field_.default` is MISSING then `field_.default_factory` cannot be, and vice versa." This kind of implication-tracking between two different types across control flow is something that we don't do (and neither does any other type checker; pyright, mypy, and pyrefly all emit the same error on this code that we do.)

Here's a version of your code that rearranges the control flow slightly so that it works in ty (and other type checkers): https://play.ty.dev/d1cf6785-8a39-481a-8337-f5b4a0da7b5f

---

_Closed by @carljm on 2025-12-22 17:33_

---

_Comment by @Thibaulltt on 2025-12-22 19:18_

Gotcha, thanks for the tip :)

I was unsure of how precisely you kept track of the control flow, but that explains it. Keep up the great work !

---
