```yaml
number: 19144
title: "UP007 and UP045: typeshed considers some functions to be classes"
type: issue
state: open
author: dscorbett
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-04T13:48:24Z
updated_at: 2025-07-07T13:43:36Z
url: https://github.com/astral-sh/ruff/issues/19144
synced_at: 2026-01-10T11:09:59Z
```

# UP007 and UP045: typeshed considers some functions to be classes

---

_Issue opened by @dscorbett on 2025-07-04 13:48_

### Summary

There are more functions which typeshed considers classes for which [`non-pep604-annotation-union` (UP007)](https://docs.astral.sh/ruff/rules/non-pep604-annotation-union/) and [`non-pep604-annotation-optional` (UP045)](https://docs.astral.sh/ruff/rules/non-pep604-annotation-optional/) should be suppressed as in #18682. [Example](https://play.ruff.rs/aabb8c4b-24d8-44fe-bf1f-b8b5823f4c36):
```console
$ cat >up007_up045.py <<'# EOF'
import multiprocessing
import multiprocessing.dummy
import select
import threading
from typing import Optional, Union

print("Optional:")

try: o0: Optional[multiprocessing.JoinableQueue]
except TypeError as e: print(f"multiprocessing.JoinableQueue: {e}")
else: print(f"multiprocessing.JoinableQueue: no error")

try: o1: Optional[multiprocessing.Queue]
except TypeError as e: print(f"multiprocessing.Queue: {e}")
else: print(f"multiprocessing.Queue: no error")

try: o2: Optional[multiprocessing.SimpleQueue]
except TypeError as e: print(f"multiprocessing.SimpleQueue: {e}")
else: print(f"multiprocessing.SimpleQueue: no error")

try: o3: Optional[multiprocessing.dummy.Lock]
except TypeError as e: print(f"multiprocessing.dummy.Lock: {e}")
else: print(f"multiprocessing.dummy.Lock: no error")

try: o4: Optional[multiprocessing.dummy.RLock]
except TypeError as e: print(f"multiprocessing.dummy.RLock: {e}")
else: print(f"multiprocessing.dummy.RLock: no error")

try: o5: Optional[select.poll]
except TypeError as e: print(f"select.poll: {e}")
else: print(f"select.poll: no error")

try: o6: Optional[threading.Lock]
except TypeError as e: print(f"threading.Lock: {e}")
else: print(f"threading.Lock: no error")

try: o7: Optional[threading.RLock]
except TypeError as e: print(f"threading.RLock: {e}")
else: print(f"threading.RLock: no error")

print("\nUnion:")

try: u0: Union[multiprocessing.JoinableQueue, None]
except TypeError as e: print(f"multiprocessing.JoinableQueue: {e}")
else: print(f"multiprocessing.JoinableQueue: no error")

try: u1: Union[multiprocessing.Queue, None]
except TypeError as e: print(f"multiprocessing.Queue: {e}")
else: print(f"multiprocessing.Queue: no error")

try: u2: Union[multiprocessing.SimpleQueue, None]
except TypeError as e: print(f"multiprocessing.SimpleQueue: {e}")
else: print(f"multiprocessing.SimpleQueue: no error")

try: u3: Union[multiprocessing.dummy.Lock, None]
except TypeError as e: print(f"multiprocessing.dummy.Lock: {e}")
else: print(f"multiprocessing.dummy.Lock: no error")

try: u4: Union[multiprocessing.dummy.RLock, None]
except TypeError as e: print(f"multiprocessing.dummy.RLock: {e}")
else: print(f"multiprocessing.dummy.RLock: no error")

try: u5: Union[select.poll, None]
except TypeError as e: print(f"select.poll: {e}")
else: print(f"select.poll: no error")

try: u6: Union[threading.Lock, None]
except TypeError as e: print(f"threading.Lock: {e}")
else: print(f"threading.Lock: no error")

try: u7: Union[threading.RLock, None]
except TypeError as e: print(f"threading.RLock: {e}")
else: print(f"threading.RLock: no error")
# EOF

$ python3.10 up007_up045.py
Optional:
multiprocessing.JoinableQueue: no error
multiprocessing.Queue: no error
multiprocessing.SimpleQueue: no error
multiprocessing.dummy.Lock: no error
multiprocessing.dummy.RLock: no error
select.poll: no error
threading.Lock: no error
threading.RLock: no error

Union:
multiprocessing.JoinableQueue: no error
multiprocessing.Queue: no error
multiprocessing.SimpleQueue: no error
multiprocessing.dummy.Lock: no error
multiprocessing.dummy.RLock: no error
select.poll: no error
threading.Lock: no error
threading.RLock: no error

$ ruff --isolated check up007_up045.py --select UP007,UP045 --target-version py310 --fix
Found 16 errors (16 fixed, 0 remaining).

$ python3.10 up007_up045.py
Optional:
multiprocessing.JoinableQueue: unsupported operand type(s) for |: 'method' and 'NoneType'
multiprocessing.Queue: unsupported operand type(s) for |: 'method' and 'NoneType'
multiprocessing.SimpleQueue: unsupported operand type(s) for |: 'method' and 'NoneType'
multiprocessing.dummy.Lock: unsupported operand type(s) for |: 'builtin_function_or_method' and 'NoneType'
multiprocessing.dummy.RLock: unsupported operand type(s) for |: 'function' and 'NoneType'
select.poll: unsupported operand type(s) for |: 'builtin_function_or_method' and 'NoneType'
threading.Lock: unsupported operand type(s) for |: 'builtin_function_or_method' and 'NoneType'
threading.RLock: unsupported operand type(s) for |: 'function' and 'NoneType'

Union:
multiprocessing.JoinableQueue: unsupported operand type(s) for |: 'method' and 'NoneType'
multiprocessing.Queue: unsupported operand type(s) for |: 'method' and 'NoneType'
multiprocessing.SimpleQueue: unsupported operand type(s) for |: 'method' and 'NoneType'
multiprocessing.dummy.Lock: unsupported operand type(s) for |: 'builtin_function_or_method' and 'NoneType'
multiprocessing.dummy.RLock: unsupported operand type(s) for |: 'function' and 'NoneType'
select.poll: unsupported operand type(s) for |: 'builtin_function_or_method' and 'NoneType'
threading.Lock: unsupported operand type(s) for |: 'builtin_function_or_method' and 'NoneType'
threading.RLock: unsupported operand type(s) for |: 'function' and 'NoneType'
```

`threading.Lock` and `multiprocessing.dummy.Lock` became classes in Python 3.13, so the rules shouldn’t be suppressed for them with a recent enough target version. That is, the current behavior produces true positives for Python ≥ 3.13, but fixing the previous issue simplistically might make this produce false negatives. [Example](https://play.ruff.rs/b73c0855-3e0d-40cd-a251-9aa8fb80d8c3):
```console
$ cat >up007_up045_py313.py <<'# EOF'
from multiprocessing.dummy import Lock as DummyLock
from threading import Lock
from typing import Optional, Union

o0: Optional[Lock]
o1: Optional[DummyLock]
u0: Union[Lock, None]
u1: Union[DummyLock, None]
# EOF

$ python3.13 up007_up045_py313.py; echo $?
0

$ ruff --isolated check up007_up045_py313.py --select UP007,UP045 --target-version py313 --fix
Found 4 errors (4 fixed, 0 remaining).

$ python3.13 up007_up045_py313.py; echo $?
0
```

### Version

ruff 0.12.2 (9bee8376a 2025-07-03)

---

_Comment by @AlexWaygood on 2025-07-04 14:23_

> `typing.NamedTuple` became a class in Python 3.14,

no it didn't:

```pycon
~/dev/cpython (main)⚡ % ./python.exe                                  
Python 3.15.0a0 (heads/main:93263d43141, Jul  4 2025, 15:20:06) [Clang 17.0.0 (clang-1700.0.13.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import typing
>>> type(typing.NamedTuple)
<class 'function'>
```

But you're correct that `x: NamedTuple | None` won't fail on Python 3.14+, due to [PEP 649](https://peps.python.org/pep-0649/).

---

_Comment by @ntBre on 2025-07-04 14:42_

This came up before for `threading.RLock` too: #17711. Do we want to suppress all of these? I thought the consensus before was that we didn't, but looking back at the thread there wasn't as much discussion as I remembered. 

---

_Label `rule` added by @MichaReiser on 2025-07-07 13:43_

---

_Label `needs-decision` added by @MichaReiser on 2025-07-07 13:43_

---
