```yaml
number: 4427
title: "[`pyupgrade`] Remove `keep-runtime-typing` setting"
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: main
head: charlie/pep
created_at: 2023-05-14T03:05:56Z
updated_at: 2023-05-20T21:51:44Z
url: https://github.com/astral-sh/ruff/pull/4427
synced_at: 2026-01-12T15:55:15Z
```

# [`pyupgrade`] Remove `keep-runtime-typing` setting

---

_@charliermarsh_

## Summary

This was implemented "by accident" before realizing that it's exactly equivalent to turning off two of our rules. (Pyupgrade doesn't have a generic mechanism for turning off individual rules, and so has dedicated flags for them.)


---

_Label `breaking` added by @charliermarsh on 2023-05-14 03:05_

---

_Label `configuration` added by @charliermarsh on 2023-05-14 03:05_

---

_Renamed from "Remove `keep-runtime-typing` setting" to "[`pyupgrade`] Remove `keep-runtime-typing` setting" by @charliermarsh on 2023-05-14 03:10_

---

_Merged by @charliermarsh on 2023-05-14 03:12_

---

_Closed by @charliermarsh on 2023-05-14 03:12_

---

_Branch deleted on 2023-05-14 03:12_

---

_Comment by @github-actions[bot] on 2023-05-14 03:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.6±0.03ms     3.0 MB/sec    1.00     13.6±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    415.9±0.55µs     7.1 MB/sec    1.00    414.4±1.01µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.02ms     4.5 MB/sec    1.00      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1442.8±2.45µs    11.5 MB/sec    1.00   1434.8±3.53µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.2±0.29µs    18.4 MB/sec    1.00    160.6±2.70µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.00      5.4±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1053.2±1.31µs    15.8 MB/sec    1.00   1057.7±0.73µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    106.9±0.35µs    27.6 MB/sec    1.01    107.5±0.32µs    27.5 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.3±0.14ms     2.5 MB/sec    1.00     16.1±0.20ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   482.4±10.04µs     6.1 MB/sec    1.00    481.0±5.68µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.06ms     3.8 MB/sec    1.00      6.8±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1683.0±13.39µs     9.9 MB/sec    1.01  1695.3±23.83µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.2±2.78µs    15.7 MB/sec    1.01    189.8±3.72µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.6±0.04ms     6.2 MB/sec    1.00      6.6±0.06ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1260.6±13.24µs    13.2 MB/sec    1.00  1261.9±14.90µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    129.0±1.84µs    22.9 MB/sec    1.00    129.5±1.78µs    22.8 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @layday on 2023-05-19 22:10_

I don't think the two mechanisms are exactly equivalent.  The benefit of having a stand-alone setting for it is that the rules will take effect immediately upon bumping the minimum supported Python version, whereas now the user will have to remember to un-exclude the rules they've been forced to exclude for runtime typing compatibility, and the burden of tracking compatibility per Python version will have fallen on them.

---

_Comment by @charliermarsh on 2023-05-20 02:39_

@layday - I understand that perspective, but these rules already take the minimum compatible version into account (and/or the required presence of `from __future__ import annotations`), so I don't believe users should have to consider their own minimum supported version at all when enabling or disabling them, or toggling the `keep-runtime-typing ` settings. There was more discussion of this here (https://github.com/charliermarsh/ruff/issues/3174), long ago -- but, e.g., the only reason to turn these rules off or set `keep-runtime-typing = true` would be if you had some application behavior that depended on the availability of runtime typing, and in that case, it seems like changing the Python version would be irrelevant to whether that decision changed.

As implemented in Ruff, this setting was exactly equivalent to enabling and disabling these rules. But I'm happy to continue discussing or reconsider if you have an example of where this would cause problems.


---

_Comment by @layday on 2023-05-20 07:27_

> [...] would be if you had some application behavior that depended on the availability of runtime typing, and in that case, it seems like changing the Python version would be irrelevant to whether that decision changed.

Ah, but the assumption is that the application is able to handle these new typing constructs at runtime, the only limitation being the lowest Python version that it supports, i.e. that evaluating stringified annotations does not raise.

> As implemented in Ruff, this setting was exactly equivalent to enabling and disabling these rules.

To clarify, bumping `target-version` to 3.10 would not have enabled either of these rules?

---

_Comment by @charliermarsh on 2023-05-20 15:24_

> To clarify, bumping target-version to 3.10 would not have enabled either of these rules?

That's correct. It _used_ to only affect the pre-3.10 branch, but I changed it in #2449. I may have misunderstood the motivation for the flag, and it's possible that our enforcement of it was incorrect, though I still don't fully understand why it exists. It seems like the example of a case in which users want to use this flag is on pre-3.9 code like:

```py
from __future__ import annotations
import pydantic

class User(pydantic.BaseModel):
    first_name: str | None = None
    last_name: str | None = None
```

But in this case, why is `__future__` annotations enabled? Is it not unsafe to use `__future__` annotations at all if you're relying on runtime typing in this way?


---

_Comment by @JacobHayes on 2023-05-20 21:51_

One might also use `from __future__ import annotations` to make recursive or cyclic type hints nicer, eg:
```python
from __future__ import annotations

import pydantic

class User(pydantic.BaseModel):
    friends: list[User]
```
([here's a non-contrived example](https://github.com/artigraph/artigraph/blob/golden/src/arti/fingerprints/__init__.py#L48))

---

This may be fairly minor, but if there are future syntax incompatible typing PEPs introduced other than PEP 585 and PEP 604, one other difference is that users (as opposed to ruff) will need to be responsible for updating their configs when making ruff updates in order to keep runtime typing.

---
