```yaml
number: 10786
title: Add tests for all kinds of assignment statement
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/assign-stmt
created_at: 2024-04-05T04:24:30Z
updated_at: 2024-04-09T13:57:13Z
url: https://github.com/astral-sh/ruff/pull/10786
synced_at: 2026-01-10T22:37:01Z
```

# Add tests for all kinds of assignment statement

---

_Pull request opened by @dhruvmanila on 2024-04-05 04:24_

## Summary

This PR adds test cases for assignment, augmented assignment and annotated assignment statement.

---

_Label `parser` added by @dhruvmanila on 2024-04-05 04:24_

---

_Label `testing` added by @dhruvmanila on 2024-04-05 04:24_

---

_@dhruvmanila reviewed on 2024-04-05 04:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:204 on 2024-04-05 04:25_

I think it makes more sense that the parsing function starts with the expected token. It's also consistent with how other parsing function works.

---

_@dhruvmanila reviewed on 2024-04-05 04:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:872 on 2024-04-05 04:27_

Consuming the `=` token in the parsing function helps us remove the memory swap logic. What we do now is collect all the expression assuming that they are assignment targets and at the end pop one from the end which will become the value. Then, we'll validate the targets.

---

_@dhruvmanila reviewed on 2024-04-05 04:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2220 on 2024-04-05 04:29_

This now matches the behavior of CPython.

Earlier, for list, tuples and starred expression, we'd highlight the entire expression as being considered invalid for an assignment target which is incorrect. For example, the highlighted range below is incorrect:

```
38 | ... = 42
39 | *foo() = 42
40 | [x, foo(), y] = [42, 42, 42]
   | ^^^^^^^^^^^^^ Syntax Error: invalid assignment target
41 | [[a, b], [[42]], d] = [[1, 2], [[3]], 4]
42 | (x, foo(), y) = (42, 42, 42)
```

A list is allowed as an assignment target but it's one or more of the elements inside the list which is an invalid target. Same goes for tuples and starred expression. For example,

```
  |
1 | "abc": str = "def"
2 | [123, 456]: (int, int) = (1, 2)
  |  ^^^ Syntax Error: invalid assignment target
  |


  |
1 | "abc": str = "def"
2 | [123, 456]: (int, int) = (1, 2)
  |       ^^^ Syntax Error: invalid assignment target
```

Moving the logic in the `Parser` struct also avoids allocating a vector of `ParseError` and instead we can just add it directly.

---

_Comment by @codspeed-hq[bot] on 2024-04-05 04:30_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/assign-stmt)

### Merging #10786 will **degrade performances by 8.21%**

<sub>Comparing <code>dhruv/assign-stmt</code> (5b99e0e) with <code>dhruv/parser</code> (22d70b0)</sub>



### Summary

`⚡ 1` improvements
`❌ 2` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/assign-stmt)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/assign-stmt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.8 ms | -7.77% |
| ❌ | `parser[pydantic/types.py]` | 10.8 ms | 11.8 ms | -8.21% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.38% |


---

_@dhruvmanila reviewed on 2024-04-05 04:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:950 on 2024-04-05 04:32_

I'm adopting this pattern wherever I can. This is to avoid adding an invalid expression node in the AST where `None` is feasible.

---

_Marked ready for review by @dhruvmanila on 2024-04-05 04:38_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-05 04:38_

---

_Comment by @MichaReiser on 2024-04-05 06:43_

Hmm, can you double check if the codspeed regression is real?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:830 on 2024-04-05 06:45_

I'm not sure if that's true because `parse_list` will call `is_at_list_element` which returns `false` if the parser isn't positioned at a `=` in which case it goes into error recovery. 



---

_@MichaReiser approved on 2024-04-05 06:52_

---

_@MichaReiser reviewed on 2024-04-05 06:52_

The inline tests make reviewing so much easier. Thanks for implementing them.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:830 on 2024-04-05 10:56_

Hmm, yes but the method is only called when the parser sees the `=` token.

---

_@dhruvmanila reviewed on 2024-04-05 10:56_

---

_Comment by @github-actions[bot] on 2024-04-05 12:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2024-04-05 12:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:830 on 2024-04-05 12:45_

That's correct. I just think that the comment `Panics` isn't true. We would need an explicit `assert` to ensure that's true.

---

_Comment by @dhruvmanila on 2024-04-09 11:38_

Regarding the regression, I made one change which removed the regression locally but it's still showing up in Codspeed. This change is to take the list parsing path only if there are more than one assignment target. This is in a9b362e75ad64008fa6000f8abf8630313921ac7 commit.

Here's local benchmarks:

