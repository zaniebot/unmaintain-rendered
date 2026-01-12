```yaml
number: 4156
title: "Use --filter=blob:none to clone CPython faster"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: faster-benchmark-setup
created_at: 2023-04-30T09:14:55Z
updated_at: 2023-04-30T14:23:50Z
url: https://github.com/astral-sh/ruff/pull/4156
synced_at: 2026-01-12T15:55:14Z
```

# Use --filter=blob:none to clone CPython faster

---

_@JonathanPlasse_

```console
# Before
scripts/benchmarks/setup.sh  72,98s user 9,59s system 11% cpu 11:31,73 total
# After
scripts/benchmarks/setup.sh  10,70s user 2,77s system 6% cpu 3:33,95 total
```

---

_Comment by @github-actions[bot] on 2023-04-30 09:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.11ms     2.5 MB/sec    1.00     16.6±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.02ms     4.2 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    496.2±4.99µs     5.9 MB/sec    1.00    488.3±4.73µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     4.9 MB/sec    1.01      8.4±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1775.2±7.86µs     9.4 MB/sec    1.01  1786.9±11.74µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.6±2.02µs    14.9 MB/sec    1.01    200.1±1.61µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.02      3.7±0.02ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.5±0.02ms     6.3 MB/sec    1.00      6.5±0.03ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1262.5±4.60µs    13.2 MB/sec    1.00   1262.0±7.03µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    128.7±0.54µs    22.9 MB/sec    1.00    128.8±0.78µs    22.9 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.3 MB/sec    1.00      2.8±0.01ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.6±1.06ms  2027.1 KB/sec    1.01     20.7±0.95ms  2011.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.2±0.34ms     3.2 MB/sec    1.00      5.2±0.26ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   620.7±41.04µs     4.8 MB/sec    1.00   605.2±48.16µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.39ms     3.0 MB/sec    1.00      8.5±0.40ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.31ms     4.0 MB/sec    1.04     10.5±0.53ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.6 MB/sec    1.05      2.3±0.12ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   239.6±10.08µs    12.3 MB/sec    1.12   269.2±14.56µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.29ms     5.4 MB/sec    1.00      4.7±0.23ms     5.4 MB/sec
parser/large/dataset.py                    1.00      8.2±0.31ms     5.0 MB/sec    1.08      8.8±0.46ms     4.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1536.6±55.50µs    10.8 MB/sec    1.08  1659.8±153.40µs    10.0 MB/sec
parser/numpy/globals.py                    1.00    159.6±7.49µs    18.5 MB/sec    1.06    168.5±6.32µs    17.5 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.27ms     7.2 MB/sec    1.01      3.6±0.12ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-04-30 11:39_

thanks, that's a huge speedup!

i have to try this in the ecosystem ci docker too, cloning is currently the bottleneck there

---

_Merged by @konstin on 2023-04-30 11:39_

---

_Closed by @konstin on 2023-04-30 11:39_

---

_Branch deleted on 2023-04-30 13:09_

---

_Comment by @JonathanPlasse on 2023-04-30 13:09_

> thanks, that's a huge speedup!
> 
> i have to try this in the ecosystem ci docker too, cloning is currently the bottleneck there

I can make PR for this too.

---

_Comment by @JonathanPlasse on 2023-04-30 13:29_

The solution used for the ecosystem is much faster than `--filter=blob:none`.

---

_Comment by @konstin on 2023-04-30 13:34_

do you mean the settings below? i'm unfortunately not familiar enough with the internals of git to compare them. https://github.com/charliermarsh/ruff/blob/a32617911a36998d2ad88ee7bafcd7c9e01720f1/scripts/check_ecosystem.py#L43-L54

---

_Comment by @JonathanPlasse on 2023-04-30 14:23_

> do you mean the settings below? i'm unfortunately not familiar enough with the internals of git to compare them.
> 
> https://github.com/charliermarsh/ruff/blob/a32617911a36998d2ad88ee7bafcd7c9e01720f1/scripts/check_ecosystem.py#L43-L54

Yes, that is what I checked.

---
