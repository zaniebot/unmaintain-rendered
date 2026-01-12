```yaml
number: 17989
title: "[ty] Do not carry the generic context of `Protocol` or `Generic` in the `ClassBase` enum"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/generic-mro
created_at: 2025-05-09T17:04:29Z
updated_at: 2025-05-23T01:37:05Z
url: https://github.com/astral-sh/ruff/pull/17989
synced_at: 2026-01-12T15:56:09Z
```

# [ty] Do not carry the generic context of `Protocol` or `Generic` in the `ClassBase` enum

---

_@AlexWaygood_

## Summary

It doesn't seem to be necessary for our generics implementation to carry the `GenericContext` in the `ClassBase` variants. Removing it simplifies the code, fixes many TODOs about `Generic` or `Protocol` appearing multiple times in MROs when each should only appear at most once, and allows us to more accurately detect runtime errors that occur due to `Generic` or `Protocol` appearing multiple times in a class's bases.

In order to remove the `GenericContext` from the `ClassBase` variant, it turns out to be necessary to emulate `typing._GenericAlias.__mro_entries__`, or we end up with a large number of false-positive `inconsistent-mro` errors. This PR therefore also does that.

Lastly, this PR fixes the inferred MROs of PEP-695 generic classes, which implicitly inherit from `Generic` even if they have no explicit bases.

## Test Plan

mdtests


---

_Label `ty` added by @AlexWaygood on 2025-05-09 17:05_

---

_Marked ready for review by @AlexWaygood on 2025-05-09 17:06_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-09 17:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-09 17:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-09 17:06_

---

_Comment by @github-actions[bot] on 2025-05-09 17:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
+ error[inconsistent-mro] more_itertools/more.pyi:166:1: Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Iterable[_T_co]'>]`
+ error[inconsistent-mro] more_itertools/more.pyi:169:1: Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Reversible[_T_co]'>]`
- Found 50 diagnostics
+ Found 52 diagnostics

anyio (https://github.com/agronholm/anyio)
+ error[inconsistent-mro] src/anyio/from_thread.py:84:1: Cannot create a consistent method resolution order (MRO) for class `_BlockingAsyncContextManager` with bases list `[typing.Generic[T_co], <class 'AbstractContextManager'>]`
- Found 118 diagnostics
+ Found 119 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- error[inconsistent-mro] mkdocs/config/config_options.py:323:1: Cannot create a consistent method resolution order (MRO) for class `Type` with bases list `[typing.Generic[T], <class 'OptionallyRequired[T]'>]`
- warning[unused-ignore-comment] mkdocs/config/defaults.py:41:42: Unused blanket `type: ignore` directive
- Found 210 diagnostics
+ Found 208 diagnostics

mypy (https://github.com/python/mypy)
+ error[invalid-argument-type] mypy/plugins/proper_plugin.py:172:21: Argument to bound method `__init__` is incorrect: Expected `TypeInfo`, found `Unknown | SymbolNode | None`
+ error[call-non-callable] mypy/report.py:515:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]])` is not callable on object of type `Unknown | list[Unknown]`
+ error[call-non-callable] mypy/report.py:659:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]])` is not callable on object of type `Unknown | list[Unknown]`
+ error[call-non-callable] mypy/report.py:890:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]])` is not callable on object of type `Unknown | list[Unknown]`
- error[inconsistent-mro] mypy/visitor.py:353:1: Cannot create a consistent method resolution order (MRO) for class `NodeVisitor` with bases list `[typing.Generic[T], <class 'ExpressionVisitor[T]'>, <class 'StatementVisitor[T]'>, <class 'PatternVisitor[T]'>]`
- Found 3316 diagnostics
+ Found 3319 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- error[inconsistent-mro] django-stubs/contrib/auth/forms.pyi:115:1: Cannot create a consistent method resolution order (MRO) for class `SetPasswordForm` with bases list `[typing.Generic[_UserType: AbstractBaseUser], <class 'SetPasswordMixin[Unknown]'>, <class 'Form'>]`
+ error[inconsistent-mro] django-stubs/contrib/auth/forms.pyi:115:1: Cannot create a consistent method resolution order (MRO) for class `SetPasswordForm` with bases list `[typing.Generic[_UserType: AbstractBaseUser], <class 'SetPasswordMixin'>, <class 'Form'>]`
+ error[inconsistent-mro] django-stubs/core/paginator.pyi:14:1: Cannot create a consistent method resolution order (MRO) for class `_SupportsPagination` with bases list `[typing.Protocol[_T], <class 'Sized'>, <class 'Iterable'>]`
+ error[inconsistent-mro] django-stubs/forms/formsets.pyi:28:1: Cannot create a consistent method resolution order (MRO) for class `BaseFormSet` with bases list `[typing.Generic[_F: BaseForm], <class 'Sized'>, <class 'RenderableFormMixin'>]`
+ error[inconsistent-mro] django-stubs/utils/datastructures.pyi:39:1: Cannot create a consistent method resolution order (MRO) for class `_IndexableCollection` with bases list `[typing.Protocol[_I], <class 'Collection[_I]'>]`
- error[inconsistent-mro] django-stubs/views/generic/edit.pyi:71:1: Cannot create a consistent method resolution order (MRO) for class `DeleteView` with bases list `[typing.Generic[_M: Model, _ModelFormT: BaseModelForm[Unknown]], <class 'SingleObjectTemplateResponseMixin'>, <class 'BaseDeleteView[_M, _ModelFormT]'>]`
- Found 471 diagnostics
+ Found 473 diagnostics

```
</details>


