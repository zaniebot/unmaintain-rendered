```yaml
number: 4067
title: Unify positional and keyword arguments when checking for missing arguments in docstring
type: pull_request
state: merged
author: evanrittenhouse
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: 4047_docs_kwargs
created_at: 2023-04-22T22:09:44Z
updated_at: 2023-06-15T21:15:54Z
url: https://github.com/astral-sh/ruff/pull/4067
synced_at: 2026-01-12T15:55:14Z
```

# Unify positional and keyword arguments when checking for missing arguments in docstring

---

_@evanrittenhouse_

Fixes #4047. 

We were previously at risk of false positives when an argument was specified in the `Arguments` section and not in `Keyword Arguments`, or vice versa. Rather than checking arguments on a section-by-section basis, this PR combines positional and keyword arguments into one set then checks that superset against the function signature.

It also adds `Other Params/Other Args/Other Arguments` identifiers to the `SectionKind` enum.

---

_Renamed from "4047 docs kwargs" to "Unify positional and keyword arguments when checking for missing arguments in docstring" by @evanrittenhouse on 2023-04-22 22:27_

---

_Comment by @github-actions[bot] on 2023-04-22 22:38_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -3, 0 error(s))

<details><summary>bokeh (+3, -3)</summary>
<p>

```diff
- src/bokeh/application/application.py:90:9: D417 Missing argument description in the docstring: `metadata`
+ src/bokeh/application/handlers/server_lifecycle.py:57:9: D417 Missing argument description in the docstring: `package`
+ src/bokeh/application/handlers/server_request_handler.py:59:9: D417 Missing argument description in the docstring: `package`
- src/bokeh/core/enums.py:198:5: D417 Missing argument descriptions in the docstring: `case_sensitive`, `quote`
+ src/bokeh/models/plots.py:124:9: D417 Missing argument description in the docstring: `*args`
- src/bokeh/models/plots.py:124:9: D417 Missing argument descriptions in the docstring: `**kwargs`, `*args`
```

</p>
</details>
<details><summary>zulip (+1, -0)</summary>
<p>

```diff
+ zerver/lib/tex.py:11:5: D417 Missing argument descriptions in the docstring: `is_inline`, `tex`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.6±0.06ms     2.6 MB/sec    1.00     15.4±0.04ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.00ms     4.3 MB/sec    1.00      3.9±0.00ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.6±2.04µs     6.9 MB/sec    1.00    429.2±1.23µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.01ms     3.9 MB/sec    1.00      6.6±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.01ms     5.0 MB/sec    1.00      8.1±0.01ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1771.4±2.08µs     9.4 MB/sec    1.00   1775.5±2.74µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.4±0.78µs    15.9 MB/sec    1.00    184.8±0.54µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.5±0.02ms     6.3 MB/sec    1.03      6.7±0.01ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1263.7±1.23µs    13.2 MB/sec    1.01   1270.2±1.85µs    13.1 MB/sec
parser/numpy/globals.py                    1.01    127.0±0.42µs    23.2 MB/sec    1.00    126.1±0.85µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.17ms     2.4 MB/sec    1.01     17.2±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.01      4.4±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    536.0±6.06µs     5.5 MB/sec    1.01    542.9±7.17µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.08ms     3.5 MB/sec    1.00      7.3±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.9±0.06ms     4.6 MB/sec    1.00      8.8±0.07ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1935.2±29.80µs     8.6 MB/sec    1.00  1933.8±17.50µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    215.0±3.76µs    13.7 MB/sec    1.04   224.1±16.98µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.04ms     6.3 MB/sec    1.00      4.0±0.06ms     6.3 MB/sec
parser/large/dataset.py                    1.01      6.8±0.05ms     5.9 MB/sec    1.00      6.8±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1298.7±13.78µs    12.8 MB/sec    1.00  1302.5±15.60µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    130.7±1.90µs    22.6 MB/sec    1.00    131.1±1.85µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.7 MB/sec    1.00      2.9±0.03ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @evanrittenhouse on 2023-04-23 00:17_

---

_Renamed from "Unify positional and keyword arguments when checking for missing arguments in docstring" to "Unify positional and keyword arguments when checking for missing arguments in Google-style docstring" by @evanrittenhouse on 2023-04-23 00:30_

---

_Renamed from "Unify positional and keyword arguments when checking for missing arguments in Google-style docstring" to "Unify positional and keyword arguments when checking for missing arguments in docstring" by @evanrittenhouse on 2023-04-23 00:30_

---

_@charliermarsh reviewed on 2023-04-23 19:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:870 on 2023-04-23 19:58_

Is this unchanged, apart from the use of the named parameter instead of the capture group index?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:1078 on 2023-04-23 20:00_

Previously, this only ran if the `Args` section was present (so if `Args` was omitted entirely, we didn't raise an error). Is that still the case?

---

_@charliermarsh reviewed on 2023-04-23 20:00_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:1078 on 2023-04-23 20:20_

No - I'll add another check in to do so. 

---

_@evanrittenhouse reviewed on 2023-04-23 20:20_

---

_@evanrittenhouse reviewed on 2023-04-23 20:22_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:870 on 2023-04-23 20:22_

Yup!

---

_@evanrittenhouse reviewed on 2023-04-23 20:23_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:870 on 2023-04-23 20:23_

~~If you'd prefer to keep it unnamed that's fine too, happy to remove it. I just prefer using named groups if I need to pull one out.~~ Reverted to use the index

---

_@evanrittenhouse reviewed on 2023-04-23 20:26_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:1078 on 2023-04-23 20:26_

Just wondering - is there a reason we prioritize `Args` like that?

---

_@charliermarsh reviewed on 2023-04-25 05:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:1078 on 2023-04-25 05:13_

Ah sorry, I wasn't clear -- what I meant was: we should only trigger if at least _one_ arguments-related section is present. It doesn't have to be `Args`.

---

_Label `bug` added by @charliermarsh on 2023-04-25 05:14_

---

_Label `docstring` added by @charliermarsh on 2023-04-25 05:14_

---

_Merged by @charliermarsh on 2023-04-25 05:32_

---

_Closed by @charliermarsh on 2023-04-25 05:32_

---

_Branch deleted on 2023-06-15 21:15_

---
