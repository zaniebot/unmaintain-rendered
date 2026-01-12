```yaml
number: 6289
title: Add rule to upgrade type alias annotations to keyword (UP040)
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: rule/type-alias-to-type
created_at: 2023-08-02T23:44:23Z
updated_at: 2023-08-09T02:10:53Z
url: https://github.com/astral-sh/ruff/pull/6289
synced_at: 2026-01-12T02:52:03Z
```

# Add rule to upgrade type alias annotations to keyword (UP040)

---

_Pull request opened by @zanieb on 2023-08-02 23:44_

Adds rule to convert type aliases defined with annotations i.e. `x: TypeAlias = int` to the new PEP-695 syntax e.g. `type x = int`.

Does not support using new generic syntax for type variables, will be addressed in a follow-up.
Added as part of pyupgrade — ~the code 100 as chosen to avoid collision with real pyupgrade codes~.

Part of #4617 
Builds on #5062 

---

_@zanieb reviewed on 2023-08-02 23:45_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/ruff/RUF017.py`:11 on 2023-08-02 23:45_

This case is where the `type` keyword shines but seems much harder to auto-fix?

---

_Renamed from "Add rule to upgrade type alias annotations to keyword" to "Add rule to upgrade type alias annotations to keyword (RUF017)" by @zanieb on 2023-08-02 23:46_

---

_@charliermarsh reviewed on 2023-08-03 00:05_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/ruff/RUF017.py`:11 on 2023-08-03 00:05_

If we could detect that `T` is _only_ used here, would that be sufficient to allow an autofix?

---

_@charliermarsh reviewed on 2023-08-03 00:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:810 on 2023-08-03 00:06_

I would still vote for `NonPEP695TypeAlias`. It's more consistent IMO, as it matches `NonPEP585Annotation` and `NonPEP604Annotation`. 

---

_@charliermarsh reviewed on 2023-08-03 00:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/type_alias_annotation.rs`:74 on 2023-08-03 00:06_

Nit: `TODO(zanie):` for consistency (so that it matches on case-sensitive searches).

---

_@charliermarsh reviewed on 2023-08-03 00:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/type_alias_annotation.rs`:37 on 2023-08-03 00:07_

I think we tend not to add trailing punctuation unless the message spans more than one sentence.

---

_@charliermarsh reviewed on 2023-08-03 00:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/type_alias_annotation.rs`:41 on 2023-08-03 00:08_

Nit: I think omitting "instead" or even writing as "Replace with `type` keyword" could be more consistent with how this title is worded in other rules.

---

_@charliermarsh reviewed on 2023-08-03 00:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:810 on 2023-08-03 00:10_

I'm tempted to say we should just put this in the `pyupgrade` category... We've added some other rules in there that don't originate in `pyupgrade` but instead fall into the same semantic category (code upgrades based on increasing Python version). What do you think?

---

_Comment by @github-actions[bot] on 2023-08-03 00:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     10.5±0.35ms     3.9 MB/sec    1.00     10.2±0.23ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.0±0.07ms     8.2 MB/sec    1.00      2.0±0.04ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00   226.9±14.55µs    13.0 MB/sec    1.03   232.8±18.11µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.14ms     5.8 MB/sec    1.01      4.5±0.25ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.02     14.3±0.38ms     2.8 MB/sec    1.00     14.1±0.58ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.06ms     4.7 MB/sec    1.00      3.5±0.07ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    476.3±9.26µs     6.2 MB/sec    1.00    473.9±7.04µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.5±0.16ms     4.0 MB/sec    1.00      6.2±0.11ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.18ms     5.6 MB/sec    1.00      7.2±0.12ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1498.2±34.03µs    11.1 MB/sec    1.00  1503.4±41.34µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.4±6.84µs    17.7 MB/sec    1.05   175.5±19.74µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.12ms     8.0 MB/sec    1.00      3.2±0.07ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.9±0.23ms     4.5 MB/sec    1.00      9.0±0.10ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1729.9±29.92µs     9.6 MB/sec    1.01  1749.7±31.72µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    192.9±6.37µs    15.3 MB/sec    1.01    194.1±5.53µs    15.2 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.08ms     6.7 MB/sec    1.01      3.8±0.14ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.14ms     3.3 MB/sec    1.00     12.2±0.26ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.2±0.04ms     5.2 MB/sec    1.00      3.2±0.04ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    397.0±7.19µs     7.4 MB/sec    1.00    392.7±7.79µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.6±0.08ms     4.6 MB/sec    1.00      5.5±0.11ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.11ms     6.2 MB/sec    1.00      6.6±0.06ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1340.2±18.96µs    12.4 MB/sec    1.00  1341.7±36.81µs    12.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    151.3±3.74µs    19.5 MB/sec    1.00    148.4±3.26µs    19.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.05ms     8.7 MB/sec    1.00      2.9±0.06ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb reviewed on 2023-08-03 00:16_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/ruff/RUF017.py`:11 on 2023-08-03 00:16_

