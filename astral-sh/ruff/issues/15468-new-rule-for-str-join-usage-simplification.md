```yaml
number: 15468
title: New rule for str.join usage simplification
type: issue
state: open
author: victorsamun
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-01-14T05:11:36Z
updated_at: 2025-03-11T08:52:43Z
url: https://github.com/astral-sh/ruff/issues/15468
synced_at: 2026-01-12T15:54:54Z
```

# New rule for str.join usage simplification

---

_@victorsamun_

Method `str.join` takes an iterable, so we don't need create list or tuple explicitly.

For example, `", ".join(list(data))` and `", ".join(tuple(data))` simplifies to `", ".join(data)`.
This can be done automatically.

---

_Label `rule` added by @MichaReiser on 2025-01-14 07:50_

---

_Label `needs-decision` added by @MichaReiser on 2025-01-14 07:50_

---

_Comment by @dylwil3 on 2025-01-14 12:01_

A related thing we could do is extend [`C419`](https://docs.astral.sh/ruff/rules/unnecessary-comprehension-in-call/) to include this string method, and maybe other builtins and standard library methods that accept iterables. That would cover some instances of the proposed lint.

---

_Comment by @nijel on 2025-03-10 13:41_

Unnecessary list for `str.join` is indeed often used in comprehension:

```py
        return b"\r\n".join(
            [
                key.encode("ascii") + b": " + value.encode("latin-1")
                for key, value in self.headers.items()
            ]
        )
```

So C419 might be the good place to land this.

---

_Comment by @nijel on 2025-03-10 18:49_

Hmm, it turns out to be actually a bad idea. Passing list to `str.join` is more effective than iterator because it does two passes over the sequence. Also see https://stackoverflow.com/a/9061024. I've verified the numbers in Python 3.13 and 3.14:

```console
(py3.14) nijel:/tmp$ python -m timeit '"".join([str(n) for n in range(1000)])'
5000 loops, best of 5: 79.8 usec per loop
(py3.14) nijel:/tmp$ python -m timeit '"".join(str(n) for n in range(1000))'
5000 loops, best of 5: 102 usec per loop
```

---

_Comment by @MichaReiser on 2025-03-11 07:41_

Related https://github.com/astral-sh/ruff/issues/12912 and https://github.com/astral-sh/ruff/issues/10838

---

_Comment by @nijel on 2025-03-11 08:52_

I've already spent a lot in this rabbit hole to say it's a bit different. With `sum`/`min`/`max`/`all`/`any` there is at least something to gain (less memory allocations). For `join` there is nothing to gain as it internally converts iterator to a list as the first step (because it needs two passes on the data).

---