---

_Converted to draft by @AlexWaygood on 2025-05-09 17:14_

---

_Comment by @dcreager on 2025-05-09 18:41_

I haven't reviewed the code yet, but I like the idea!  The reason I had put it there to begin with was because I thought I'd need it when determining that a class is generic via the legacy syntax.  But we do that by looking at the _explicit bases_, not the MRO, which means that we're looking at a `Type`, not a `ClassBase`:

https://github.com/astral-sh/ruff/blob/7a48477c6797ae817c33994e5cddb7f8375118bb/crates/ty_python_semantic/src/types/class.rs#L539-L542

So I don't foresee any problems with this vis-à-vis the generics machinery.

---

_Comment by @AlexWaygood on 2025-05-09 20:25_

I think I might need to emulate the logic that lives at https://github.com/python/cpython/blob/98e2c3af4794d6c6ebe47b20badbd31c542d944e/Lib/typing.py#L1214-L1244 at runtime to fix those `inconsistent-mro` errors in the primer diff :-(

Inheriting from `Generic[T]` is heavily special-cased at runtime...

---

_Comment by @AlexWaygood on 2025-05-22 22:47_

that primer diff is _starting_ to look a little better... still looks like there might be _some_ bugs here...

---

_Comment by @AlexWaygood on 2025-05-22 23:21_

## Partial mypy_primer analysis

> ```diff
> more-itertools (https://github.com/more-itertools/more-itertools)
> + error[inconsistent-mro] more_itertools/more.pyi:166:1: Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Iterable[_T_co]'>]`
> + error[inconsistent-mro] more_itertools/more.pyi:169:1: Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Reversible[_T_co]'>]`
> ```

These classes do succeed at runtime:

```pycon
>>> from typing import Protocol, TypeVar, Sized, Iterable
>>> T = TypeVar("T")
>>> class _SizedIterable(Protocol[T], Sized, Iterable[T]): ...
... 
>>>
```

but they _wouldn't_ succeed if these bases actually had the MROs that typeshed says they have!

```pycon
>>> from typing import Protocol, TypeVar
>>> T = TypeVar("T")
>>> class Sized(Protocol): ...
... 
>>> class Iterable(Protocol[T]): ...
... 
>>> class _SizedIterable(Protocol[T], Sized, Iterable[T]): ...
... 
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    class _SizedIterable(Protocol[T], Sized, Iterable[T]): ...
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 2067, in __new__
    return super().__new__(mcls, name, bases, namespace, **kwargs)
           ~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen abc>", line 106, in __new__
