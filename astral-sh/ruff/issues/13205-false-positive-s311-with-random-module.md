---
number: 13205
title: "False positive S311 with `random` module"
type: issue
state: closed
author: DanielYang59
labels:
  - question
assignees: []
created_at: 2024-09-02T03:41:28Z
updated_at: 2024-09-02T09:29:22Z
url: https://github.com/astral-sh/ruff/issues/13205
synced_at: 2026-01-10T01:22:53Z
---

# False positive S311 with `random` module

---

_Issue opened by @DanielYang59 on 2024-09-02 03:41_

Hi ruff team,

I noticed `ruff` might flag the following code snippet with [S311 (suspicious-non-cryptographic-random-usage) ](https://docs.astral.sh/ruff/rules/suspicious-non-cryptographic-random-usage/) when using the built-in `random` module, but not with the random generator from `numpy`:
- Is the difference in behavior between `random` and `numpy` intended?
- Why this would be considered a possible cryptographic usage? It just generates an array of random integers.

With:
```python3
import random

import numpy as np


random_ints = [random.randint(0, 10) for _ in range(5)]  # triggers S311 flag

rng = np.random.default_rng()
random_ints_np_0 = [rng.integers(0, 10) for _ in range(5)]  # no S311 flag
random_ints_np_1 = rng.integers(0, 10, size=5)  # no S311 flag
```

I got:
```
>>> ruff check ruff-s311.py --select S311
simple-ruff-crpto.py:6:16: S311 Standard pseudo-random generators are not suitable for cryptographic purposes
  |
6 | random_ints = [random.randint(0, 10) for _ in range(5)]
  |                ^^^^^^^^^^^^^^^^^^^^^ S311
7 | 
8 | rng = np.random.default_rng()
  |

Found 1 error.
```

### Ruff version

`ruff 0.6.3`

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @MichaReiser on 2024-09-02 08:55_

> Is the difference in behavior between random and numpy intended?

I just checked, and `S311` does not support `numpy`, so the `numpy` calls aren't flagged. Adding support for `numpy` does make sense but I suspect that it may require type inference to reason about that `rng` is an instance of `np.random.default_rng`. 


> Why this would be considered a possible cryptographic usage? It just generates an array of random integers.

S311 flags all usages of random because it's hard for a static analysis tool to determine if the random number is used in a cryptographic context or not.


---

_Label `question` added by @MichaReiser on 2024-09-02 08:55_

---

_Comment by @DanielYang59 on 2024-09-02 09:29_

Thanks a lot for the explanation!

> S311 flags all usages of random because it's hard for a static analysis tool to determine if the random number is used in a cryptographic context or not.

I fully understand the difficulty, but still feels it's a bit unconvincing to suspect all usage of `random` to be cryptographic without any evidence (the var name being `token/password` for example). But I guess it's better safe than sorry, and one could globally ignore this rule if there are too many false positives.

---

_Closed by @DanielYang59 on 2024-09-02 09:29_

---