Well I'm fine with making T an unused variable and dealing with that later. The part I don't know how to do is recursively find all `TypeVar` bindings used in a complex expression. I'm guessing we can use the visitor somehow or we already have some helpers?

---

_@zanieb reviewed on 2023-08-03 00:16_

---

_Review comment by @zanieb on `crates/ruff/src/codes.rs`:810 on 2023-08-03 00:16_

Sounds good to me. 

---

_@charliermarsh reviewed on 2023-08-03 00:36_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/ruff/RUF017.py`:11 on 2023-08-03 00:36_

I would probably write a custom visitor, like `NameFinder`, that just collects all the `TypeVar` instances.

---

_Renamed from "Add rule to upgrade type alias annotations to keyword (RUF017)" to "Add rule to upgrade type alias annotations to keyword (PY100)" by @zanieb on 2023-08-03 15:07_

---

_Marked ready for review by @zanieb on 2023-08-03 15:08_

---

_Renamed from "Add rule to upgrade type alias annotations to keyword (PY100)" to "Add rule to upgrade type alias annotations to keyword (UP100)" by @zanieb on 2023-08-03 15:09_

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:441 on 2023-08-03 15:16_

What's the motivation for UP100 in lieu of UP040? (All of these codes are made up so we don't have to worry about pyupgrade compatibility anyway.)

---

_@charliermarsh reviewed on 2023-08-03 15:16_

---

_Review comment by @zanieb on `crates/ruff/src/codes.rs`:441 on 2023-08-03 15:17_

Oh I didn't know they were made up

---

_@zanieb reviewed on 2023-08-03 15:17_

---

_Renamed from "Add rule to upgrade type alias annotations to keyword (UP100)" to "Add rule to upgrade type alias annotations to keyword (UP040)" by @zanieb on 2023-08-03 15:19_

---

_@charliermarsh approved on 2023-08-03 15:22_

---

_Merged by @zanieb on 2023-08-03 16:13_

---

_Closed by @zanieb on 2023-08-03 16:13_

---

_Branch deleted on 2023-08-03 16:13_

---

_Comment by @henryiii on 2023-08-08 20:08_

FYI, this was not added to PyUpgrade because the new syntax is not valid at runtime. For example:

```python
# file1.py
type newint = int
```

```python
# file2.py
from file1 import newint
isinstance(3, newint) # Doesn't work!
```

This "upgrade" is _only_ valid if you never use the TypeAlias at runtime. Which you can't tell statically (as shown above).

This is a bit safer than PyUpgrade, since you can disable it by code, but it's not a 1:1 upgrade like all the other upgrades.

Standalone example:

```console
$ docker run --rm -it python:3.12-rc
Python 3.12.0b4 (main, Jul 28 2023, 04:24:27) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
```
```pycon
>>> type newint = int
>>> isinstance(3, newint)
```
```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: isinstance() arg 2 must be a type, a tuple of types, or a union
```

---

_Comment by @charliermarsh on 2023-08-08 20:12_

Thanks! We can just limit this to `.pyi` files for now to be safe while we figure out more nuanced detection. (We can certainly detect whether an annotation is used at runtime within a file, so we could always apply this to private type variables as a suggested fix.) Wdyt @zanieb?


---

_Comment by @charliermarsh on 2023-08-09 02:10_

Created https://github.com/astral-sh/ruff/issues/6434 to track modifications.

---
