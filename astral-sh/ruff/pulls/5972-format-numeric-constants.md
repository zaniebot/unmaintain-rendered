```yaml
number: 5972
title: Format numeric constants
type: pull_request
state: merged
author: lkh42t
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-number
created_at: 2023-07-22T09:44:27Z
updated_at: 2023-07-29T05:26:46Z
url: https://github.com/astral-sh/ruff/pull/5972
synced_at: 2026-01-12T02:58:30Z
```

# Format numeric constants

---

_Pull request opened by @lkh42t on 2023-07-22 09:44_

## Summary

Format int, float and complex constants.

## Test Plan

Existing snapshots.

---

_Comment by @github-actions[bot] on 2023-07-22 10:12_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      9.7±0.14ms     4.2 MB/sec    1.00      9.3±0.10ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.04  1927.2±30.10µs     8.6 MB/sec    1.00   1854.3±4.19µs     9.0 MB/sec
formatter/numpy/globals.py                 1.05    213.5±0.56µs    13.8 MB/sec    1.00    203.0±0.59µs    14.5 MB/sec
formatter/pydantic/types.py                1.03      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.02ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.23ms     3.1 MB/sec    1.00     13.2±0.18ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.04ms     5.0 MB/sec    1.00      3.3±0.04ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.9±1.10µs     6.8 MB/sec    1.00    429.2±0.55µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.10ms     4.3 MB/sec    1.00      5.9±0.07ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.05ms     6.2 MB/sec    1.00      6.6±0.06ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1411.3±2.00µs    11.8 MB/sec    1.00   1399.0±3.99µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    157.9±1.19µs    18.7 MB/sec    1.00    155.0±0.87µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.07ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07     14.4±0.83ms     2.8 MB/sec    1.00     13.4±0.67ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.8±0.20ms     5.9 MB/sec    1.00      2.7±0.21ms     6.1 MB/sec
formatter/numpy/globals.py                 1.07   316.7±29.17µs     9.3 MB/sec    1.00   295.9±26.09µs    10.0 MB/sec
formatter/pydantic/types.py                1.02      5.8±0.27ms     4.4 MB/sec    1.00      5.7±0.29ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.01     19.3±0.90ms     2.1 MB/sec    1.00     19.1±1.17ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.30ms     3.6 MB/sec    1.09      5.1±0.24ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   593.9±46.89µs     5.0 MB/sec    1.03   610.7±32.02µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.55ms     2.8 MB/sec    1.03      9.2±0.54ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.03     10.2±0.54ms     4.0 MB/sec    1.00      9.9±0.49ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.10ms     7.9 MB/sec    1.00      2.0±0.15ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.07   246.7±18.02µs    12.0 MB/sec    1.00   230.5±14.44µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.5±0.20ms     5.7 MB/sec    1.00      4.4±0.20ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/number.rs`:24 on 2023-07-22 10:32_

We should pass `range.start()` as the second argument to all `dynamic_text` usages. The position is used in the generated source map and we'll use the source map for range formatting.

---

_@MichaReiser reviewed on 2023-07-22 10:32_

Oh wow nice! Excellent work. 

What I understand from the code is that the formatted now uses `dynamic_text` for every number constant even if it is already correctly formatted. This is, unfortunately, somewhat expensive because every `dynamic_text`  allocates a `String` when writing the text. 

Would you be interested in taking a stab at implementing an optimisation similar to what we did in `normalize_string`

https://github.com/astral-sh/ruff/blob/33657d3a1ccc5ccfefa26f50758ba46b74946da0/crates/ruff_python_formatter/src/expression/string.rs#L413-L481

The trick is to use a `Cow` and return `Cow::Borrowed` if the number is already correctly formatted (in which case we can print the code from the source using `source_text_slice` (extremely fast). The implementation returns a `Cow::Owned` if it reformatted the number and it then uses `dynamic_text`. 

The benefit of this optimisation is that we only pay the cost of allocating when formatting the file for the first time. Checking the formatting in subsequent passes (when there are no changes) takes the fast path instead. 

Let me know if that's something you're interested in and if so, if you want to tackle this as part of this PR or want to do a follow up instead.

---

_Comment by @lkh42t on 2023-07-22 11:01_

I'll implement optimization like `normalize_string`. Thanks!

---

_@MichaReiser requested changes on 2023-07-22 11:50_

Alright. I'll assign this back to you. Feel free to ping me if you have any questions and request another review once your done.

---

_Review requested from @MichaReiser by @lkh42t on 2023-07-22 12:48_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/number.rs`:158 on 2023-07-23 09:57_

i think you can simplify this to `input.as_bytes().get(index - 1).copied() == Some(b'.')`

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__attribute_access_on_number_literals.py.snap`:46 on 2023-07-23 10:00_

now that we have no more trailing dots in floats we can also fix the parentheses rules for them (maybe better in a follow-up PR so that the scope of this PR doesn't get too large)

---

_@konstin approved on 2023-07-23 10:00_

---

_Label `formatter` added by @konstin on 2023-07-23 13:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/number.rs`:157 on 2023-07-24 06:39_

I believe this could, at least in theory, result in indexing between two character boundaries if the text starts with `[e10]`. I don't think this is a problem in practice because it's impossible that the preceding character is a unicode character, because the number would then be part of an identifier. I still recommend fixing this just to be sure (e.g. by assigning `is_float on line 147)

---

_@MichaReiser approved on 2023-07-24 06:58_

---

_Merged by @MichaReiser on 2023-07-24 07:04_

---

_Closed by @MichaReiser on 2023-07-24 07:04_

---

_Branch deleted on 2023-07-29 05:26_

---
