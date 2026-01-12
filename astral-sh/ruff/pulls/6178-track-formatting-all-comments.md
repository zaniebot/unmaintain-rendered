```yaml
number: 6178
title: Track formatting all comments
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: track_formatting_all_comments
created_at: 2023-07-30T13:44:11Z
updated_at: 2023-08-10T07:37:11Z
url: https://github.com/astral-sh/ruff/pull/6178
synced_at: 2026-01-12T15:55:20Z
```

# Track formatting all comments

---

_@konstin_

We currently don't format all comments as match statements are not yet implemented. We can work around this for the top level match statement by setting them manually formatted but the mocked-out top level match doesn't call into its children so they would still have unformatted comments

---

_Comment by @github-actions[bot] on 2023-07-30 14:19_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.09ms     4.9 MB/sec    1.00      8.3±0.10ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1647.0±38.73µs    10.1 MB/sec    1.00  1633.4±11.28µs    10.2 MB/sec
formatter/numpy/globals.py                 1.02    187.6±7.25µs    15.7 MB/sec    1.00    184.5±2.58µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.09ms     7.3 MB/sec    1.01      3.5±0.12ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.2±0.04ms     4.0 MB/sec    1.00     10.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.02ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.0±0.87µs     7.7 MB/sec    1.00    382.2±0.66µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.07ms     4.8 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.3±0.01ms     7.6 MB/sec    1.00      5.3±0.02ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1151.5±3.99µs    14.5 MB/sec    1.00   1144.3±2.90µs    14.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    132.7±2.34µs    22.2 MB/sec    1.00    132.1±1.60µs    22.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.6 MB/sec    1.00      2.4±0.02ms    10.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.9±0.29ms     3.4 MB/sec    1.01     12.0±0.28ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.07ms     7.3 MB/sec    1.03      2.3±0.13ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   255.3±11.24µs    11.6 MB/sec    1.01   257.0±12.29µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.15ms     5.1 MB/sec    1.01      5.1±0.16ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.28ms     2.7 MB/sec    1.02     15.2±0.44ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.12ms     4.0 MB/sec    1.01      4.1±0.10ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   512.6±14.51µs     5.8 MB/sec    1.01   515.7±17.90µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.18ms     3.3 MB/sec    1.01      7.8±0.16ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.15ms     5.0 MB/sec    1.01      8.2±0.13ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1697.9±37.91µs     9.8 MB/sec    1.01  1712.2±64.73µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   195.9±11.93µs    15.1 MB/sec    1.02   199.6±10.26µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.07ms     7.0 MB/sec    1.00      3.6±0.09ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-07-31 08:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:184 on 2023-07-31 08:50_

You could implement your own `PreorderVisitor` that marks the comments of all children as formatted. We probably need something similar for `fmt: skip` anyway.

---

_@konstin reviewed on 2023-08-01 10:13_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/lib.rs`:184 on 2023-08-01 10:13_

i'll wait until we have match statement formatting instead

---

_Assigned to @MichaReiser by @konstin on 2023-08-08 13:41_

---

_Label `formatter` added by @MichaReiser on 2023-08-08 14:32_

---

_Comment by @MichaReiser on 2023-08-08 15:16_

Current dependencies on/for this PR:
* main
  * **PR #6422** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6422" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #6426** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6426" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #6441** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6441" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #6178** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6178" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6178?utm_source=stack-comment).

---

_@MichaReiser reviewed on 2023-08-08 15:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/match_case.rs`:27 on 2023-08-08 15:23_

This is intentionally ugly to make sure we remove we don't forget to remove the `comment.mark_formatted` when replacing this with `pattern.format()`

---

_Marked ready for review by @MichaReiser on 2023-08-08 15:24_

---

_Comment by @MichaReiser on 2023-08-08 15:24_

Can I now approve my own PR :sweat_smile: 

---

_Comment by @konstin on 2023-08-08 15:30_

¯\\\_(ツ)\_/¯

---

_@MichaReiser approved on 2023-08-08 15:34_

---

_Comment by @konstin on 2023-08-08 20:26_

so who'll merge?

---

_Comment by @MichaReiser on 2023-08-09 06:58_

I can take care of merging since we have to coordinate with the downstream PRs. All I need is a thumbs up/down from you.

---

_Comment by @konstin on 2023-08-09 07:21_

:+1:

---

_Comment by @MichaReiser on 2023-08-10 07:12_

[Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6178) rebased this pull request as part of a merge.

---

_Merged by @MichaReiser on 2023-08-10 07:19_

---

_Closed by @MichaReiser on 2023-08-10 07:19_

---

_Branch deleted on 2023-08-10 07:19_

---

_Comment by @MichaReiser on 2023-08-10 07:19_

@MichaReiser merged this pull request with [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6178).

---
