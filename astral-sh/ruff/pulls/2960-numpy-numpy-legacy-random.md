```yaml
number: 2960
title: "[`numpy`] numpy-legacy-random"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: numpy-legacy-random-npy002
created_at: 2023-02-16T17:06:20Z
updated_at: 2023-02-17T06:44:08Z
url: https://github.com/astral-sh/ruff/pull/2960
synced_at: 2026-01-12T04:39:44Z
```

# [`numpy`] numpy-legacy-random

---

_Pull request opened by @sbrugman on 2023-02-16 17:06_

The new `Generator` in NumPy uses bits provided by [PCG64](https://numpy.org/doc/stable/reference/random/bit_generators/pcg64.html#numpy.random.PCG64) which has better statistical properties than the legacy [MT19937](https://numpy.org/doc/stable/reference/random/bit_generators/mt19937.html#numpy.random.MT19937) used in [RandomState](https://numpy.org/doc/stable/reference/random/legacy.html#numpy.random.RandomState). Global random functions can also be problematic with parallel processing.

This rule is probably quite useful for data scientists (perhaps in combination with `nbqa`)

References:
- [Legacy Random Generation](https://numpy.org/doc/stable/reference/random/legacy.html#legacy)
- [Random Sampling](https://numpy.org/doc/stable/reference/random/index.html#random-quick-start)
- [Using PyTorch + NumPy? You're making a mistake.](https://tanelp.github.io/posts/a-bug-that-plagues-thousands-of-open-source-ml-projects/)

---

_Comment by @sbrugman on 2023-02-16 21:45_

(Implements [flake8-numpy-random](https://github.com/aevtikheev/flake8-numpy-random) apparently, NPR001 => NPY002)

---

_Label `rule` added by @charliermarsh on 2023-02-16 23:20_

---

_Merged by @charliermarsh on 2023-02-17 02:06_

---

_Closed by @charliermarsh on 2023-02-17 02:06_

---

_Branch deleted on 2023-02-17 06:44_

---
