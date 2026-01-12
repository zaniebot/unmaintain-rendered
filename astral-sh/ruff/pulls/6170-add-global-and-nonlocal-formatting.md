```yaml
number: 6170
title: "Add `global` and `nonlocal` formatting"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/global-nonlocal
created_at: 2023-07-29T12:19:48Z
updated_at: 2023-08-16T07:10:00Z
url: https://github.com/astral-sh/ruff/pull/6170
synced_at: 2026-01-12T15:55:20Z
```

# Add `global` and `nonlocal` formatting

---

_@charliermarsh_

## Summary

Adds `global` and `nonlocal` formatting, without the "deviation from black" outlined in the linked issue, which I'll do separately.

See: https://github.com/astral-sh/ruff/issues/4798.

## Test Plan

Added a fixture in the Ruff-specific directory since the Black fixtures don't seem to cover this.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-29 12:19_

---

_Review requested from @konstin by @charliermarsh on 2023-07-29 12:19_

---

_Label `formatter` added by @charliermarsh on 2023-07-29 12:19_

---

_@charliermarsh reviewed on 2023-07-29 12:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_global.rs`:16 on 2023-07-29 12:20_

Straightforward I think because the grammar is so limited here? These _have_ to be identifiers, and they can't contain any content in-between.

---

_Comment by @github-actions[bot] on 2023-07-29 12:53_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.6±0.17ms     4.7 MB/sec    1.00      8.5±0.09ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1669.9±29.06µs    10.0 MB/sec    1.00  1665.1±31.76µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    180.6±2.94µs    16.3 MB/sec    1.01    182.7±5.28µs    16.1 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.11ms     7.0 MB/sec    1.00      3.6±0.07ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     11.3±0.14ms     3.6 MB/sec    1.00     11.3±0.11ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     5.9 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    381.6±1.34µs     7.7 MB/sec    1.00    382.6±0.94µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.1±0.09ms     5.0 MB/sec    1.00      5.0±0.05ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.08ms     6.8 MB/sec    1.00      6.0±0.04ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1242.7±7.66µs    13.4 MB/sec    1.00   1244.3±6.16µs    13.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    135.8±0.19µs    21.7 MB/sec    1.00    136.4±0.21µs    21.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.03ms     9.7 MB/sec    1.01      2.6±0.03ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.34ms     3.4 MB/sec    1.04     12.6±0.43ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.09ms     7.0 MB/sec    1.03      2.4±0.06ms     6.8 MB/sec
formatter/numpy/globals.py                 1.00   269.2±16.30µs    11.0 MB/sec    1.05   282.5±14.75µs    10.4 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.18ms     4.8 MB/sec    1.02      5.4±0.20ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.43ms     2.4 MB/sec    1.03     17.2±0.75ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.15ms     3.8 MB/sec    1.03      4.5±0.14ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   532.1±19.71µs     5.5 MB/sec    1.06   566.1±22.66µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.26ms     3.4 MB/sec    1.03      7.7±0.30ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.24ms     4.4 MB/sec    1.06      9.9±0.25ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1886.0±56.77µs     8.8 MB/sec    1.05  1989.7±58.01µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   225.1±12.24µs    13.1 MB/sec    1.03    232.5±9.71µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.16ms     6.3 MB/sec    1.06      4.3±0.14ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-29 13:04_

Line-breaking behavior in https://github.com/astral-sh/ruff/pull/6172.

---

_Comment by @charliermarsh on 2023-07-29 13:06_

Do we care about DRYing this up? If so, what would be the right home for that?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_global.rs`:5 on 2023-07-29 14:03_

```suggestion
use crate::{FormatNodeRule};
```

I believe these two should not be necessary if you import `prelude`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_global.rs`:1 on 2023-07-29 14:04_

```suggestion
use ruff_formatter::{format_args, write};
```

You should get these for free when importing the prelude.

---

_@MichaReiser approved on 2023-07-29 14:04_

Thank you. I'm surprised that there are no black tests... 


> Do we care about DRYing this up? If so, what would be the right home for that?

I do, for more complex implementation but the abstraction would probably require significantly more code than what we would safe. 

The way I would re-use the code here is by creating a `FormatNames` struct inside either of the file that accepts the names and implements `Format`.

---

_Merged by @charliermarsh on 2023-07-29 14:39_

---

_Closed by @charliermarsh on 2023-07-29 14:39_

---

_Branch deleted on 2023-07-29 14:39_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:10_

---
