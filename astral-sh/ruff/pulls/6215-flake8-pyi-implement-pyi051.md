```yaml
number: 6215
title: "[`flake8-pyi`] Implement PYI051"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI051
created_at: 2023-08-01T00:15:02Z
updated_at: 2023-08-02T20:15:22Z
url: https://github.com/astral-sh/ruff/pull/6215
synced_at: 2026-01-12T02:58:30Z
```

# [`flake8-pyi`] Implement PYI051

---

_Pull request opened by @LaBatata101 on 2023-08-01 00:15_

## Summary
Checks for the presence of redundant `Literal` types and builtin super types in an union. See [original source](https://github.com/PyCQA/flake8-pyi/blob/2a86db8271dc500247a8a69419536240334731cf/pyi.py#L1261).

This implementation has a couple of differences from the original. The first one is, we support the `complex` and `float` builtin types. The second is, when reporting diagnostic for a `Literal` with multiple members of the same type, we print the entire `Literal` while `flak8` only prints the `Literal` with its first member.
For example:
```python
from typing import Literal

x: Literal[1, 2] | int
```  
Ruff will show `Literal[1, 2]` while flake8 only shows `Literal[1]`.

```shell
$ ruff crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:4:18: PYI051 `Literal["foo"]` is redundant in an union with `str`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:5:37: PYI051 `Literal[b"bar", b"foo"]` is redundant in an union with `bytes`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:6:37: PYI051 `Literal[5]` is redundant in an union with `int`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:6:67: PYI051 `Literal["foo"]` is redundant in an union with `str`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:7:37: PYI051 `Literal[b"str_bytes"]` is redundant in an union with `bytes`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:7:51: PYI051 `Literal[42]` is redundant in an union with `int`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:9:31: PYI051 `Literal[1J]` is redundant in an union with `complex`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:9:53: PYI051 `Literal[3.14]` is redundant in an union with `float`
Found 8 errors.
```

```shell
$ flake8 crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:4:18: Y051 "Literal['foo']" is redundant in a union with "str"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:5:37: Y051 "Literal[b'bar']" is redundant in a union with "bytes"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:6:37: Y051 "Literal[5]" is redundant in a unionwith "int"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:6:67: Y051 "Literal['foo']" is redundant in a union with "str"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:7:37: Y051 "Literal[b'str_bytes']" is redundantin a union with "bytes"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI051.pyi:7:51: Y051 "Literal[42]" is redundant in a union with "int"
```

While implementing this rule, I found a bug in the `is_unchecked_union` check. This is the new check.

https://github.com/astral-sh/ruff/blob/1ab86bad35e5edd1ba4cd82b0eb4c78416509dac/crates/ruff/src/checkers/ast/analyze/expression.rs#L85-L102

The purpose of the check was to prevent rules from navigating through nested `Union`s, as they already handle nested `Union`s. The way it was implemented, this was not happening, the rules were getting executed more than one time and sometimes were receiving expressions that were not `Union`. For example, with the following code:
 ```python
  typing.Union[Literal[5], int, typing.Union[Literal["foo"], str]]
 ```

The rules were receiving the expressions in the following order:
     - `typing.Union[Literal[5], int, typing.Union[Literal["foo"], str]]`
     - `Literal[5]`
     - `typing.Union[Literal["foo"], str]]`

This was causing `PYI030` to report redundant information, for example:
 ```python
  typing.Union[Literal[5], int, typing.Union[Literal["foo"], Literal["bar"]]]
 ```
This is the `PYI030` output for this code:
```shell
PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[5, "foo", "bar"]`
YI030 Multiple literal members in a union. Use a single literal, e.g.`Literal[5, "foo"]`
```

If I haven't misinterpreted the rule, that looks incorrect. I didn't have the time to check the `PYI016` rule.

The last thing is, I couldn't find a reason for the "Why is this bad?" section for `PYI051`.

Ref: #848 

## Test Plan

Snapshots and manual runs of flake8.


---

_Comment by @github-actions[bot] on 2023-08-01 00:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      8.8±0.08ms     4.6 MB/sec    1.00      8.5±0.06ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1675.0±25.16µs     9.9 MB/sec    1.00  1661.4±30.06µs    10.0 MB/sec
formatter/numpy/globals.py                 1.01    173.8±0.44µs    17.0 MB/sec    1.00    172.7±1.77µs    17.1 MB/sec
formatter/pydantic/types.py                1.03      3.8±0.09ms     6.8 MB/sec    1.00      3.7±0.08ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.04     11.7±0.04ms     3.5 MB/sec    1.00     11.3±0.01ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.1±0.07ms     5.4 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    316.9±1.07µs     9.3 MB/sec    1.01    321.2±5.40µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.06      5.4±0.05ms     4.7 MB/sec    1.00      5.1±0.01ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.01      6.1±0.04ms     6.6 MB/sec    1.00      6.1±0.03ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1199.5±6.73µs    13.9 MB/sec    1.01   1213.7±4.57µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    120.5±3.44µs    24.5 MB/sec    1.01    121.4±1.03µs    24.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.01ms     9.7 MB/sec    1.00      2.6±0.01ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.08ms     4.1 MB/sec    1.02     10.0±0.10ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1926.9±36.83µs     8.6 MB/sec    1.00  1935.9±32.98µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    214.3±4.08µs    13.8 MB/sec    1.02    218.7±9.08µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.06ms     6.1 MB/sec    1.02      4.3±0.08ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.19ms     3.1 MB/sec    1.00     13.3±0.15ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.7 MB/sec    1.00      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    435.6±5.61µs     6.8 MB/sec    1.00    432.2±5.28µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.08ms     4.2 MB/sec    1.00      6.0±0.08ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.08ms     5.6 MB/sec    1.00      7.3±0.08ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1506.5±18.21µs    11.1 MB/sec    1.00  1480.7±19.07µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    170.0±3.40µs    17.4 MB/sec    1.00    165.8±2.20µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.07ms     8.0 MB/sec    1.00      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @LaBatata101 on 2023-08-01 01:03_

Looks like something is wrong in the implementation

---

_@charliermarsh reviewed on 2023-08-01 04:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:124 on 2023-08-01 04:20_

@LaBatata101 - I think the issue is that we needed to treat these two cases as separate. Otherwise, we were flagging cases like `Literal[1, 2]`, since we'd detect that we have `int` in the `Union` (by way of visiting the `1` and `2`).

---

_Comment by @charliermarsh on 2023-08-01 04:43_

Oh sorry, I missed your comment about the `is_unchecked_union` check and accidentally reverted that code when merging. @zanieb, do you mind reviewing that comment in the PR summary and helping understand whether it's a valid change?

---

_Label `rule` added by @charliermarsh on 2023-08-01 04:44_

---

_Assigned to @zanieb by @zanieb on 2023-08-02 19:26_

---

_Comment by @charliermarsh on 2023-08-02 19:37_

Mentioned to @zanieb offline but I think the current `is_unchecked_union` is "more correct" than the version proposed here (which will miss cases like `f(Union[...])`, since it ignores anything with a parent expression). However, there's at least one bug in the current version unrelated to the logic of the check itself and instead related to some quirks on the semantic model traversal, I think. I will document separately.

---

_Merged by @charliermarsh on 2023-08-02 19:37_

---

_Closed by @charliermarsh on 2023-08-02 19:37_

---

_Comment by @charliermarsh on 2023-08-02 19:46_

Documented here: https://github.com/astral-sh/ruff/issues/6285

---

_Branch deleted on 2023-08-02 20:15_

---
