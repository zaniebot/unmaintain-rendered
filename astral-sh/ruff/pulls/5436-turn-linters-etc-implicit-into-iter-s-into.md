```yaml
number: 5436
title: "Turn Linters', etc. implicit `into_iter()`s into explicit `rules()`"
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: yeet-linter-into-iter
created_at: 2023-06-29T07:34:51Z
updated_at: 2023-07-04T19:40:55Z
url: https://github.com/astral-sh/ruff/pull/5436
synced_at: 2026-01-12T15:55:18Z
```

# Turn Linters', etc. implicit `into_iter()`s into explicit `rules()`

---

_@akx_

## Summary

As discussed on ~IRC~ Discord, this will make it easier for e.g. the docs generation stuff to get all rules for a linter (using `all_rules()`) instead of just non-nursery ones, and it also makes it more Explicit Is Better Than Implicit to iterate over linter rules.

Grepping for `Item = Rule` reveals some remaining implicit `IntoIterator`s that I didn't feel were necessarily in scope for this (and honestly, iterating over a `RuleSet` makes sense).

---

_Marked ready for review by @akx on 2023-06-29 08:08_

---

_Comment by @github-actions[bot] on 2023-06-29 08:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.04ms     4.4 MB/sec    1.00      9.3±0.04ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.01ms     8.2 MB/sec    1.00      2.0±0.01ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    225.1±1.07µs    13.1 MB/sec    1.00    225.3±1.11µs    13.1 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.02ms     5.7 MB/sec    1.00      4.5±0.02ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     15.9±0.09ms     2.6 MB/sec    1.00     15.7±0.08ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.03ms     4.2 MB/sec    1.00      3.9±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    510.9±1.56µs     5.8 MB/sec    1.00    507.6±2.01µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.03ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.03ms     5.1 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1752.3±6.37µs     9.5 MB/sec    1.00   1747.1±6.30µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.5±0.98µs    14.9 MB/sec    1.01    198.8±1.51µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.7±0.36ms     4.2 MB/sec    1.00      9.6±0.31ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.10ms     8.1 MB/sec    1.00      2.1±0.08ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   224.5±17.11µs    13.1 MB/sec    1.01   227.1±11.27µs    13.0 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.16ms     5.7 MB/sec    1.00      4.4±0.11ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.56ms     2.7 MB/sec    1.06     16.0±0.61ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.08ms     4.2 MB/sec    1.06      4.1±0.13ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   473.6±14.69µs     6.2 MB/sec    1.10   521.3±20.34µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.20ms     3.9 MB/sec    1.08      7.1±0.26ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.37ms     5.1 MB/sec    1.03      8.2±0.22ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1750.9±61.48µs     9.5 MB/sec    1.00  1730.4±42.40µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01   208.3±10.43µs    14.2 MB/sec    1.00   207.3±11.96µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.11ms     6.8 MB/sec    1.02      3.8±0.13ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:330 on 2023-06-29 08:23_

Nit: Not sure if a good idea but using `vec` creates an allocation for every `rules` call. Should we instead return a static array? I'm not sure if this is a good idea because it may force Rust to copy the whole array which is rather large. 

Another alternative could be to have a static (private) array of all rules somewhere and have `rules()` return a `&'static [Rule]` slice. 

```
let rule_len = rule_paths.len();
```

```suggestion
            Linter::#linter => [#(#rule_paths,)*; #rule_len].into_iter(),
```

I'm also okay having this as a separate PR because the code already used a `vec` before.

---

_@MichaReiser reviewed on 2023-06-29 08:23_

---

_@konstin approved on 2023-06-29 08:24_

---

_Review requested from @charliermarsh by @konstin on 2023-06-29 08:24_

---

_Comment by @akx on 2023-06-29 11:08_

I'll draft this and make the array change suggested by @MichaReiser across the board 👍 

**EDIT:** Okay, maybe we can do that later on, I'm getting a mysterious macro expansion syntax error I don't have the bandwidth for right now 😂 

---

_Converted to draft by @akx on 2023-06-29 11:08_

---

_Marked ready for review by @akx on 2023-06-29 11:42_

---

_Merged by @charliermarsh on 2023-07-03 23:35_

---

_Closed by @charliermarsh on 2023-07-03 23:35_

---

_Branch deleted on 2023-07-04 19:40_

---
