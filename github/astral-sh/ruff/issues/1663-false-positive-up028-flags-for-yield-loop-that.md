---
number: 1663
title: "False positive: `UP028` flags `for-yield` loop that can't be rewritten into a `yield from`"
type: issue
state: closed
author: woodruffw
labels:
  - bug
assignees: []
created_at: 2023-01-05T15:23:55Z
updated_at: 2023-01-06T22:20:33Z
url: https://github.com/astral-sh/ruff/issues/1663
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive: `UP028` flags `for-yield` loop that can't be rewritten into a `yield from`

---

_Issue opened by @woodruffw on 2023-01-05 15:23_

Thanks again for the excellent tool! This time I think I have a real false positive 🙂 

When running the latest `ruff` with `pyupgrade` rules enabled on the following code:

```python
    def resolve_all(
        self, reqs: Iterator[Requirement]
    ) -> Iterator[tuple[Requirement, list[Dependency]]]:
        """
        Resolve a collection of `Requirement`s into their respective `Dependency` sets.

        `DependencyResolver` implementations can override this implementation with
        a more optimized one.
        """
        for req in reqs:
            yield (req, self.resolve(req))
```

I get the following `UP028` lint result:

```console
pip_audit/_dependency_source/interface.py:87:9: UP028 Replace `yield` over `for` loop with `yield from`
```

...which, when I auto-apply it with `--fix`, produces the following:

```python
    def resolve_all(
        self, reqs: Iterator[Requirement]
    ) -> Iterator[tuple[Requirement, list[Dependency]]]:
        """
        Resolve a collection of `Requirement`s into their respective `Dependency` sets.

        `DependencyResolver` implementations can override this implementation with
        a more optimized one.
        """
        yield from reqs
```

but this is no longer correct: the type signature no longer matches (we're now returning an `Iterator[Requirement]` instead of an `Iterator[tuple[Requirement, list[Dependency]]]`, and an important behavioral component of the function (calling `self.resolve(req)`) has been erased.

More generally, I don't think this particular `for-yield` loop can be re-written into a `yield from` -- I _think_ the only valid rewrites of that sort are "trivial" loop bodies like these:

```python
# original
for x in iter:
    yield iter
    
# rewritten
yield from x
```

(more precisely the loop binding needs to be the only thing yielded, but I'm not sure if `ruff` has that kind of granularity 🙂)

Runtime context:

```
$ ruff --version
ruff 0.0.211

$ python -V
Python 3.7.15
```

---

_Comment by @woodruffw on 2023-01-05 15:24_

Likely introduction point here: https://github.com/charliermarsh/ruff/pull/1544 (cc @colin99d)

---

_Comment by @charliermarsh on 2023-01-05 16:02_

Definitely a false positive, sorry about that!

---

_Label `bug` added by @charliermarsh on 2023-01-05 16:02_

---

_Comment by @charliermarsh on 2023-01-05 16:19_

(Will fix this today, just an oversight + weak test coverage.)

---

_Comment by @woodruffw on 2023-01-05 16:27_

No problem at all! Thanks for the fast response.

---

_Comment by @colin99d on 2023-01-05 16:29_

@charliermarsh I went about this by checking these two items:
    yield_items: Vec<String>, (the variables after the yield)
    target_items: Vec<String>, (the variables after for)
If they were not identical I did not attempt to fix.

---

_Comment by @charliermarsh on 2023-01-05 16:34_

Yeah, we do have that logic, it's just not robust to `yield` statements that include non-trivial names.

---

_Referenced in [astral-sh/ruff#1665](../../astral-sh/ruff/pulls/1665.md) on 2023-01-05 17:14_

---

_Closed by @charliermarsh on 2023-01-05 17:14_

---

_Comment by @woodruffw on 2023-01-06 22:20_

Thanks for the quick fix!

---
