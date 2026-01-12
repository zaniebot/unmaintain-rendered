```yaml
number: 6187
title: Print log when formatter ecosystem checks fail
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: formatter_ecosystem_ci_debugging
created_at: 2023-07-31T09:23:50Z
updated_at: 2023-07-31T17:20:20Z
url: https://github.com/astral-sh/ruff/pull/6187
synced_at: 2026-01-12T15:55:20Z
```

# Print log when formatter ecosystem checks fail

---

_@konstin_

**Summary** Print the errors when the formatter ecosystem checks failed. Im not happy that we current collect the log in the first place, but this is the less invasive change and we need it to unblock reviewing #6152.

**Test Plan** https://github.com/astral-sh/ruff/actions/runs/5713112075/job/15477879403?pr=6188

---

_Marked ready for review by @konstin on 2023-07-31 09:23_

---

_Label `internal` added by @konstin on 2023-07-31 09:24_

---

_Label `formatter` added by @konstin on 2023-07-31 09:24_

---

_Comment by @github-actions[bot] on 2023-07-31 09:57_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.27ms     4.0 MB/sec    1.08     11.0±0.45ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.10ms     8.3 MB/sec    1.05      2.1±0.09ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    224.9±7.63µs    13.1 MB/sec    1.06   237.5±13.93µs    12.4 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.23ms     5.7 MB/sec    1.05      4.7±0.20ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.03     14.4±0.73ms     2.8 MB/sec    1.00     14.0±0.55ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.14ms     4.8 MB/sec    1.01      3.5±0.15ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   477.9±19.07µs     6.2 MB/sec    1.06   506.3±79.82µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.18ms     4.2 MB/sec    1.04      6.3±0.22ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.03      7.7±0.29ms     5.3 MB/sec    1.00      7.4±0.20ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1562.9±68.20µs    10.7 MB/sec    1.00  1558.5±56.06µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.03    177.9±6.06µs    16.6 MB/sec    1.00    173.2±6.41µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.09ms     7.6 MB/sec    1.00      3.4±0.16ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.5±0.46ms     3.2 MB/sec    1.01     12.7±0.46ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.4±0.08ms     6.8 MB/sec    1.00      2.4±0.10ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   267.3±20.64µs    11.0 MB/sec    1.03   275.1±19.53µs    10.7 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.23ms     4.8 MB/sec    1.00      5.3±0.19ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.03     17.2±0.46ms     2.4 MB/sec    1.00     16.7±0.44ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.22ms     3.7 MB/sec    1.00      4.4±0.19ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.04   561.3±40.88µs     5.3 MB/sec    1.00   540.0±24.42µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.7±0.23ms     3.3 MB/sec    1.00      7.6±0.24ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.04      9.4±0.32ms     4.3 MB/sec    1.00      9.1±0.33ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1895.6±61.52µs     8.8 MB/sec    1.00  1880.3±64.39µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   212.1±14.42µs    13.9 MB/sec    1.00    211.8±8.89µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.1±0.38ms     6.1 MB/sec    1.00      4.0±0.21ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-31 10:38_

Extending the summary with some details would be helpful to ease reviewing. 

It took me a bit of time to understand what I'm reviewing

* How did you test the change?
* we need it to unblock my reviews. What is it and what reviews?

---

_Comment by @konstin on 2023-07-31 11:17_

I expected this to fail because it fails for me locally and now i'm confused. See https://github.com/astral-sh/ruff/pull/6188 for a proper failure

---

_Comment by @konstin on 2023-07-31 12:41_

Current dependencies on/for this PR:
* main
  * **PR #6187** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6187" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #6193** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6193" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #6194** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6194" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6187?utm_source=stack-comment).

---

_Comment by @konstin on 2023-07-31 12:42_

This failed for me locally because i had `target/progress_projects` symlinked and apparently `ignore::WalkBuilder` resolves symlinks in the path, so it only reads `ruff/.gitignore` if the directory isn't symlinked. 

Debugging this spawned #6193. 

---

_Comment by @konstin on 2023-07-31 12:45_

Updated the summary

---

_Merged by @konstin on 2023-07-31 12:45_

---

_Closed by @konstin on 2023-07-31 12:45_

---

_Branch deleted on 2023-07-31 12:45_

---

_Comment by @davidszotten on 2023-07-31 14:59_

this is cool. do you think we could get it to print the line containing the syntax error (ideally source + ruff output)? atm i still need to re-run locally to find the actual issue (unless i'm missing a trick here?)

---

_Comment by @konstin on 2023-07-31 17:20_

Unfortunately this is hard with our parser (it's not really meant for error recovery). I normally take the filename and let `ruff_shrinking` (documented in the `ruff_python_formatter` readme) give me a minimized example and then work on that

---
