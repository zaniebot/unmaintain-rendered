```yaml
number: 1595
title: Slice is not subscriptable at runtime
type: issue
state: open
author: joecox
labels:
  - wish
  - runtime semantics
assignees: []
created_at: 2025-11-19T19:54:51Z
updated_at: 2025-11-19T21:02:33Z
url: https://github.com/astral-sh/ty/issues/1595
synced_at: 2026-01-12T15:54:25Z
```

# Slice is not subscriptable at runtime

---

_@joecox_

### Summary

ty should report [`not-subscriptable`](https://docs.astral.sh/ty/reference/rules/#non-subscriptable) when annotating slice types ~below 3.14:~

```sh
$ cat >slice.py <<'# EOF'
s: slice[int]
# EOF

$ uv run --python 3.13 slice.py
Using CPython 3.13.7 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Traceback (most recent call last):
  File "/Users/joe/slice.py", line 1, in <module>
    s: slice[int]
       ~~~~~^^^^^
TypeError: type 'slice' is not subscriptable

$ uv run --python 3.14 slice.py
Using CPython 3.14.0
Removed virtual environment at: .venv
Creating virtual environment at: .venv

$ uvx --python 3.13 --with "ty==0.0.1-alpha.27" ty check slice.py
All checks passed!
```

I assume ty doesn't complain because typeshed has slice as generic? ~I don't even know if it's documented anywhere that subscripting slice began working on 3.14.~

### Version

ty 0.0.1-alpha.27 (26d7b6864 2025-11-18)

---

_Comment by @carljm on 2025-11-19 20:35_

Thanks for the report!

In fact, `slice` didn't become subscriptable at runtime on Python 3.14 either. What happened was that type annotations became deferred by default (PEP 649/749), so `s: slice[int]` doesn't execute the code `slice[int]` at runtime.

```
Python 3.14.0rc2 (main, Aug 28 2025, 17:02:21) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> slice[int]
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    slice[int]
    ~~~~~^^^^^
TypeError: type 'slice' is not subscriptable
```

Ideally, type checkers should not assume that just because `slice` is generic in typeshed, that also means it is subscriptable at runtime. Runtime subscriptability should be indicated separately by defining `__class_getitem__`. But in typeshed, `Generic` itself defines `__class_getitem__`, so it may be difficult for type checkers to make this distinction.

None of the other type checkers (mypy, pyright, pyrefly) report this either.

So I think it will not be a priority for us in the near term, but I'll keep the issue open as something we may want to look at in future.

---

_Label `runtime semantics` added by @carljm on 2025-11-19 20:35_

---

_Label `wish` added by @carljm on 2025-11-19 20:35_

---

_Renamed from "Slice is not subscriptable < py3.14" to "Slice is not subscriptable at runtime" by @carljm on 2025-11-19 20:35_

---

_Comment by @joecox on 2025-11-19 20:39_

That makes sense, thanks for the info!

---

_Comment by @AlexWaygood on 2025-11-19 20:41_

> Ideally, type checkers should not assume that just because `slice` is generic in typeshed, that also means it is subscriptable at runtime. Runtime subscriptability should be indicated separately by defining `__class_getitem__`.

Yes, the reason why we generally include `__class_getitem__` methods in typeshed when they exist at runtime (even for classes that inherit from `Generic` in typeshed) is so that type checkers can have the option of distinguishing between classes that are generic at runtime _and_ at type-check time, and those which are only generic at type-check time.

> But in typeshed, `Generic` itself defines `__class_getitem__`, so it may be difficult for type checkers to make this distinction.

What do you mean here? I don't think typeshed says anything about whether `Generic` defines `__class_getitem__`. `Generic` isn't even a class in typeshed.

> None of the other type checkers (mypy, pyright, pyrefly) report this either.
> 
> So I think it will not be a priority for us in the near term, but I'll keep the issue open as something we may want to look at in future.

Yeah, I agree that this shouldn't be a priority

---

_Comment by @carljm on 2025-11-19 20:47_

> What do you mean here? I don't think typeshed says anything about whether `Generic` defines `__class_getitem__`. `Generic` isn't even a class in typeshed.

I was looking at https://github.com/python/typeshed/blob/main/stdlib/typing.pyi#L445-L457. Admittedly I didn't think very hard about why it does that funny dance with `class _Generic` and then `Generic: type[_Generic]` -- but I think that if taken literally (and if a type checker allowed subclassing a `type[...]` type), that would still have the effect of making a type checker think that a class inheriting `Generic` had `__class_getitem__` (at least on Python 3.10+).

That said, I don't think that's actually the issue in ty; we special-case inheritance from Generic anyway. I agree that if this is true:

> we generally include `__class_getitem__` methods in typeshed when they exist at runtime (even for classes that inherit from `Generic` in typeshed)

then in principle type checkers have the information they need in order to handle this correctly (even though apparently they don't!).

The issues in ty here are really two-fold:

1) We don't type check annotations as runtime value expressions, on any Python version, even though ideally we should for Python <3.13, without `from __future__ import annotations`. This might be hard to fix with our current approach to value vs type expressions, but I think we should change our approach there in a way that would make it easier in the future.
2) Even in value expressions, we special-case explicit subscription of generic classes and always allow it, regardless of whether the class has `__class_getitem__` or not. This would be pretty easy to fix, I think.


---

_Comment by @AlexWaygood on 2025-11-19 20:55_

> I was looking at https://github.com/python/typeshed/blob/main/stdlib/typing.pyi#L445-L457. Admittedly I didn't think very hard about why it does that funny dance with `class _Generic` and then `Generic: type[_Generic]` -- but I think that if taken literally (and if a type checker allowed subclassing a `type[...]` type), that would still have the effect of making a type checker think that a class inheriting `Generic` had `__class_getitem__` (at least on Python 3.10+).

Oh! I'm sorry, I forgot the definition changed in https://github.com/python/typeshed/pull/14583; I had the previous definition still in my head (`Generic` used to just be an instance of `_SpecialForm` in typeshed). The funny dance is because pyright broke in the first version of that PR, when we tried to make `Generic` an actual class.



---
