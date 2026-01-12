```yaml
number: 6826
title: Fix formatting of comments between function and arguments
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/call
created_at: 2023-08-23T21:28:26Z
updated_at: 2023-08-25T04:32:50Z
url: https://github.com/astral-sh/ruff/pull/6826
synced_at: 2026-01-12T02:45:38Z
```

# Fix formatting of comments between function and arguments

---

_Pull request opened by @charliermarsh on 2023-08-23 21:28_

## Summary

We now format comments between a function and its arguments as dangling. Like with other strange placements, I've biased towards preserving the existing formatting, rather than attempting to reorder the comments.

Closes https://github.com/astral-sh/ruff/issues/6818.

## Test Plan

`cargo test`

Before:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76050          |
| django       | 0.99820          |
| transformers | 0.99800          |
| twine        | 0.99876          |
| typeshed     | 0.99953          |
| warehouse    | 0.99615          |
| zulip        | 0.99729          |

After:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76050          |
| django       | 0.99820          |
| transformers | 0.99800          |
| twine        | 0.99876          |
| typeshed     | 0.99953          |
| warehouse    | 0.99615          |
| zulip        | 0.99729          |


---

_Label `formatter` added by @charliermarsh on 2023-08-23 21:28_

---

_@charliermarsh reviewed on 2023-08-23 21:29_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:218 on 2023-08-23 21:29_

I feel like this is the behavior we want... Otherwise, if you _only_ have own-line dangling comments, you end up with formatting like:
```python
(
    thing#dangling comment doesn't get spaced properly
    ()
)
```

This doesn't just apply to dangling comments in this position -- I've run into it elsewhere too. But if this is _intentionally_ non-breaking, I can also handle it specifically in arguments like I've done elsewhere.
 

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-23 21:29_

---

_Review requested from @konstin by @charliermarsh on 2023-08-23 21:29_

---

_@charliermarsh reviewed on 2023-08-23 21:31_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:218 on 2023-08-23 21:31_

Notice, e.g., that this allowed me to get rid of all the special handling here: https://github.com/astral-sh/ruff/pull/6826/files#diff-84d598d9ec08859709064bf326a40e0b7e28f864ab05659c022be22ef13b25deL113

---

_Comment by @github-actions[bot] on 2023-08-23 22:04_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      4.1±0.03ms    10.0 MB/sec    1.00      4.0±0.02ms    10.1 MB/sec
formatter/numpy/ctypeslib.py               1.01    847.4±3.32µs    19.7 MB/sec    1.00    840.6±2.06µs    19.8 MB/sec
formatter/numpy/globals.py                 1.00     79.4±1.04µs    37.2 MB/sec    1.00     79.6±0.96µs    37.1 MB/sec
formatter/pydantic/types.py                1.00   1652.0±2.63µs    15.4 MB/sec    1.00   1653.3±9.12µs    15.4 MB/sec
linter/all-rules/large/dataset.py          1.00     10.2±0.07ms     4.0 MB/sec    1.01     10.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    385.0±0.71µs     7.7 MB/sec    1.01    389.1±0.72µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.06ms     4.8 MB/sec    1.01      5.3±0.03ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.01      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1201.2±18.84µs    13.9 MB/sec    1.01   1213.0±4.94µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.3±0.28µs    21.0 MB/sec    1.01    141.4±0.33µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.4 MB/sec    1.01      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07      6.3±0.27ms     6.5 MB/sec    1.00      5.9±0.18ms     6.9 MB/sec
formatter/numpy/ctypeslib.py               1.06  1266.9±57.39µs    13.1 MB/sec    1.00  1200.7±39.87µs    13.9 MB/sec
formatter/numpy/globals.py                 1.06   115.8±10.38µs    25.5 MB/sec    1.00    109.0±9.53µs    27.1 MB/sec
formatter/pydantic/types.py                1.06      2.5±0.10ms    10.2 MB/sec    1.00      2.4±0.14ms    10.8 MB/sec
linter/all-rules/large/dataset.py          1.14     18.9±0.96ms     2.2 MB/sec    1.00     16.6±0.55ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      4.9±0.28ms     3.4 MB/sec    1.00      4.6±0.17ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   596.0±26.06µs     5.0 MB/sec    1.03   611.4±32.61µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.11      9.5±0.43ms     2.7 MB/sec    1.00      8.6±0.20ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.08     10.1±0.36ms     4.0 MB/sec    1.00      9.3±0.41ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.1±0.08ms     7.9 MB/sec    1.00  1979.1±76.07µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.07   257.0±16.56µs    11.5 MB/sec    1.00    239.3±9.13µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.09      4.5±0.27ms     5.6 MB/sec    1.00      4.2±0.24ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:209 on 2023-08-24 06:46_

Is the `first` check still necessary?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:212 on 2023-08-24 06:47_

I believe the reason for not using a `hard_line_break` mainly was that it may result in syntax errors if the enclosing node isn't parenthesized. But we can see how it goes

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:993 on 2023-08-24 06:49_

Nit:

I find this nesting hard to read. 
```suggestion
		if let (Some(preceding), Some(following)) = (comment.preceding_node(), comment.following_node()) {
			if TextRange::new(preceding.end(), following.start()).contains(comment.range()) {
```

---

_@MichaReiser approved on 2023-08-24 06:50_

---

_@konstin approved on 2023-08-24 10:48_

i have a preference towards using those (they obscure we're dealing with a function call), but i'm fine with keeping as is.

---

_Merged by @charliermarsh on 2023-08-25 04:06_

---

_Closed by @charliermarsh on 2023-08-25 04:06_

---

_Branch deleted on 2023-08-25 04:06_

---
