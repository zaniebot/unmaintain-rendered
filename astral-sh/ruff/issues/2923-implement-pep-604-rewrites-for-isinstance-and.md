```yaml
number: 2923
title: "Implement PEP 604 rewrites for `isinstance` and `issubclass` checks"
type: issue
state: closed
author: michaeloliverx
labels:
  - rule
  - fixes
assignees: []
created_at: 2023-02-15T14:41:49Z
updated_at: 2023-03-03T22:14:32Z
url: https://github.com/astral-sh/ruff/issues/2923
synced_at: 2026-01-10T11:09:45Z
```

# Implement PEP 604 rewrites for `isinstance` and `issubclass` checks

---

_Issue opened by @michaeloliverx on 2023-02-15 14:41_

PEP 604 introduced simplified syntax for union types e.g. `var: str | int` for which we already have support for in #369. It also allows this syntax to be used [inside `isinstance` and `issubclass` checks](https://peps.python.org/pep-0604/#isinstance-and-issubclass), it would be great if these could be autofixed too!

```python
# before
isinstance("", (int, str))
# after
isinstance("", int | str)


# before
issubclass(bool, (int, float))
# after
issubclass(bool, int | float)

```






---

_Label `rule` added by @charliermarsh on 2023-02-15 14:44_

---

_Label `autofix` added by @charliermarsh on 2023-02-15 14:44_

---

_Comment by @bluetech on 2023-02-15 15:03_

Not that it affects the usefulness of such a rule, but note that `|` is slower:

```
$ python -m timeit 'isinstance("", (int, str))'
2000000 loops, best of 5: 103 nsec per loop

$ python -m timeit 'isinstance("", int | str)'
1000000 loops, best of 5: 217 nsec per loop
```

This is on Python 3.10, might be faster on newer versions.

---

_Comment by @michaeloliverx on 2023-02-15 15:23_

> Not that it affects the usefulness of such a rule, but note that `|` is slower:
> 
> ```
> $ python -m timeit 'isinstance("", (int, str))'
> 2000000 loops, best of 5: 103 nsec per loop
> 
> $ python -m timeit 'isinstance("", int | str)'
> 1000000 loops, best of 5: 217 nsec per loop
> ```
> 
> This is on Python 3.10, might be faster on newer versions.

Interesting! Definitely worth putting a note beside the rule.

---

_Comment by @wbolster on 2023-02-15 16:56_

python 3.11 has optimizations related to exactly this; see https://github.com/python/cpython/issues/91603

on python 3.10:

``` shell
$ a='isinstance("", (int, str))'
$ b='isinstance("", int | str)'
$ python3.10 -m timeit "$a"; python3.10 -m timeit "$b"
5000000 loops, best of 5: 48.6 nsec per loop
2000000 loops, best of 5: 108 nsec per loop
```

python 3.11 is significantly faster in both cases, and the difference between the two approaches has shrunk:

``` shell
$ python3.11 -m timeit "$a"; python3.11 -m timeit "$b"
5000000 loops, best of 5: 40.6 nsec per loop
5000000 loops, best of 5: 63.2 nsec per loop
```

note that `isinstance()` checks (which in simple cases boil down to a pointer comparison in cpython) are very unlikely to dominate any real-world workloads.

---

_Comment by @martinlehoux on 2023-02-27 23:02_

Can I take a look at this one? 

---

_Comment by @charliermarsh on 2023-02-27 23:09_

Sure! We can probably make it part of `UP007`.

---

_Comment by @Skylion007 on 2023-02-28 15:38_

@charliermarsh I'd be in favor of separating it out into it's on rule code as one could imagine stylistically a user preferring this syntax, but not like replacing Union with `|` and vice versa.

---

_Comment by @charliermarsh on 2023-02-28 17:13_

@Skylion007 - I'm a little torn because that rule is intended to capture PEP 604 adoption / usage, and this behavior _is_ explicitly part of [PEP 604](https://peps.python.org/pep-0604/#isinstance-and-issubclass).

---

_Comment by @martinlehoux on 2023-02-28 18:32_

It's been a while since I contributed last, so I don't remember everything. How would this interact with SIM101 rewriting `isinstance(a, int) or isinstance(a, float)` to `isinstance(a, (int, float))`?

---

_Comment by @charliermarsh on 2023-02-28 18:33_

It's okay to "ignore" that, since if both rules are enabled, we'd end up correcting `isinstance(a, (int, float))` to `isinstance(a, int | float)` in a second pass (we fix iteratively until there are no more fixable rules).

---

_Comment by @martinlehoux on 2023-02-28 19:57_

I have a first draft, but not sure about code architecture: it doesn't to fit in the same function / ast level

---

_Comment by @charliermarsh on 2023-03-03 22:14_

This was added in https://github.com/charliermarsh/ruff/pull/3280.

---

_Closed by @charliermarsh on 2023-03-03 22:14_

---
