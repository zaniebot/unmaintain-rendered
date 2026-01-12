```yaml
number: 6801
title: Handle bracketed comments on sequence patterns
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/bracketed
created_at: 2023-08-23T03:11:22Z
updated_at: 2023-08-25T04:30:07Z
url: https://github.com/astral-sh/ruff/pull/6801
synced_at: 2026-01-12T02:45:38Z
```

# Handle bracketed comments on sequence patterns

---

_Pull request opened by @charliermarsh on 2023-08-23 03:11_

## Summary

This PR ensures that we handle bracketed comments on sequences, like `# comment` here:

```python
match x:
    case [ # comment
        1, 2
    ]:
        pass
```

The handling is very similar to other, similar nodes, except that we do need some special logic to determine whether the sequence is parenthesized, similar to our logic for tuples.

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-08-23 03:11_

---

_Comment by @github-actions[bot] on 2023-08-23 04:03_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.1±0.02ms    10.0 MB/sec    1.00      4.1±0.02ms    10.0 MB/sec
formatter/numpy/ctypeslib.py               1.00    848.9±3.25µs    19.6 MB/sec    1.00    850.4±2.93µs    19.6 MB/sec
formatter/numpy/globals.py                 1.00     78.9±0.42µs    37.4 MB/sec    1.04     81.7±0.37µs    36.1 MB/sec
formatter/pydantic/types.py                1.00   1651.5±2.53µs    15.4 MB/sec    1.00   1653.5±8.27µs    15.4 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.05ms     4.0 MB/sec    1.00     10.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.01      2.8±0.03ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    386.1±0.84µs     7.6 MB/sec    1.00    387.8±1.11µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.05ms     4.8 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.02ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1205.3±4.44µs    13.8 MB/sec    1.00   1206.0±9.55µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.2±1.19µs    21.0 MB/sec    1.00    140.3±0.61µs    21.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.03ms    10.3 MB/sec    1.00      2.5±0.04ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.2±0.38ms     7.8 MB/sec    1.00      5.2±0.11ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   999.5±64.21µs    16.7 MB/sec    1.07  1065.5±50.11µs    15.6 MB/sec
formatter/numpy/globals.py                 1.00     88.8±3.18µs    33.2 MB/sec    1.09     97.0±8.32µs    30.4 MB/sec
formatter/pydantic/types.py                1.00  1938.4±92.54µs    13.2 MB/sec    1.09      2.1±0.09ms    12.0 MB/sec
linter/all-rules/large/dataset.py          1.24     15.4±1.08ms     2.6 MB/sec    1.00     12.4±0.38ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.25      4.4±0.31ms     3.8 MB/sec    1.00      3.5±0.12ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.19   522.9±24.13µs     5.6 MB/sec    1.00   441.0±20.86µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.22      8.2±0.60ms     3.1 MB/sec    1.00      6.7±0.25ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.18      8.5±0.47ms     4.8 MB/sec    1.00      7.2±0.51ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.36      2.1±0.18ms     8.0 MB/sec    1.00  1517.5±64.48µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.08   224.8±11.07µs    13.1 MB/sec    1.00   207.5±14.47µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.20      4.0±0.19ms     6.5 MB/sec    1.00      3.3±0.31ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:82 on 2023-08-23 05:01_

Can you give an example of what this means? From what I understand, there are no patterns but it does start with an opening parentheses i.e., is this an empty tuple `()`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:109 on 2023-08-23 05:08_

I'm not able to follow this. Can you add an example/test cases for the 2 cases here?

---

_@dhruvmanila reviewed on 2023-08-23 05:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:79 on 2023-08-23 06:54_

This seems to be the same as `is_tuple_parenthesized`. It may make sense to make the code reusable (as it is somewhat tricky). It would also be helpful to add an example of why the complex `open > close` paren path is necessary. I already forgot and had to look it up on the PR https://github.com/astral-sh/ruff/commit/95f78821adff8ee8b8d35158309c070bc2a00dfe

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:93 on 2023-08-23 06:57_

The terminology we used so far is:

* parentheses: `(`, `)` or all kind of parentheses (e.g. `has_own_parentheses`)
* brackets: `[`, `]`
* braces: `{`, `}`

We can avoid the terminology altogether by changing it to `is_parenthesized`. 

```suggestion
    pub(crate) fn is_parenthesized(self) -> bool {
```



---

_@MichaReiser approved on 2023-08-23 06:57_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:29 on 2023-08-23 09:24_

How would an empty `TupleNoParens` look like?

---

_@konstin approved on 2023-08-23 09:24_

---

_@charliermarsh reviewed on 2023-08-23 13:32_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:79 on 2023-08-23 13:32_

Haha ok. I can try. It felt difficult to DRY it up without making the types much more permissive.

---

_@MichaReiser reviewed on 2023-08-23 15:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:79 on 2023-08-23 15:28_

Yeah, don't try too hard. 

---

_Merged by @charliermarsh on 2023-08-25 04:03_

---

_Closed by @charliermarsh on 2023-08-25 04:03_

---

_Branch deleted on 2023-08-25 04:03_

---
