```yaml
number: 19403
title: "PYI016 fix introduces a `TypeError` for `Union[None, None]`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-07-17T15:51:00Z
updated_at: 2025-08-14T12:06:22Z
url: https://github.com/astral-sh/ruff/issues/19403
synced_at: 2026-01-12T15:54:56Z
```

# PYI016 fix introduces a `TypeError` for `Union[None, None]`

---

_@dscorbett_

### Summary

When all the elements of a `Union` are `None`, the fix for [`duplicate-union-member` (PYI016)](https://docs.astral.sh/ruff/rules/duplicate-union-member/) introduces a `TypeError`. [Example](https://play.ruff.rs/27f2c40f-2ac9-4949-a25b-0de875366a5a):

```console
$ cat >pyi016.py <<'# EOF'
from typing import Union
isinstance(None, Union[None, None])
# EOF

$ ruff --isolated check pyi016.py --select PYI016 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat pyi016.py
from typing import Union
isinstance(None, None)

$ python pyi016.py 2>&1 | tail -n 1
TypeError: isinstance() arg 2 must be a type, a tuple of types, or a union
```

### Version

ruff 0.12.3 (5bc81f26c 2025-07-11)

---

_Label `bug` added by @ntBre on 2025-07-17 16:04_

---

_Label `fixes` added by @ntBre on 2025-07-17 16:04_

---

_Label `help wanted` added by @ntBre on 2025-07-17 16:04_

---

_Comment by @soundsonacid on 2025-07-17 16:29_

will take this one

---

_Assigned to @soundsonacid by @ntBre on 2025-07-17 16:33_

---

_Comment by @soundsonacid on 2025-07-17 23:22_

semantically, this issue seems to bring up a problem that is pretty tricky to solve.

there’s some situations in which reducing from `Union[None, None]` to `None` will introduce a `TypeError`.

for example:

this program runs:
```python
from typing import Union
print(isinstance(None, Union[None, None]))
```

but this program:
```python
print(isinstance(None, None))
```

throws
```
  Traceback (most recent call last):
  File "/Users/frank/throws_error.py", line 1, in <module>
    print(isinstance(None, None))
TypeError: isinstance() arg 2 must be a type, a tuple of types, or a union
```

the typical fix for PYI108 would be to reduce the `Union[None, None]` to `None` by virtue of the duplicate union member, but in this case that obviously doesn't work correctly.

[this pr](https://github.com/astral-sh/ruff/pull/19416), as discussed with @ntBre, addresses the problem with a kind of duct-tapey solution, which is to `defuse` all `DiagnosticGuard`s created within `duplicate_union_member` if the only union members are `None`.

this is obviously not ideal as `Union[None, None] ` _can_ be reduced in any expr, and in some cases _should_ be reduced to simply `None`, such as in a type hint:

```python
x: Union[None, None] = None
```

as i see it, there's two primary options for a more adventurous solution here:

1. do not reduce `Union[T, T]` when it is being passed as a function argument if the underlying `T` is a non-type or a non-runtime-checkable special form.

2. reduce `Union[T, T]` to `type(T)` when it is being passed as a function argument if the underlying `T` is a non-type or a non-runtime-checkable special form.

the main issue with both of these two options is that we would have to be able to statically determine whether or not a given function requires a type, a tuple of types, or a union (such as `isinstance`).

figuring this out statically seems pretty tough without hardcoding a bunch of function names, and even then i can still do stuff like this:

```
~
➜ cat override.py
def isinstance(foo, bar):
    print("oops!")

isinstance(None, None)

~
➜ python override.py
oops!
```

which might cause some user-defined functions, in corner cases, to break if `Union[T, T]` is reduced to `type(T)`.  we may not be concerned about something that narrow though.

any input on what direction to go here for a more concrete fix would be super appreciated!  tagging @AlexWaygood for visibility, was told that he is the resident expert on `typing` and code owner of PYI rules

---

_Comment by @MichaReiser on 2025-07-18 08:59_

Thanks for diving this deep into this. Should we just bail out and not provide a fix in those cases. It seems rare enough and something a user can judge better.

---

_Comment by @soundsonacid on 2025-07-18 13:28_

yeah that is what we were thinking initially, pr #19416 does that but currently has a bug (will resolve today) with handling `Optional[None]` types.

i was hopeful that we could come up with a more comprehensive solution here but it does seem like bailing out & not providing a fix in the most common cases is definitely the easiest way to go about it. we could also potentially leave an `Applicability::DisplayOnly` fix  saying something like "either reduce to `type(T)` or `T` depending on the usage".  even that feels somewhat awkward to me though.

---

_Comment by @dscorbett on 2025-08-14 11:43_

The example in the original comment has been fixed. Is there anything left to do for this issue?

---

_Closed by @AlexWaygood on 2025-08-14 12:06_

---