TypeError: Cannot create a consistent method resolution order (MRO) for bases Protocol, Sized, Iterable
```

The two new hits in `django-stubs` look similar, as does the new one in `anyio`.

There's an increase in the total number of diagnostics on mypy, but this looks like it's mostly because a bogus `inconsistent-mro` error is going away, which means we no longer resolve that class's MRO as containing `Unknown` (which led us to treat it as a very dynamic type).

dd-trace-py: this one is definitely a bug in the PR as it currently stands. Minimal repro:

```py
from typing import Generic, TypeVar

T = TypeVar("T")

class Base: ...
class Intermediate(Base, Generic[T]): ...
class Sub(Intermediate[T], Base): ...  # false-positive `[inconsistent-mro]` here
```

---

_Comment by @AlexWaygood on 2025-05-22 23:58_

## mypy_primer analysis

> ```diff
> more-itertools (https://github.com/more-itertools/more-itertools)
> + error[inconsistent-mro] more_itertools/more.pyi:166:1: Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Iterable[_T_co]'>]`
> + error[inconsistent-mro] more_itertools/more.pyi:169:1: Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Reversible[_T_co]'>]`
> ```

These classes do succeed at runtime:

```pycon
>>> from typing import Protocol, TypeVar, Sized, Iterable
>>> T = TypeVar("T")
>>> class _SizedIterable(Protocol[T], Sized, Iterable[T]): ...
... 
>>>
```

but they _wouldn't_ succeed if these bases actually had the MROs that typeshed says they have!

```pycon
>>> from typing import Protocol, TypeVar
>>> T = TypeVar("T")
>>> class Sized(Protocol): ...
... 
>>> class Iterable(Protocol[T]): ...
... 
>>> class _SizedIterable(Protocol[T], Sized, Iterable[T]): ...
... 
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    class _SizedIterable(Protocol[T], Sized, Iterable[T]): ...
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 2067, in __new__
    return super().__new__(mcls, name, bases, namespace, **kwargs)
           ~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen abc>", line 106, in __new__
TypeError: Cannot create a consistent method resolution order (MRO) for bases Protocol, Sized, Iterable
```

The two new hits in django-stubs look similar, as does the new one in anyio.

There's an increase in the total number of diagnostics on mypy, but this looks like it's mostly because a bogus `inconsistent-mro` error is going away, which means we no longer resolve that class's MRO as containing `Unknown` (which led us to treat it as a very dynamic type). We can also see that there's a bogus `inconsistent-mro` error that goes away from mkdocs, which is great.

---

_Marked ready for review by @AlexWaygood on 2025-05-23 00:14_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-23 00:14_

---

_Comment by @AlexWaygood on 2025-05-23 00:15_

Okay, this is finally ready for review!

---

_Comment by @carljm on 2025-05-23 01:19_

The more-itertools issue also shows up in [pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAK4MYAxmADYA0UAKogKYBqAhiDQMpIBeDAJjQCSMBiBYAjCgwBQtKAF46jViAAUAIlrqa5AG5skLFDHm0QAVwYBKaaQosAzg6gB9Lrz7DREqaqJgScgoAbVoAXU4efiERMUkGULCrAC4oADoM6WkgA). Not in [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=541a59339d2cedb51f89e17179ad58c3), but I think if it's a) correct, and b) pyright emits it too, that's good enough to go with it for now.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/mro.md`:539 on 2025-05-23 01:24_

It's not clear to me why we see `Iterator[T]` and `Iterable[T]` in the MRO here, rather than `Iterator[Unknown]` and `Iterable[Unknown]`. I thought we propagated specialization up through the MRO? Do we fail to propagate the multiple layers of specialization ("the typevar used in Iterator in typeshed" -> "our T typevar here" -> Unknown)?

(This might be a @dcreager question, and doesn't need to block this PR.)

---

_@carljm approved on 2025-05-23 01:32_

Very nice!

---

_@AlexWaygood reviewed on 2025-05-23 01:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/mro.md`:539 on 2025-05-23 01:36_

Yeah, I agree that this still isn't quite right. The issue predates this PR, though — you can see it in lots of other MROs in our tests — so I'm going to punt on fixing it for now.

---

_Merged by @AlexWaygood on 2025-05-23 01:37_

---

_Closed by @AlexWaygood on 2025-05-23 01:37_

---

_Branch deleted on 2025-05-23 01:37_

---
