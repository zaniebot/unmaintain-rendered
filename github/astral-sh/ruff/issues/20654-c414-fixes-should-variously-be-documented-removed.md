---
number: 20654
title: C414 fixes should variously be documented, removed, and marked as safe
type: issue
state: open
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-09-30T21:48:05Z
updated_at: 2025-10-01T01:31:37Z
url: https://github.com/astral-sh/ruff/issues/20654
synced_at: 2026-01-07T13:12:16-06:00
---

# C414 fixes should variously be documented, removed, and marked as safe

---

_Issue opened by @dscorbett on 2025-09-30 21:48_

### Summary

The fix for [`unnecessary-double-cast-or-process` (C414)](https://docs.astral.sh/ruff/rules/unnecessary-double-cast-or-process/) is marked as unsafe. The documentation says this is because comments may be removed, which is true but not the main reason. Many of the fixes could actually be marked as safe, others should be suppressed, and the rest should be documented.

To avoid changing behavior by introducing or suppressing a `TypeError`, the fix (or the rule itself) should be suppressed when one of these is true:
* The inner function has an unknown keyword argument.
* The inner function has the wrong number of positional arguments.
* The inner function has zero arguments and the outer function requires an argument. (`sorted` is the only outer function that requires an argument.)
* The inner function has a double-starred argument but doesn’t accept keyword arguments. (`sorted` is the only one that accepts keyword arguments.)

[Examples](https://play.ruff.rs/5be686f2-b5a5-4b8a-b598-cd583a458675):
```console
$ cat >c414_1.py <<'# EOF'
try: print(list(list("abc", error=True)))
except TypeError as e: print(e)

try: print(list(list("abc", True)))
except TypeError as e: print(e)

try: print(set(sorted()))
except TypeError as e: print(e)

try: print(sorted(tuple()))
except TypeError as e: print(e)

try: print(list(list("abc", **{"error": True})))
except TypeError as e: print(e)
# EOF

$ python c414_1.py
list() takes no keyword arguments
list expected at most 1 argument, got 2
sorted expected 1 argument, got 0
[]
list() takes no keyword arguments

$ ruff --isolated check --select C414 c414_1.py --unsafe-fixes --fix
Found 5 errors (5 fixed, 0 remaining).

$ python c414_1.py
['a', 'b', 'c']
['a', 'b', 'c']
set()
sorted expected 1 argument, got 0
['a', 'b', 'c']
```

Many combinations of functions are safe to fix. [All of these](https://play.ruff.rs/8c3e70ad-ad3c-4cd8-aa08-3a3bbe33b093) are safe:
```console
$ cat >c414_2.py <<'# EOF'
print(list(list("LL")))
print(list(tuple("LT")))
print(tuple(list("TL")))
print(tuple(tuple("TT")))
print(set(set("SS")))
print(set(list("SL")))
print(set(tuple("ST")))
print(sorted(list("~L")))
print(sorted(tuple("~T")))
print(sorted(sorted("~!")))
# EOF

$ python c414_2.py
['L', 'L']
['L', 'T']
('T', 'L')
('T', 'T')
{'S'}
{'S', 'L'}
{'S', 'T'}
['L', '~']
['T', '~']
['!', '~']

$ ruff --isolated check --select C414 c414_2.py --unsafe-fixes --fix
Found 10 errors (10 fixed, 0 remaining).

$ python c414_2.py
['L', 'L']
['L', 'T']
('T', 'L')
('T', 'T')
{'S'}
{'L', 'S'}
{'T', 'S'}
['L', '~']
['T', '~']
['!', '~']
```

That list notwithstanding, the fix is still unsafe when one of these is true:
* A comment is deleted.
* The combination of `sorted` and `sorted` uses keyword arguments with side effects.
* The outer function is `sorted`, the inner function is `list` or `tuple`, and the argument is starred.
* The outer and inner functions respectively are:
  * `sorted` and `reversed`
  * `set` and `reversed`
  * `set` and `sorted`

[Examples](https://play.ruff.rs/81de048a-b41a-4dcf-a2bf-ee0d743976d7):
```console
$ cat >c414_3.py <<'# EOF'
keys = iter([hex, lambda x: len(str(x))])
print(sorted(sorted([True, 2, 1], key=next(keys)), key=next(keys)))

try: print(sorted(list(*())))
except TypeError as e: print(e)

try: print(sorted(tuple(*())))
except TypeError as e: print(e)

print(sorted(reversed([1, True])))

print(set(reversed([1, True])))

try: print(set(sorted([t"1", t"2"])))
except TypeError as e: print(e)
# EOF

$ python3.14 c414_3.py
[1, 2, True]
[]
[]
[True, 1]
{True}
'<' not supported between instances of 'string.templatelib.Template' and 'string.templatelib.Template'

$ ruff --isolated check --select C414 c414_3.py --unsafe-fixes --fix --target-version py314 --preview
Found 6 errors (6 fixed, 0 remaining).

$ python3.14 c414_3.py
[True, 1, 2]
sorted expected 1 argument, got 0
sorted expected 1 argument, got 0
[1, True]
{1}
{Template(strings=('1',), interpolations=()), Template(strings=('2',), interpolations=())}
```


### Version

ruff 0.13.2 (b0bdf0334 2025-09-25)

---

_Label `fixes` added by @amyreese on 2025-10-01 01:31_

---
