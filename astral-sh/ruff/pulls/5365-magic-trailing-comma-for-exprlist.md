```yaml
number: 5365
title: magic trailing comma for ExprList
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: magic-comma-for-expr-list
created_at: 2023-06-26T07:11:51Z
updated_at: 2023-07-07T20:47:45Z
url: https://github.com/astral-sh/ruff/pull/5365
synced_at: 2026-01-12T15:55:18Z
```

# magic trailing comma for ExprList

---

_@davidszotten_

seems mostly better, still need to think about

`json = {"k": {"k2": {"k3": [1,]}}}` where we differ from black in trailing commas for all the outer dicts



---

_Comment by @github-actions[bot] on 2023-06-26 07:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07      8.9±0.36ms     4.6 MB/sec    1.00      8.3±0.28ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.06  1976.6±69.07µs     8.4 MB/sec    1.00  1857.4±66.34µs     9.0 MB/sec
formatter/numpy/globals.py                 1.01   236.4±15.85µs    12.5 MB/sec    1.00   233.1±15.82µs    12.7 MB/sec
formatter/pydantic/types.py                1.08      4.4±0.18ms     5.8 MB/sec    1.00      4.1±0.10ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     17.3±0.57ms     2.4 MB/sec    1.02     17.7±0.54ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.18ms     4.0 MB/sec    1.06      4.5±0.17ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   547.8±21.20µs     5.4 MB/sec    1.01   552.5±20.61µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.20ms     3.4 MB/sec    1.03      7.7±0.29ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.37ms     4.5 MB/sec    1.06      9.6±0.24ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1914.5±79.01µs     8.7 MB/sec    1.04  1982.5±48.84µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.03   231.0±13.12µs    12.8 MB/sec    1.00   223.9±12.55µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.15ms     6.4 MB/sec    1.05      4.2±0.12ms     6.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.06ms     5.1 MB/sec    1.08      8.6±0.08ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1802.9±24.48µs     9.2 MB/sec    1.06  1903.7±20.82µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    215.5±4.73µs    13.7 MB/sec    1.04    223.5±4.41µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.05ms     6.3 MB/sec    1.06      4.3±0.06ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.11ms     2.6 MB/sec    1.01     15.6±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.6±6.91µs     5.8 MB/sec    1.01    511.0±7.80µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.06ms     5.0 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1753.3±19.59µs     9.5 MB/sec    1.00  1741.2±14.01µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.5±3.28µs    14.4 MB/sec    1.00    205.4±6.66µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-06-26 07:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__function_trailing_comma_py.snap`:123 on 2023-06-26 07:27_

Interesting that black doesn't add trailing commas to all outer elements, especially considering that the trailing comma in the array expression isn't JSON.

We can tackle this divergence as its own PR if you like. It probably requires:
* Detecting that the dict (and list) only contains JSON like values
* Add a `Preserve` option to the `comma_separated` builder to only preserve existing trailing commas.

---

_@konstin approved on 2023-06-26 10:17_

---

_Label `formatter` added by @konstin on 2023-06-26 10:45_

---

_@MichaReiser approved on 2023-06-26 11:05_

---

_Comment by @davidszotten on 2023-06-26 18:23_

so many merge conflicts today :(

---

_Comment by @davidszotten on 2023-06-26 18:45_

hm i can't reproduce this test failure locally

---

_Comment by @konstin on 2023-06-26 18:53_

Could you try `cargo insta test --all --all-features --delete-unreferenced-snapshots`? That should delete the extra snapshot.

---

_Comment by @davidszotten on 2023-06-26 19:17_

ah, thanks

i was very confused because my original patch didn't touch that file but i realise now that my patch +the most recent rebase made ruff match black exactly for this file which removes the snap file

---

_Marked ready for review by @davidszotten on 2023-06-26 19:34_

---

_Comment by @MichaReiser on 2023-06-26 19:58_

> so many merge conflicts today :(

I'm sorry 😞 

---

_Merged by @MichaReiser on 2023-06-26 19:59_

---

_Closed by @MichaReiser on 2023-06-26 19:59_

---

_Comment by @davidszotten on 2023-06-26 20:05_

> > so many merge conflicts today :(
> 
> I'm sorry 😞

no worries, all good

---

_Branch deleted on 2023-07-07 20:47_

---
