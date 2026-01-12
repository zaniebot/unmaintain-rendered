```yaml
number: 4274
title: Update confusable character mapping
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: update-ambiguous-list
created_at: 2023-05-08T09:57:48Z
updated_at: 2023-05-08T18:56:10Z
url: https://github.com/astral-sh/ruff/pull/4274
synced_at: 2026-01-12T03:56:39Z
```

# Update confusable character mapping

---

_Pull request opened by @akx on 2023-05-08 09:57_

... with a script to do it automatically!

Fixes #4268
Refs https://github.com/microsoft/vscode/pull/175918

---

_Marked ready for review by @akx on 2023-05-08 12:03_

---

_Comment by @github-actions[bot] on 2023-05-08 12:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.35ms     2.5 MB/sec    1.01     16.7±0.26ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.13ms     4.1 MB/sec    1.00      4.0±0.10ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   512.3±29.25µs     5.8 MB/sec    1.00   509.6±15.20µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.36ms     3.7 MB/sec    1.00      7.0±0.17ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.12ms     5.1 MB/sec    1.01      8.1±0.18ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1746.5±64.63µs     9.5 MB/sec    1.00  1724.2±53.02µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   210.1±10.25µs    14.0 MB/sec    1.00    206.4±7.65µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.08ms     7.0 MB/sec    1.01      3.7±0.13ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.4±0.13ms     6.4 MB/sec    1.33      8.5±0.15ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1272.1±30.94µs    13.1 MB/sec    1.25  1588.5±53.56µs    10.5 MB/sec
parser/numpy/globals.py                    1.00    126.3±4.90µs    23.4 MB/sec    1.20    151.9±6.16µs    19.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.10ms     9.1 MB/sec    1.25      3.5±0.10ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.6±0.37ms     2.1 MB/sec    1.00     19.6±0.31ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.14ms     3.4 MB/sec    1.00      5.0±0.15ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   586.0±18.54µs     5.0 MB/sec    1.00   585.0±14.11µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.20ms     3.1 MB/sec    1.00      8.3±0.16ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01     10.0±0.19ms     4.1 MB/sec    1.00      9.9±0.14ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     7.9 MB/sec    1.00      2.1±0.06ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.01   239.2±15.32µs    12.3 MB/sec    1.00   237.6±12.31µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.09ms     5.8 MB/sec    1.01      4.4±0.11ms     5.7 MB/sec
parser/large/dataset.py                    1.00      8.1±0.13ms     5.0 MB/sec    1.01      8.1±0.12ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1551.2±39.76µs    10.7 MB/sec    1.00  1550.2±39.79µs    10.7 MB/sec
parser/numpy/globals.py                    1.00    159.6±8.82µs    18.5 MB/sec    1.00    160.3±6.02µs    18.4 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.08ms     7.4 MB/sec    1.00      3.4±0.07ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `scripts/update_ambiguous_characters.py`:20 on 2023-05-08 12:41_

I'm not very familiar with Python but would `urllib` be a viable (dependency free) option to get cross-platform support?

---

_Review comment by @MichaReiser on `scripts/update_ambiguous_characters.py`:42 on 2023-05-08 12:44_

Can we use `'chr(items[i])'` instead? It would make the unicode table slightly more readable. But don't bother if it is giving you problems. 

---

_@MichaReiser approved on 2023-05-08 12:45_

---

_@akx reviewed on 2023-05-08 16:35_

---

_Review comment by @akx on `scripts/update_ambiguous_characters.py`:20 on 2023-05-08 16:35_

I figured this is more bullet-proof and reasonably cross-platform, since cURL is easily available for all platforms. This script is only expected to be run (by a maintainer) when the upstream updates anyway, and I would assume said maintainer has cURL :)

---

_Review comment by @akx on `scripts/update_ambiguous_characters.py`:42 on 2023-05-08 16:43_

NAK on that - the characters' textual representations themselves can be unprintable; with that patched in, the start of the list is

```rust
    FxHashMap::from_iter([
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
        (' ', ' '),
```

😁 

---

_@akx reviewed on 2023-05-08 16:43_

---

_Merged by @charliermarsh on 2023-05-08 18:20_

---

_Closed by @charliermarsh on 2023-05-08 18:20_

---

_Branch deleted on 2023-05-08 18:56_

---
