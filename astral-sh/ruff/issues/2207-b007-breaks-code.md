---
number: 2207
title: B007 breaks code
type: issue
state: closed
author: spaceone
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-01-26T17:43:46Z
updated_at: 2023-02-03T01:59:48Z
url: https://github.com/astral-sh/ruff/issues/2207
synced_at: 2026-01-10T01:22:40Z
---

# B007 breaks code

---

_Issue opened by @spaceone on 2023-01-26 17:43_

```
$ cat foo.py
for foo, bar, baz in (['1', '2', '3'], ):
    if foo or baz:
        break

int(bar)
$ ruff --select B007 foo.py
foo.py:1:10: B007 Loop control variable `bar` not used within loop body
Found 1 error.
1 potentially fixable with the --fix option.
$ ruff --select B007 --fix foo.py
Found 1 error (1 fixed, 0 remaining).
$ cat foo.py
for foo, _bar, baz in (['1', '2', '3'], ):
    if foo or baz:
        break

int(bar)
$ python3 foo.py
Traceback (most recent call last):
  File "foo.py", line 5, in <module>
    int(bar)
NameError: name 'bar' is not defined
```

---

_Label `autofix` added by @charliermarsh on 2023-01-26 17:44_

---

_Label `bug` added by @charliermarsh on 2023-01-26 22:00_

---

_Comment by @dycw on 2023-01-27 02:59_

Isn't this the right behaviour? `bar` is not used in the loop, it's used outside the loop.

---

_Comment by @charliermarsh on 2023-01-27 03:02_

If the thing you're iterating over is non-empty, then when the loop finishes, `bar` will continue to exist, taking on the last value (in this case, `'2'`).

---

_Comment by @charliermarsh on 2023-01-28 12:33_

It's possible for us to avoid this, but it requires some refactoring. We do track whether variables are used, even after their declaration site, but this specific check doesn't have access to that info.

---

_Referenced in [astral-sh/ruff#2397](../../astral-sh/ruff/issues/2397.md) on 2023-01-31 14:22_

---

_Comment by @spaceone on 2023-02-01 15:02_

another real life example which was caused by this:
```
        def _fake_func(self, iterator, *args):
-               for args in iterator:
+               for _args in iterator:
                        break
                yield function(self, *args)
```

---

_Referenced in [astral-sh/ruff#2509](../../astral-sh/ruff/pulls/2509.md) on 2023-02-03 01:55_

---

_Closed by @charliermarsh on 2023-02-03 01:59_

---

_Referenced in [astral-sh/ruff#2511](../../astral-sh/ruff/pulls/2511.md) on 2023-02-03 03:25_

---
