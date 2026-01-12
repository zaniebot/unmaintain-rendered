```yaml
number: 5350
title: format StmtWith
type: pull_request
state: merged
author: davidszotten
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: format-stmt-with
created_at: 2023-06-24T07:27:21Z
updated_at: 2023-07-07T20:48:09Z
url: https://github.com/astral-sh/ruff/pull/5350
synced_at: 2026-01-12T03:36:55Z
```

# format StmtWith

---

_Pull request opened by @davidszotten on 2023-06-24 07:27_

## Summary

format `StmtWith` (and `WithItem`)

one outstanding todo (see the bottom of `with.py`)

## Test Plan

snapshots


---

_Marked ready for review by @davidszotten on 2023-06-24 07:35_

---

_Comment by @github-actions[bot] on 2023-06-24 07:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.6±0.01ms     6.1 MB/sec    1.02      6.8±0.02ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1553.9±48.63µs    10.7 MB/sec    1.00   1554.2±5.23µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    184.2±1.03µs    16.0 MB/sec    1.03    189.0±0.44µs    15.6 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.01ms     7.3 MB/sec    1.00      3.5±0.02ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.11ms     3.1 MB/sec    1.00     13.1±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.4±0.40µs     6.9 MB/sec    1.00    426.9±0.88µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.01      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1443.4±2.42µs    11.5 MB/sec    1.01   1455.1±3.12µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.2±0.20µs    18.2 MB/sec    1.00    162.6±0.64µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.54ms     3.7 MB/sec    1.05     11.5±0.65ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.14ms     6.8 MB/sec    1.02      2.5±0.15ms     6.6 MB/sec
formatter/numpy/globals.py                 1.00   298.8±27.70µs     9.9 MB/sec    1.06   317.5±40.02µs     9.3 MB/sec
formatter/pydantic/types.py                1.00      5.4±0.29ms     4.7 MB/sec    1.05      5.7±0.39ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     19.7±0.75ms     2.1 MB/sec    1.09     21.4±0.78ms  1943.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.24ms     3.0 MB/sec    1.03      5.7±0.33ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   669.7±34.54µs     4.4 MB/sec    1.00   667.8±30.88µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.5±0.32ms     2.7 MB/sec    1.00      9.4±0.44ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.8±0.35ms     3.8 MB/sec    1.02     11.0±0.39ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.08ms     7.2 MB/sec    1.03      2.4±0.16ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.02   280.9±17.45µs    10.5 MB/sec    1.00   276.3±13.74µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.15ms     5.2 MB/sec    1.07      5.2±0.39ms     4.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/with.py`:43 on 2023-06-24 12:07_

This sounds reasonable to me. We should make it wrap the same as for function definition

```python
# All flat
def test(a, b, c): 
	pass

# parentheses expanded
def test(
	a, b, c
): 
	pass

# all expanded
def test(
	a, 
	b, 
	c
): 
	pass
```

You can achieve this by using two nested groups:
* outer group: For the parentheses and indent
* inner group: Around the with items

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/with_item.rs`:21 on 2023-06-24 12:08_

Not sure where we should plate the `as ... name`. Should we place the `as` on a new line (as we do for binary operators) or keep it on the same line?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__with_py.snap`:25 on 2023-06-24 12:10_

Is this `fmt: skip` intentional?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:29 on 2023-06-24 12:12_

Do we need the `partition_point` or can we format all `dangling_comments` at once? Because it seems there's only a single place where dangling comments can appear (and thus, splitting is unnecessary). 


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:34 on 2023-06-24 12:14_

Nit: You can use `optional_parentheses(&joined_items)` if this lands after #5328

---

_@MichaReiser approved on 2023-06-24 12:15_

This looks great. Thank you! I've one suggestion on how to format the with items that span over multiple lines. But this is otherwise good to go.

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/with.py`:43 on 2023-06-24 18:32_

oh wait. when there is an `as`, wrapping isn't allowed in python < 3.9 (3.8 isn't eol until oct '24)



---

_@davidszotten reviewed on 2023-06-24 18:32_

---

_@davidszotten reviewed on 2023-06-24 18:34_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__with_py.snap`:25 on 2023-06-24 18:34_

oops, this whole block is leftover from debugging. will remove

---

_@MichaReiser reviewed on 2023-06-24 20:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/with.py`:43 on 2023-06-24 20:04_

We can change that when adding support for targeting different python versions. It's something that we'll need to add to the formatter options. 

---

_@davidszotten reviewed on 2023-06-25 12:18_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/with_item.rs`:21 on 2023-06-25 12:18_

black seems to keep on the same line i think

---

_@MichaReiser reviewed on 2023-06-26 14:08_

Thank you!

---

_Merged by @MichaReiser on 2023-06-26 14:09_

---

_Closed by @MichaReiser on 2023-06-26 14:09_

---

_Label `internal` added by @MichaReiser on 2023-06-26 14:09_

---

_Label `formatter` added by @MichaReiser on 2023-06-26 14:09_

---

_Branch deleted on 2023-07-07 20:48_

---
