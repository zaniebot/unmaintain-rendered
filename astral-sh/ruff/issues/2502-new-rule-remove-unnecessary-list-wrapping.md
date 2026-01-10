```yaml
number: 2502
title: "new rule: remove unnecessary list wrapping"
type: issue
state: closed
author: spaceone
labels:
  - rule
assignees: []
created_at: 2023-02-02T22:29:21Z
updated_at: 2023-05-18T15:59:51Z
url: https://github.com/astral-sh/ruff/issues/2502
synced_at: 2026-01-10T11:09:45Z
```

# new rule: remove unnecessary list wrapping

---

_Issue opened by @spaceone on 2023-02-02 22:29_

`''.join([chr(i) for i in range(128)])`
should be:
`''.join(chr(i) for i in range(128))`

---

_Comment by @ngnpope on 2023-02-02 23:45_

This was discussed way back when for `C407` in https://github.com/adamchainz/flake8-comprehensions/issues/156.

It was rejected because using a list comprehension was faster, and still is:

```python
# Python 3.10.9

In [1]: %timeit ",".join([str(i) for i in range(3)])
389 ns ± 1.79 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)

In [2]: %timeit ",".join([str(i) for i in range(300)])
17.9 µs ± 90.9 ns per loop (mean ± std. dev. of 7 runs, 100,000 loops each)

In [3]: %timeit ",".join(str(i) for i in range(3))
473 ns ± 1.68 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)

In [4]: %timeit ",".join(str(i) for i in range(300))
20.8 µs ± 21.4 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)

# Python 3.11.1

In [1]: %timeit ",".join([str(i) for i in range(3)])
384 ns ± 4.26 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)

In [2]: %timeit ",".join([str(i) for i in range(300)])
16.2 µs ± 169 ns per loop (mean ± std. dev. of 7 runs, 100,000 loops each)

In [3]: %timeit ",".join(str(i) for i in range(3))
501 ns ± 5.38 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)

In [4]: %timeit ",".join(str(i) for i in range(300))
19.9 µs ± 268 ns per loop (mean ± std. dev. of 7 runs, 100,000 loops each)
```

---

_Comment by @charliermarsh on 2023-02-02 23:57_

So strange that that's faster! Intuitively I would've expected the opposite.

---

_Comment by @spaceone on 2023-02-03 00:26_

ah, I saw this in the past as well.

The reason:
https://github.com/adamchainz/flake8-comprehensions/issues/156#issuecomment-525719846
> because join internally converts all generators to lists

But I think the performance aspect doesn't matter here, it's minimal in the recent python versions.
code readability is more important. If performance matters use rust instead of python :-P

So a new rule which can do this and be deactivated by folks who want the faster version.


---

_Label `rule` added by @charliermarsh on 2023-02-03 18:16_

---

_Comment by @charliermarsh on 2023-02-03 18:17_

I might err on the side of omitting if `flake8-comprehensions` intentionally omits it.

---

_Comment by @charliermarsh on 2023-02-03 18:17_

(Even though I personally wouldn't mind it.)

---

_Comment by @ngnpope on 2023-02-04 08:57_

Out of interest, the performance dip for 3.11 in the third test (small generator) is likely caused by https://github.com/python/cpython/issues/100762.

Even then, it'll still be slower. If we did implement this, it should be a different rule to `C407`. But it would be good to have a rule that enforces a list (comprehension) too given that's most optimal.

---

_Comment by @charliermarsh on 2023-05-18 15:59_

I'm going to close for now, we can re-open if `flake8-comprehensions` changes their stance.

---

_Closed by @charliermarsh on 2023-05-18 15:59_

---