1. Here, the baseline is `dhruv/parser` and current build is `dhruv/assign-stmt` **without** the a9b362e75ad64008fa6000f8abf8630313921ac7 commit. It can be seen that the performance has indeed regressed:
```console
❯ cargo bench -p ruff_benchmark --bench parser --profile=profiling -- --baseline=parser
    Finished profiling [optimized + debuginfo] target(s) in 21.50s
     Running benches/parser.rs (target/profiling/deps/parser-d8622fac03063ec1)
parser/numpy/globals.py time:   [9.0928 µs 9.0953 µs 9.0979 µs]
                        thrpt:  [324.32 MiB/s 324.42 MiB/s 324.51 MiB/s]
                 change:
                        time:   [+4.2718% +4.3901% +4.5088%] (p = 0.00 < 0.05)
                        thrpt:  [-4.3143% -4.2055% -4.0968%]
                        Performance has regressed.
Found 9 outliers among 100 measurements (9.00%)
  4 (4.00%) high mild
  5 (5.00%) high severe
parser/unicode/pypinyin.py
                        time:   [34.544 µs 34.558 µs 34.572 µs]
                        thrpt:  [121.54 MiB/s 121.59 MiB/s 121.64 MiB/s]
                 change:
                        time:   [+1.7319% +2.1667% +2.4890%] (p = 0.00 < 0.05)
                        thrpt:  [-2.4286% -2.1207% -1.7025%]
                        Performance has regressed.
Found 5 outliers among 100 measurements (5.00%)
  1 (1.00%) low severe
  1 (1.00%) high mild
  3 (3.00%) high severe
parser/pydantic/types.py
                        time:   [257.20 µs 257.24 µs 257.28 µs]
                        thrpt:  [99.125 MiB/s 99.142 MiB/s 99.158 MiB/s]
                 change:
                        time:   [+2.1911% +2.3220% +2.4558%] (p = 0.00 < 0.05)
                        thrpt:  [-2.3969% -2.2693% -2.1441%]
                        Performance has regressed.
Found 10 outliers among 100 measurements (10.00%)
  2 (2.00%) high mild
  8 (8.00%) high severe
parser/numpy/ctypeslib.py
                        time:   [110.57 µs 110.59 µs 110.62 µs]
                        thrpt:  [150.52 MiB/s 150.56 MiB/s 150.60 MiB/s]
                 change:
                        time:   [+1.0194% +1.1439% +1.2748%] (p = 0.00 < 0.05)
                        thrpt:  [-1.2588% -1.1310% -1.0091%]
                        Performance has regressed.
Found 11 outliers among 100 measurements (11.00%)
  4 (4.00%) high mild
  7 (7.00%) high severe
parser/large/dataset.py time:   [656.06 µs 656.14 µs 656.22 µs]
                        thrpt:  [61.996 MiB/s 62.004 MiB/s 62.011 MiB/s]
                 change:
                        time:   [+2.1560% +2.2464% +2.3389%] (p = 0.00 < 0.05)
                        thrpt:  [-2.2854% -2.1971% -2.1105%]
                        Performance has regressed.
Found 8 outliers among 100 measurements (8.00%)
  3 (3.00%) high mild
  5 (5.00%) high severe
```

2. Here, the baseline is `dhruv/assign-stmt` **without**  a9b362e75ad64008fa6000f8abf8630313921ac7 and the current build is `dhruv/assign-stmt` with the a9b362e75ad64008fa6000f8abf8630313921ac7 commit. It can be seen that the performance has indeed improved:
```console
❯ cargo bench -p ruff_benchmark --bench parser --profile=profiling -- --baseline=orig-assign-stmt
    Finished profiling [optimized + debuginfo] target(s) in 21.43s
     Running benches/parser.rs (target/profiling/deps/parser-d8622fac03063ec1)
parser/numpy/globals.py time:   [8.6279 µs 8.6297 µs 8.6316 µs]
                        thrpt:  [341.84 MiB/s 341.92 MiB/s 341.99 MiB/s]
                 change:
                        time:   [-4.7410% -4.6397% -4.5339%] (p = 0.00 < 0.05)
                        thrpt:  [+4.7492% +4.8654% +4.9769%]
                        Performance has improved.
Found 6 outliers among 100 measurements (6.00%)
  2 (2.00%) low mild
  4 (4.00%) high severe
parser/unicode/pypinyin.py
                        time:   [34.073 µs 34.080 µs 34.087 µs]
                        thrpt:  [123.27 MiB/s 123.30 MiB/s 123.32 MiB/s]
                 change:
                        time:   [-0.7874% -0.6685% -0.5517%] (p = 0.00 < 0.05)
                        thrpt:  [+0.5547% +0.6730% +0.7936%]
                        Change within noise threshold.
Found 7 outliers among 100 measurements (7.00%)
  1 (1.00%) low mild
  2 (2.00%) high mild
  4 (4.00%) high severe
parser/pydantic/types.py
                        time:   [248.44 µs 248.48 µs 248.54 µs]
                        thrpt:  [102.61 MiB/s 102.64 MiB/s 102.65 MiB/s]
                 change:
                        time:   [-3.1474% -3.0314% -2.9278%] (p = 0.00 < 0.05)
                        thrpt:  [+3.0161% +3.1262% +3.2497%]
                        Performance has improved.
Found 11 outliers among 100 measurements (11.00%)
  4 (4.00%) high mild
  7 (7.00%) high severe
parser/numpy/ctypeslib.py
                        time:   [109.71 µs 109.75 µs 109.83 µs]
                        thrpt:  [151.61 MiB/s 151.72 MiB/s 151.78 MiB/s]
                 change:
                        time:   [-0.7424% -0.6053% -0.4651%] (p = 0.00 < 0.05)
                        thrpt:  [+0.4672% +0.6090% +0.7479%]
                        Change within noise threshold.
Found 14 outliers among 100 measurements (14.00%)
  7 (7.00%) high mild
  7 (7.00%) high severe
parser/large/dataset.py time:   [632.69 µs 632.90 µs 633.14 µs]
                        thrpt:  [64.256 MiB/s 64.280 MiB/s 64.301 MiB/s]
                 change:
                        time:   [-3.1404% -3.0469% -2.9653%] (p = 0.00 < 0.05)
                        thrpt:  [+3.0559% +3.1426% +3.2422%]
                        Performance has improved.
Found 5 outliers among 100 measurements (5.00%)
  3 (3.00%) high mild
  2 (2.00%) high severe
```

