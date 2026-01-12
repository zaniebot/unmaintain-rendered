```yaml
number: 5397
title: Update default configuration.md to mention C901 rule
type: pull_request
state: merged
author: ethunk
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-06-27T16:37:06Z
updated_at: 2023-06-28T21:43:04Z
url: https://github.com/astral-sh/ruff/pull/5397
synced_at: 2026-01-12T03:36:55Z
```

# Update default configuration.md to mention C901 rule

---

_Pull request opened by @ethunk on 2023-06-27 16:37_

## Summary

The default configuration provides a configuration for mccabe complexity, but since rule "C90" is not enabled by default, the configuration has no effect. In the first pass implementation, I used the default configuration and assumed that the mccabe complexity rule would be checked for and left confused on why ruff was not correctly checking. 

The solution was to enable the rule. This change updates the docs to show that the C90 rule enabled by default and provides the correct configuration where the optional settings for [tool.ruff.mccabe] makes sense.

## Test Plan

No need to test as this is a markdown file.


### Notes
I love the tool! I'm not a copy writer, so i'm open to feedback on the wording or other feedback.


---

_Comment by @github-actions[bot] on 2023-06-27 16:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.04ms     4.3 MB/sec    1.00      9.4±0.02ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.01ms     8.0 MB/sec    1.00      2.0±0.01ms     8.2 MB/sec
formatter/numpy/globals.py                 1.03    234.9±4.23µs    12.6 MB/sec    1.00    228.5±0.67µs    12.9 MB/sec
formatter/pydantic/types.py                1.02      4.5±0.05ms     5.7 MB/sec    1.00      4.4±0.02ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     16.0±0.08ms     2.5 MB/sec    1.00     15.9±0.06ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    514.9±1.19µs     5.7 MB/sec    1.00    511.7±1.12µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.02ms     3.6 MB/sec    1.00      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.02ms     5.1 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1769.8±3.14µs     9.4 MB/sec    1.00   1754.0±4.34µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.8±0.50µs    14.8 MB/sec    1.00    199.0±0.38µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     7.0 MB/sec    1.00      3.7±0.02ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.18ms     4.3 MB/sec    1.01      9.7±0.22ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.0±0.02ms     8.3 MB/sec    1.00  1974.3±18.95µs     8.4 MB/sec
formatter/numpy/globals.py                 1.01   215.9±10.35µs    13.7 MB/sec    1.00    212.8±5.38µs    13.9 MB/sec
formatter/pydantic/types.py                1.02      4.6±0.12ms     5.5 MB/sec    1.00      4.5±0.07ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.32ms     2.6 MB/sec    1.02     16.2±0.27ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.2±6.35µs     6.7 MB/sec    1.02    446.4±9.41µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.21ms     3.5 MB/sec    1.01      7.3±0.19ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.12ms     5.0 MB/sec    1.02      8.4±0.21ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1696.7±25.22µs     9.8 MB/sec    1.03  1752.7±37.47µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.8±3.62µs    16.3 MB/sec    1.01    182.8±3.87µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.01      3.8±0.06ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-28 20:12_

Thank you! I think we need to treat this a little differently. The default configuration is meant to represent the configuration that Ruff uses by default, and by default Ruff doesn't actually enabled the C90 rules. I'll make this a bit clearer and merge...

---

_Label `documentation` added by @charliermarsh on 2023-06-28 21:13_

---

_Renamed from "Update default configuration.md to include C90 rule as enabled." to "Update default configuration.md to mention C901 rule" by @charliermarsh on 2023-06-28 21:13_

---

_Merged by @charliermarsh on 2023-06-28 21:22_

---

_Closed by @charliermarsh on 2023-06-28 21:22_

---
