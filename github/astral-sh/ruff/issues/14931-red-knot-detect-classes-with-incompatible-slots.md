---
number: 14931
title: "[red-knot] Detect classes with incompatible `__slots__` being used in multiple inheritance"
type: issue
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
created_at: 2024-12-12T11:02:16Z
updated_at: 2024-12-27T11:43:50Z
url: https://github.com/astral-sh/ruff/issues/14931
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Detect classes with incompatible `__slots__` being used in multiple inheritance

---

_Issue opened by @AlexWaygood on 2024-12-12 11:02_

This isn't necessarily a priority for red-knot, but it's something that we'll want to do at some point and it could be easily done now.

Two classes that both have nonempty `__slots__` cannot be used in multiple inheritance. We should detect this and emit a diagnostic for it.

```pycon
>>> class Foo:
...     __slots__ = ("a",)
...     
>>> class Bar:
...     __slots__ = ("b",)
...     
>>> class Baz(Foo, Bar): ...
... 
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    class Baz(Foo, Bar): ...
TypeError: multiple bases have instance lay-out conflict
```

We'll probably want to check for this as part of this method here: https://github.com/astral-sh/ruff/blob/82faa9bb62e66a562f8a7ad81a645162ca558a08/crates/red_knot_python_semantic/src/types/infer.rs#L510-L520

Notes on the semantics:

### (1) A class with no `__slots__` can be used in multiple inheritance with a class that has `__slots__`

```pycon
>>> class A: ...
... 
>>> class B:
...     __slots__ = ("a",)
...     
>>> class C(A, B): ...
... 
>>> 
```

### (2) Empty `__slots__` are fine in combination with multiple inheritance:

```pycon
>>> class A:
...     __slots__ = ()
...     
>>> class B:
...     __slots__ = ("a",)
...     
>>> class C(A, B): ...
... 
>>> 
```

### (3) But even two classes with equal `__slots__` will fail in multiple inheritance if they are both non-empty:

```pycon
>>> class A:
...     __slots__ = ("a",)
...     
>>> class B:
...     __slots__ = ("a",)
...     
>>> class C(A, B): ...
... 
Traceback (most recent call last):
  File "<python-input-11>", line 1, in <module>
    class C(A, B): ...
TypeError: multiple bases have instance lay-out conflict
```

### (5) And `__slots__` are inherited by subclasses:

```pycon
>>> class A:
...     __slots__ = ("a",)
...     
>>> class B(A): ...
... 
>>> class C:
...     __slots__ = ("c",)
...     
>>> class D(C): ...
... 
>>> class E(B, D): ...
... 
Traceback (most recent call last):
  File "<python-input-16>", line 1, in <module>
    class E(B, D): ...
TypeError: multiple bases have instance lay-out conflict
```

---

_Label `red-knot` added by @AlexWaygood on 2024-12-12 11:02_

---

_Comment by @carljm on 2024-12-14 00:59_

Perhaps the case where this comes up most is with builtin/extension types, that aren't implemented in Python and thus don't use `__slots__` Python API, but do still have a bespoke instance memory layout (rather than just a dictionary of attributes), like a Python class that uses `__slots__`. E.g. you can't multiple-inherit `int` and `str` for this reason.

But typeshed does not explicitly apply `__slots__` to types like `int` or `str` that effectively do have slots. We could potentially advocate to change this. But in the meantime, fully implementing this feature would also require adding a special case to always consider some known typeshed classes as having slots.

---

_Comment by @AlexWaygood on 2024-12-14 14:22_

Yeah, I think there's three distinct issues here:
1. If two classes both have non-empty `__slots__` definitions that are "visible" (i.e., we can see the `__slots__` definitions in the source-code files we treat as the canonical source of information for those classes) to red-knot, and they're used in multiple inheritance, should we emit a diagnostic? I think we clearly should, because we have all the information we require to detect that an exception will definitely be raised at runtime. Lots of libraries and applications use classes with `__slots__`, so I think we'll catch a lot of possible bugs just by implementing this.
2. Typeshed generally doesn't include `__slots__` for any classes (even pure-Python) in its stubs. It probably should, as it would give type checkers a lot more information to work with in this domain, allowing them to catch a lot more bugs. This has been discussed previously in https://github.com/python/typeshed/issues/8832.
3. As you say, you get an identical error message at runtime to the `__slots__` error message if you try to multiple-inherit from various builtin classes such as `str` or `int` that CPython considers to be "solid bases", despite the fact that these classes do not have `__slots__` definitions (at least, not `__slots__` definitions that are visible from the Python level):
   ```pycon
   >>> class Foo(int, str): ...
   ... 
   Traceback (most recent call last):
     File "<python-input-0>", line 1, in <module>
       class Foo(int, str): ...
   TypeError: multiple bases have instance lay-out conflict
   >>> hasattr(int, "__slots__")
   False
   >>> hasattr(str, "__slots__")
   False
   ```

   There are various possible ways we could detect this. Typeshed could pretend that they have `__slots__` definitions even though they don't (at least, not in the same way as pure-Python classes that define `__slots__`); we could hardcode a list of builtin classes in red-knot that you can't combine in multiple inheritance; or we could work on a new standardised type-system feature that allows us to express when a class is a "solid base" that you can't use in multiple inheritance.

I think these are all quite separate questions; the only one I was attempting to tackle in this issue was question (1).

---

_Comment by @InSyncWithFoo on 2024-12-23 20:27_

Some other problems to consider:

1. Invalid

	```python
	class A:
		__slots__ = 1    # Not iterable

	class B:
		__slots__ = '1'  # Not identifier
	```

2. String literal

	```python
	class A:
		__slots__ = 'abc'

	A().abc = 1  # This is fine
	A().a = 1    # This is not
	```

3. Possibly unbound

	```python
	class A:
		if ...:
			__slots__ = ('a', 'b')
	
	class B:
		__slots__ = ('c', 'd')
	
	# Might or might not be valid at runtime
	class C(A, B): ...
	```

4. Dynamic

	```python
	t = ('a', 'b') if ... else ()
	
	class A:
		__slots__ = t  # or `tuple(e for e in t)`
	
	class B:
		__slots__ = ('c', 'd')
	
	# Might or might not be valid at runtime
	class C(A, B): ...
	```

5. Post-hoc modifications

	```python
	class A:
		__slots__ = ['a', 'b']
		__slots__.clear()
	
	class B:
		__slots__ = ('c', 'd')
	
	# Valid at runtime
	class C(A, B): ...
	```

6. Some or all of the above mixed together
7. Some or all of the above mixed together, in an unresolvable MRO


---

_Comment by @AlexWaygood on 2024-12-23 20:38_

Those are great edge cases @InSyncWithFoo! I think for an initial implementation of this check we could easily defer them all however, and only emit the incompatible-slots error for two classes in a bases list if both classes have non-dynamic slots definitions that are definitely bound. We could then open follow-up issues to expand the check and detect issues in more cases. This would make the initial PR easy for us to review and would be in keeping with the principle that we should avoid aim to avoid false positives as much as possible (even if it causes false negatives in some situations) in our initial implementation of red-knot.

---

_Referenced in [astral-sh/ruff#15129](../../astral-sh/ruff/pulls/15129.md) on 2024-12-24 00:28_

---

_Closed by @AlexWaygood on 2024-12-27 11:43_

---
