---
number: 20094
title: "False positive for F821 in cases of `del` and function scopes"
type: issue
state: closed
author: KennethEnevoldsen
labels: []
assignees: []
created_at: 2025-08-26T08:24:24Z
updated_at: 2025-08-26T12:42:55Z
url: https://github.com/astral-sh/ruff/issues/20094
synced_at: 2026-01-07T13:12:16-06:00
---

# False positive for F821 in cases of `del` and function scopes

---

_Issue opened by @KennethEnevoldsen on 2025-08-26 08:24_

### Summary

I get a F821 when I have a variable called inside a function scope (`test()`), if the scope is followed by `del`.

```py
# tmp.py
for i in range(10):
    a = 2
    print(a) # works fine
    del a

for i in range(10):
    a = 2
    def test():
        print(a) # Undefined name 'a'
    test()
    del a
```

errors are obtained using:
```
ruff check tmp.py
```

### Other

Here is a few more observations that might help locate the bug. Removing the `del` solves the issue
```py
for i in range(10):
    a = 2
    def test():
        print(a) # works fine
    test()
```

The loop is also not required to this to appear:
```py
def test():
    print(a)  # Undefined name 'a'

test()
del a
```
but the function scope is.


### Version

ruff 0.12.9 (ef422460d 2025-08-14)

---

_Referenced in [centre-for-humanities-computing/m-gsm-symbolic#42](../../centre-for-humanities-computing/m-gsm-symbolic/pulls/42.md) on 2025-08-26 08:24_

---

_Comment by @dylwil3 on 2025-08-26 11:21_

Unless I'm misunderstanding this lint is correct. We can't know when and where you will call that function, and whether it's called after you have done `del a`. So, for example:

```pycon
>>> a = 1
>>> def test():
...     print(a)
...
>>> del a
>>> test()
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    test()
    ~~~~^^
  File "<python-input-1>", line 2, in test
    print(a)
          ^
NameError: name 'a' is not defined
```

---

_Comment by @KennethEnevoldsen on 2025-08-26 12:17_

Ahh, right, so it's a case that is unknowable - so the lint is correct - thanks for the clarification :)

---

_Closed by @KennethEnevoldsen on 2025-08-26 12:17_

---

_Comment by @ntBre on 2025-08-26 12:42_

There's also https://github.com/astral-sh/ruff/issues/9858, which sounds closely related.

---
