```yaml
number: 5997
title: Add a unit test for python-file-like directory exclusion
type: pull_request
state: merged
author: harupy
labels:
  - internal
assignees: []
merged: true
base: main
head: py-dir-unittest
created_at: 2023-07-23T01:01:01Z
updated_at: 2023-07-24T02:28:33Z
url: https://github.com/astral-sh/ruff/pull/5997
synced_at: 2026-01-12T03:30:22Z
```

# Add a unit test for python-file-like directory exclusion

---

_Pull request opened by @harupy on 2023-07-23 01:01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Related to #5850 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-07-23 01:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.02ms     4.1 MB/sec    1.00      9.9±0.03ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1906.3±1.99µs     8.7 MB/sec    1.00   1909.2±4.36µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    207.1±1.22µs    14.2 MB/sec    1.00    206.7±0.38µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.00ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.02ms     3.0 MB/sec    1.00     13.6±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.8±0.94µs     8.1 MB/sec    1.00    365.4±0.85µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.01ms     5.7 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1426.1±3.16µs    11.7 MB/sec    1.00   1419.3±3.41µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.8±0.28µs    19.7 MB/sec    1.00    149.5±1.05µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.00ms     8.1 MB/sec    1.00      3.1±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.9±0.73ms     2.7 MB/sec    1.17     17.4±0.64ms     2.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.9±0.13ms     5.8 MB/sec    1.18      3.4±0.21ms     5.0 MB/sec
formatter/numpy/globals.py                 1.00   328.3±14.84µs     9.0 MB/sec    1.15   377.9±38.60µs     7.8 MB/sec
formatter/pydantic/types.py                1.00      6.3±0.30ms     4.0 MB/sec    1.21      7.6±0.42ms     3.3 MB/sec
linter/all-rules/large/dataset.py          1.01     20.7±0.74ms  2012.0 KB/sec    1.00     20.5±0.71ms  2035.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      5.7±0.30ms     2.9 MB/sec    1.00      5.4±0.18ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   668.9±33.54µs     4.4 MB/sec    1.00   648.4±41.41µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.9±0.48ms     2.6 MB/sec    1.00      9.8±0.53ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.03     11.6±0.79ms     3.5 MB/sec    1.00     11.3±0.36ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.08ms     7.2 MB/sec    1.00      2.3±0.09ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.04   282.3±19.28µs    10.5 MB/sec    1.00   270.8±19.49µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.24ms     5.2 MB/sec    1.00      4.9±0.25ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/resolver.rs`:625 on 2023-07-23 10:43_

nit: `use std::fs::File` and then `File::create(&file1).unwrap();`

---

_Review comment by @konstin on `crates/ruff/src/resolver.rs`:644 on 2023-07-23 10:45_

```suggestion
        let mut paths = paths
            .iter()
            .flatten()
            .map(|entry| entry.path())
            .collect::<Vec<_>>();
        paths.sort();
        assert_eq!(paths, &[file2, file1]);
```

---

_Review comment by @konstin on `crates/ruff/src/resolver.rs`:624 on 2023-07-23 10:46_

good comment

---

_@konstin approved on 2023-07-23 10:46_

---

_Review requested from @konstin by @harupy on 2023-07-24 01:08_

---

_Label `internal` added by @charliermarsh on 2023-07-24 01:44_

---

_Comment by @charliermarsh on 2023-07-24 01:44_

This is great! Thanks!

---

_Merged by @charliermarsh on 2023-07-24 01:50_

---

_Closed by @charliermarsh on 2023-07-24 01:50_

---