3. And for clarity, here the baseline is `dhruv/parser` and current build is `dhruv/assign-stmt` with the a9b362e75ad64008fa6000f8abf8630313921ac7 commit. There are no visible regressions:

```console
❯ cargo bench -p ruff_benchmark --bench parser --profile=profiling -- --baseline=parser
    Finished profiling [optimized + debuginfo] target(s) in 21.37s
     Running benches/parser.rs (target/profiling/deps/parser-d8622fac03063ec1)
parser/numpy/globals.py time:   [8.6349 µs 8.6366 µs 8.6384 µs]
                        thrpt:  [341.58 MiB/s 341.65 MiB/s 341.72 MiB/s]
                 change:
                        time:   [-0.9459% -0.8310% -0.7193%] (p = 0.00 < 0.05)
                        thrpt:  [+0.7245% +0.8379% +0.9550%]
                        Change within noise threshold.
Found 8 outliers among 100 measurements (8.00%)
  2 (2.00%) high mild
  6 (6.00%) high severe
parser/unicode/pypinyin.py
                        time:   [33.984 µs 33.989 µs 33.995 µs]
                        thrpt:  [123.60 MiB/s 123.62 MiB/s 123.64 MiB/s]
                 change:
                        time:   [+0.1451% +0.5866% +0.9046%] (p = 0.00 < 0.05)
                        thrpt:  [-0.8965% -0.5832% -0.1449%]
                        Change within noise threshold.
Found 8 outliers among 100 measurements (8.00%)
  5 (5.00%) high mild
  3 (3.00%) high severe
parser/pydantic/types.py
                        time:   [251.89 µs 251.94 µs 251.99 µs]
                        thrpt:  [101.21 MiB/s 101.23 MiB/s 101.25 MiB/s]
                 change:
                        time:   [+0.0362% +0.1804% +0.3243%] (p = 0.01 < 0.05)
                        thrpt:  [-0.3232% -0.1801% -0.0362%]
                        Change within noise threshold.
Found 7 outliers among 100 measurements (7.00%)
  2 (2.00%) high mild
  5 (5.00%) high severe
parser/numpy/ctypeslib.py
                        time:   [108.39 µs 108.63 µs 108.99 µs]
                        thrpt:  [152.78 MiB/s 153.28 MiB/s 153.63 MiB/s]
                 change:
                        time:   [-1.1797% -0.9915% -0.7578%] (p = 0.00 < 0.05)
                        thrpt:  [+0.7636% +1.0014% +1.1938%]
                        Change within noise threshold.
Found 3 outliers among 100 measurements (3.00%)
  2 (2.00%) high mild
  1 (1.00%) high severe
parser/large/dataset.py time:   [634.40 µs 634.60 µs 634.82 µs]
                        thrpt:  [64.086 MiB/s 64.108 MiB/s 64.128 MiB/s]
                 change:
                        time:   [-1.1884% -1.0887% -0.9926%] (p = 0.00 < 0.05)
                        thrpt:  [+1.0025% +1.1007% +1.2027%]
                        Change within noise threshold.
Found 3 outliers among 100 measurements (3.00%)
  1 (1.00%) high mild
  2 (2.00%) high severe
```

I ran hyperfine benchmarks as well on CPython repository which gave me the same results.

---

_Merged by @dhruvmanila on 2024-04-09 13:45_

---

_Closed by @dhruvmanila on 2024-04-09 13:45_

---

_Branch deleted on 2024-04-09 13:45_

---
