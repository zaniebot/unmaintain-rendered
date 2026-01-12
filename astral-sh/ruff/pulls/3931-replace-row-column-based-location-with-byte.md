```yaml
number: 3931
title: "Replace row/column based `Location` with byte-offsets."
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: byte-offset-parser
created_at: 2023-04-11T10:09:30Z
updated_at: 2023-04-26T18:32:28Z
url: https://github.com/astral-sh/ruff/pull/3931
synced_at: 2026-01-12T15:55:14Z
```

# Replace row/column based `Location` with byte-offsets.

---

_@MichaReiser_

## Summary

This PR changes ruff to use our own fork of `RustPython` that replaces `Location { row: u32, column: u32 }` with `TextSize` https://github.com/charliermarsh/RustPython/pull/4. The main motivation for this change is to ship the logical line rules. Enabling the logical line rules regresses performance by as much as 50% because the rules need to slice into the source string, which requires building and querying the `LineIndex`. Using byte offsets everywhere trades the need from having to build the `LineIndex` to inspect the source text in lint rules with re-computing the row and column information when rendering diagnostics. This is a favourable trade because most projects using ruff only have very few diagnostics.


## Notable Changes

### `SourceCodeFile`

It is now necessary to always include the source code when passing `Message`s because the source text is necessary to re-compute the row and column positions for byte offsets (`TextSize`). Previously, the source text was only included when using `--show-source`. This results in a noticeable slowdown in projects with many (ten thousand) diagnostics. 


### `Locator`

The `Locator` now exposes methods to:

* For a byte offset, compute its line's start and end positions. Useful to compute the indent or when replacing a full line. 
* For a text range, compute the offset of the first line and the last line in that range. Useful when replacing all lines in a range.

The computations are performed on demand without querying the `LineIndex`. 

The `Locator` still has a lazy computed `LineIndex` because we have a few diagnostics that use a line number as part of their message. 

### `SourceCode`

`SourceCode` now provides methods to compute the `SourceLocation` (row column information) given an offset. 

### `UniversalNewline`

The `UniversalNewline` iterator now returns `Line` items instead of `&str`. This is necessary because many lints need to know the offset of the `nth` line and summing the `text.text_len()` doesn't give you the right result because the `text` does not include the trailing newline character:

```py
def f:
    pass
```

The text len of the first line is 6 bytes because the line does not include the trailing newline character. 

The `Line` struct provides methods o get a line's start offset, end offset, range, and text. It also provides methods to get the text, end offset, and range, including the trailing newline character.

### Use `TextRange` for ranges

Consistently uses `TextRange` in favor of: `(Location, Location)` and `start: Location, end: Location` because `TextRange` better communicates that the two offsets are related.

Replaces all references of `Range` with `TextRange` and deletes `Range`.

### Use `TextSize` instead of `Location`

Replaces all references to `Location` with `TextSize`. 


### `Stylist` 

This PR removes the lazy computations for `indention` and `quote` because slicing into the source string is now cheap.

### `Indexer`

The `Indexer` used to store the line numbers of commented lines, lines with continuations, and lines with multiline strings.  This is no longer feasible because it would require computing the line numbers. The new implementation stores the line-start offset for continuous lines and the `TextRange` for comments and multiline strings. 

Storing the `TextRange` instead of line numbers helped to fix a false-negative where a mixed spaces-tab indent at the start of a multiline string was not reported because the analysis incorrectly assumed that it is part of the multiline string. 

### Noqa

This PR now stores the `TextRange` of the line for each noqa comment sorted in ascending order by the start position. Testing whether a `diagnostic` is suppressed requires a binary search on the ranges to test if any range contains the `diagnostic`s start location. 

This PR further replaces the mapping to suppress some syntaxes on other lines by a `TextRange` vector where every entry means that a position falling into that range should be remapped to the end of the range. 


### isort directives

Similar to noqa. It now stores the `TextRange`s instead of the line numbers for the areas where sorting is disabled. This PR now only stores the `TextSize` for split positions as this proves to be sufficient. 

## Benchmark 

TLDR: 10% performance improvement for projects with few diagnostics. Identical performance or small regression for projects with many diagnostics. The new implementation with logical-lines enabled outperforms `main` with logical-lines disabled. 

### Micro Benchmarks

This PR improves the default-rules benchmark by 6-15% and the all-rules benchmark by 4-8%. More importantly, ruff with logical-lines enabled is as fast or even faster than `main`. This should allow us to ship logical lines without causing a runtime regression. 


<pre>group                                      bytes                                  bytes-logical                          main                                   main-logical
-----                                      -----                                  -------------                          ----                                   ------------
linter/all-rules/large/dataset.py<span style="color:#26A269"><b>          1.00      8.6±0.08ms     4.7 MB/sec</b></span>    1.03      8.9±0.17ms     4.6 MB/sec    1.04      8.9±0.12ms     4.6 MB/sec    1.10      9.4±0.01ms     4.3 MB/sec
linter/all-rules/numpy/ctypeslib.py<span style="color:#26A269"><b>        1.00      2.0±0.07ms     8.3 MB/sec</b></span>    1.05      2.1±0.03ms     7.9 MB/sec    1.05      2.1±0.03ms     7.9 MB/sec    1.13      2.3±0.00ms     7.4 MB/sec
linter/all-rules/numpy/globals.py<span style="color:#26A269"><b>          1.00    220.7±4.63µs    13.4 MB/sec</b></span>    1.04    228.8±2.74µs    12.9 MB/sec    1.08    238.8±1.17µs    12.4 MB/sec    1.16    256.8±1.25µs    11.5 MB/sec
linter/all-rules/pydantic/types.py<span style="color:#26A269"><b>         1.00      3.5±0.04ms     7.3 MB/sec</b></span>    1.05      3.7±0.10ms     6.9 MB/sec    1.07      3.7±0.02ms     6.8 MB/sec    1.14      4.0±0.02ms     6.4 MB/sec
linter/default-rules/large/dataset.py<span style="color:#26A269"><b>      1.00      4.3±0.07ms     9.5 MB/sec</b></span>    1.07      4.6±0.03ms     8.8 MB/sec    1.08      4.6±0.05ms     8.8 MB/sec    1.18      5.1±0.08ms     8.1 MB/sec
linter/default-rules/numpy/ctypeslib.py<span style="color:#26A269"><b>    1.00    870.1±5.69µs    19.1 MB/sec</b></span>    1.12    971.9±3.08µs    17.1 MB/sec    1.15   1002.2±4.42µs    16.6 MB/sec    1.27   1108.6±3.25µs    15.0 MB/sec
linter/default-rules/numpy/globals.py<span style="color:#26A269"><b>      1.00     95.9±0.72µs    30.8 MB/sec</b></span>    1.08    103.2±1.69µs    28.6 MB/sec    1.06    102.0±1.01µs    28.9 MB/sec    1.19    114.5±0.47µs    25.8 MB/sec
linter/default-rules/pydantic/types.py<span style="color:#26A269"><b>     1.00  1883.0±10.58µs    13.5 MB/sec</b></span>    1.12      2.1±0.01ms    12.1 MB/sec    1.10      2.1±0.00ms    12.3 MB/sec    1.23      2.3±0.01ms    11.0 MB/sec
</pre>

It's worth pointing out that the relative slowdown introduced by enabling the logical lines lint rules remains unchanged. I'm surprised by this because it doesn't show the improvement I expected from removing the `LineIndex` computation from the linting path. 

<pre>group                                      main                                   main-logical
-----                                      ----                                   ------------
linter/all-rules/large/dataset.py<span style="color:#26A269"><b>          1.00      8.9±0.12ms     4.6 MB/sec</b></span>    1.06      9.4±0.01ms     4.3 MB/sec
linter/all-rules/numpy/ctypeslib.py<span style="color:#26A269"><b>        1.00      2.1±0.03ms     7.9 MB/sec</b></span>    1.07      2.3±0.00ms     7.4 MB/sec
linter/all-rules/numpy/globals.py<span style="color:#26A269"><b>          1.00    238.8±1.17µs    12.4 MB/sec</b></span>    1.08    256.8±1.25µs    11.5 MB/sec
linter/all-rules/pydantic/types.py<span style="color:#26A269"><b>         1.00      3.7±0.02ms     6.8 MB/sec</b></span>    1.07      4.0±0.02ms     6.4 MB/sec
linter/default-rules/large/dataset.py<span style="color:#26A269"><b>      1.00      4.6±0.05ms     8.8 MB/sec</b></span>    1.09      5.1±0.08ms     8.1 MB/sec
linter/default-rules/numpy/ctypeslib.py<span style="color:#26A269"><b>    1.00   1002.2±4.42µs    16.6 MB/sec</b></span>    1.11   1108.6±3.25µs    15.0 MB/sec
linter/default-rules/numpy/globals.py<span style="color:#26A269"><b>      1.00    102.0±1.01µs    28.9 MB/sec</b></span>    1.12    114.5±0.47µs    25.8 MB/sec
linter/default-rules/pydantic/types.py<span style="color:#26A269"><b>     1.00      2.1±0.00ms    12.3 MB/sec</b></span>    1.12      2.3±0.01ms    11.0 MB/sec
</pre>

<pre>group                                      bytes                                  bytes-logical
-----                                      -----                                  -------------
linter/all-rules/large/dataset.py<span style="color:#26A269"><b>          1.00      8.6±0.08ms     4.7 MB/sec</b></span>    1.03      8.9±0.17ms     4.6 MB/sec
linter/all-rules/numpy/ctypeslib.py<span style="color:#26A269"><b>        1.00      2.0±0.07ms     8.3 MB/sec</b></span>    1.05      2.1±0.03ms     7.9 MB/sec
linter/all-rules/numpy/globals.py<span style="color:#26A269"><b>          1.00    220.7±4.63µs    13.4 MB/sec</b></span>    1.04    228.8±2.74µs    12.9 MB/sec
linter/all-rules/pydantic/types.py<span style="color:#26A269"><b>         1.00      3.5±0.04ms     7.3 MB/sec</b></span>    1.05      3.7±0.10ms     6.9 MB/sec
linter/default-rules/large/dataset.py<span style="color:#26A269"><b>      1.00      4.3±0.07ms     9.5 MB/sec</b></span>    1.07      4.6±0.03ms     8.8 MB/sec
linter/default-rules/numpy/ctypeslib.py<span style="color:#26A269"><b>    1.00    870.1±5.69µs    19.1 MB/sec</b></span>    1.12    971.9±3.08µs    17.1 MB/sec
linter/default-rules/numpy/globals.py<span style="color:#26A269"><b>      1.00     95.9±0.72µs    30.8 MB/sec</b></span>    1.08    103.2±1.69µs    28.6 MB/sec
linter/default-rules/pydantic/types.py<span style="color:#26A269"><b>     1.00  1883.0±10.58µs    13.5 MB/sec</b></span>    1.12      2.1±0.01ms    12.1 MB/sec
</pre>

### CPython

This benchmark measures the worst-case performance: A project with many violations. 

* Performance regresses for loading cached results (except when using `--show-source`). This is expected because printing diagnostics now always requires storing the source text and computing the source locations adds some overhead as well.
* Slight improvement for `--no-cache` as seen in the micro benchmarks
* 10% performance improvement when using silent (`-s`). This shows the potential of the refactor for projects with few or no diagnostics. Silent still pays the overhead for storing the source text for every diagnostic, but the implementation doesn't compute the `LineIndex`. 

<details><summary>Benchmark results</summary>

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./ruff-bytes ./crates/ruff/resources/test/cpython/ -e   ` | 44.4 ± 2.0 | 40.3 | 49.0 | 1.17 ± 0.08 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e   ` | 37.9 ± 1.8 | 34.1 | 43.5 | 1.00 |
| `./ruff-bytes ./crates/ruff/resources/test/cpython/ -e --no-cache  ` | 200.2 ± 4.0 | 194.5 | 210.1 | 5.28 ± 0.28 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e --no-cache  ` | 215.7 ± 6.5 | 203.5 | 228.0 | 5.69 ± 0.32 |
| `./ruff-bytes ./crates/ruff/resources/test/cpython/ -e  --select=ALL ` | 401.6 ± 9.6 | 391.4 | 420.1 | 10.60 ± 0.57 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e  --select=ALL ` | 395.6 ± 8.8 | 383.8 | 412.5 | 10.44 ± 0.55 |
| `./ruff-bytes ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL ` | 691.1 ± 9.6 | 677.6 | 709.3 | 18.24 ± 0.91 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL ` | 716.7 ± 11.3 | 700.7 | 736.3 | 18.91 ± 0.96 |
| `./ruff-bytes ./crates/ruff/resources/test/cpython/ -e   --show-source` | 85.7 ± 2.4 | 81.7 | 93.2 | 2.26 ± 0.13 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e   --show-source` | 85.0 ± 2.0 | 81.9 | 90.4 | 2.24 ± 0.12 |
| `./ruff-bytes ./crates/ruff/resources/test/cpython/ -e --no-cache  --show-source` | 245.7 ± 4.4 | 238.5 | 251.2 | 6.48 ± 0.33 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e --no-cache  --show-source` | 259.8 ± 5.1 | 251.3 | 272.6 | 6.86 ± 0.36 |
| `./ruff-bytes ./crates/ruff/resources/test/cpython/ -e  --select=ALL --show-source` | 1507.1 ± 18.6 | 1474.6 | 1531.3 | 39.77 ± 1.98 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e  --select=ALL --show-source` | 1459.6 ± 23.5 | 1426.0 | 1500.0 | 38.52 ± 1.96 |
| `./ruff-bytes ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL --show-source` | 1770.1 ± 21.8 | 1747.0 | 1822.1 | 46.71 ± 2.32 |
| `./ruff-main ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL --show-source` | 1799.7 ± 31.7 | 1762.7 | 1856.2 | 47.49 ± 2.44 |


</details>

### Homeassitant

Best case benchmark: A project with very few diagnostics (10). 

* Performance is unchanged when running with caching
* Performance improves by about 10% when caching is disabled.

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `../ruff/ruff-bytes . -e ` | 51.4 ± 2.0 | 44.6 | 55.7 | 1.00 |
| `../ruff/ruff-main . -e ` | 51.4 ± 2.3 | 45.4 | 57.3 | 1.00 ± 0.06 |
| `../ruff/ruff-bytes . -e --no-cache` | 360.0 ± 3.0 | 355.4 | 364.0 | 7.01 ± 0.28 |
| `../ruff/ruff-main . -e --no-cache` | 387.4 ± 5.9 | 380.1 | 396.2 | 7.54 ± 0.32 |

Enabling logical lines introduces many new errors (2000), no longer showing the best case. But the new implementation still outperforms the old with logical-lines enabled and remains about 10% faster. 


| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `../ruff/ruff-bytes-logical . -e ` | 53.7 ± 2.3 | 49.1 | 59.8 | 1.03 ± 0.06 |
| `../ruff/ruff-main-logical . -e ` | 52.1 ± 2.4 | 46.6 | 56.6 | 1.00 |
| `../ruff/ruff-bytes-logical . -e --no-cache` | 383.2 ± 2.7 | 378.9 | 387.6 | 7.35 ± 0.34 |
| `../ruff/ruff-main-logical . -e --no-cache` | 417.7 ± 6.3 | 411.8 | 433.6 | 8.01 ± 0.38 |


## Test Plan

* [x] The ecosystem check shows no changes (after applying #4006, #4007, and #4005)
* [x] WASM playground
* [x] Test LSP
* [x] Test fixes across repositories
* [x] Test `--add-noqa` with airflow repository
* [x] Test `--fix` with airflow repository

## ~Breaking Changes~

~This PR changes the column numbers of fixes in the JSON output to be one indexed to align the column numbers with the `Diagnostic` start and end columns. I can undo this change but I got it "for free" by using `SourceLocation` consistently.~

I reverted the change in this PR and extracted it into https://github.com/charliermarsh/ruff/pull/4007


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/format/expr.rs`:100 on 2023-04-11 10:12_

CC: @charliermarsh it could be useful to define a `type PyFrmatter<'a> = Formatter<ASTFormatContext<'a>>` type alias. 

---

_@MichaReiser reviewed on 2023-04-11 10:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/cst/mod.rs`:56 on 2023-04-11 10:16_

@charliermarsh I think we want to use `node` here to avoid comparing the location of the `Located`. I ran into stack overflows in attaching the tokens before making this change because the body of a except handler contained itself as a child.... :open_mouth: 

---

_@MichaReiser reviewed on 2023-04-11 10:16_

---

_Comment by @evanrittenhouse on 2023-04-11 15:29_

Just for my own curiosity, what's the context for this? Why's it necessary?

---

_Comment by @MichaReiser on 2023-04-12 07:18_

> Just for my own curiosity, what's the context for this? Why's it necessary?

Strictly speaking, it isn't necessary from a functional point of view, but using byte offsets helps to improve performance and reduce memory consumption. 

I started investigating switching to byte offsets because enabling the pycodestyle (logical line) rules #3689 results in a 20%-50% performance regression, even tough I already improved the performance of the rules themselves. A key observation is that benchmarks for the default-rules regress more than for the all rules benchmarks. 

```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.5±0.06ms     3.0 MB/sec    1.20     16.3±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.18      4.2±0.01ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    490.6±2.52µs     6.0 MB/sec    1.16    567.3±0.81µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.3 MB/sec    1.23      7.4±0.02ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.38      9.9±0.04ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1615.1±3.91µs    10.3 MB/sec    1.39      2.3±0.01ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.5±0.32µs    16.3 MB/sec    1.52    274.0±4.45µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.41      4.8±0.02ms     5.4 MB/sec
```

This is because the pycodestyle rules are the first rules in the default set that inspect the source code (trivia). The challenge with inspecting the source code is that you can't slice a string with a row/column location. This isn't possible: `string[row=1, column=5..10]`. Ruff works around this by building a `LineIndex` that maps `row` and `column` locations to byte offsets (somewhat expensive), and than uses that index (lookup is cheap) to retrieve the string locations. 

My goal with using byte-offsets is to remove the need to build a `LineIndex` from the linting phase. It will still be necessary to build the line index when we emit diagnostics, because, as a user, I strongly prefer row/column numbers over byte indices ;). Having to build a `LineIndex` for all files with diagnostics  may result in a performance regression for projects where most files have diagnostics, but this is rare for projects using ruff that tend to have zero or only few diagnostics. 

There are other, non-pycodestyle specific reasons why I want to adopt byte offsets:

* Code size reduction: Comparing a byte-offset is a single `u32` comparison, compared to two `u32` comparisons for a `row`/`column` based source location. I further made `end_location` mandatory on `Located` which helps removing some `unwrap` code paths. The result for `ruff_python_formatter` is a small code size reduction: ~4.4MB → 4.3
* Reduced memory consumption: A row/column based `Location` uses 8 bytes (4 bytes for the row and column). A byte offset only uses half of that (single `u32`). This means, we reduce the size of every `LexResult`, `Located`, and AST node by 8 bytes (-4 bytes for the `start`, and -4 bytes for the `end` locations). 
* Performance improvements: Writing and reading less data, and fewer CPU instructions should help to improve overall performance. A preliminary benchmark comparing the parser and visitor (counting all statements) performance shows a 10-15% performance improvement. Whether we're able to reap all these improvements in Ruff depends on how efficiently we can rewrite the logic that uses row information in the linting phase today (noqa, isort comments, commented lines, etc)

<pre><span style="color:#26A269">parser/numpy/globals.py</span> time:   [65.752 µs <b>65.844 µs</b> 65.973 µs]
                        thrpt:  [44.725 MiB/s <b>44.813 MiB/s</b> 44.876 MiB/s]
                 change:
                        time:   [-5.8033% <span style="color:#26A269"><b>-5.5729%</b></span> -5.3542%] (p = 0.00 &lt; 0.05)
                        thrpt:  [+5.6571% <span style="color:#26A269"><b>+5.9018%</b></span> +6.1609%]
                        Performance has <span style="color:#26A269">improved</span>.
<span style="color:#A2734C">Found 21 outliers among 100 measurements (21.00%)</span>
  15 (15.00%) low severe
  2 (2.00%) low mild
  3 (3.00%) high mild
  1 (1.00%) high severe
<span style="color:#26A269">parser/pydantic/types.py</span>
                        time:   [1.4442 ms <b>1.4453 ms</b> 1.4466 ms]
                        thrpt:  [17.630 MiB/s <b>17.646 MiB/s</b> 17.659 MiB/s]
                 change:
                        time:   [-12.393% <span style="color:#26A269"><b>-12.225%</b></span> -11.904%] (p = 0.00 &lt; 0.05)
                        thrpt:  [+13.512% <span style="color:#26A269"><b>+13.927%</b></span> +14.146%]
                        Performance has <span style="color:#26A269">improved</span>.
<span style="color:#A2734C">Found 9 outliers among 100 measurements (9.00%)</span>
  3 (3.00%) high mild
  6 (6.00%) high severe
<span style="color:#26A269">parser/numpy/ctypeslib.py</span>
                        time:   [647.58 µs <b>650.18 µs</b> 652.90 µs]
                        thrpt:  [25.503 MiB/s <b>25.610 MiB/s</b> 25.713 MiB/s]
                 change:
                        time:   [-14.351% <span style="color:#26A269"><b>-14.154%</b></span> -13.948%] (p = 0.00 &lt; 0.05)
                        thrpt:  [+16.209% <span style="color:#26A269"><b>+16.488%</b></span> +16.756%]
                        Performance has <span style="color:#26A269">improved</span>.
<span style="color:#A2734C">Found 18 outliers among 100 measurements (18.00%)</span>
  17 (17.00%) high mild
  1 (1.00%) high severe
<span style="color:#26A269">parser/large/dataset.py</span> time:   [3.5024 ms <b>3.5104 ms</b> 3.5195 ms]
                        thrpt:  [11.559 MiB/s <b>11.589 MiB/s</b> 11.616 MiB/s]
                 change:
                        time:   [-11.825% <span style="color:#26A269"><b>-11.603%</b></span> -11.374%] (p = 0.00 &lt; 0.05)
                        thrpt:  [+12.834% <span style="color:#26A269"><b>+13.126%</b></span> +13.411%]
                        Performance has <span style="color:#26A269">improved</span>.
<span style="color:#A2734C">Found 13 outliers among 100 measurements (13.00%)</span>
  7 (7.00%) low severe
  1 (1.00%) low mild
  1 (1.00%) high mild
  4 (4.00%) high severe
</pre>

Other compilers using byte-offsts:

* Zig: [string interning](https://mitchellh.com/zig/astgen)
* [Roslyn](https://learn.microsoft.com/en-us/dotnet/api/microsoft.codeanalysis.text.textspan?view=roslyn-dotnet-4.3.0): C#/VisualBasic compiler
* [Rust](https://doc.rust-lang.org/stable/nightly-rustc/rustc_span/struct.SpanData.html)

---

_Comment by @MichaReiser on 2023-04-12 08:34_

Current dependencies on/for this PR:
* main
  * **PR #3990** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3990" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #3931** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3931" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #3985** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3985" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #4022** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/4022" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #4120** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/4120" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #4121** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/4121" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/3931?utm_source=stack-comment).

---

_Comment by @evanrittenhouse on 2023-04-13 01:10_

Thanks for the detailed explanation @MichaReiser! If you don't mind - where do the offsets come from? Column offset makes sense (obviously just offsetting from index 0), but how are rows represented? Or is a total byte-offset calculated from what is effectively row 0, column 0?

E: Never mind, just found `locator.rs`. Seems like we essentially treat the file as one giant string, then define rows as being delimited by `\n`/`\r` and then the columns as the offsets from _that_ offset?

---

_Label `breaking` added by @MichaReiser on 2023-04-14 18:04_

---

_Comment by @github-actions[bot] on 2023-04-14 19:04_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -16, 0 error(s))

<details><summary>airflow (+0, -7)</summary>
<p>

```diff
- airflow/api_connexion/endpoints/task_instance_endpoint.py:274:12: RET504 Unnecessary variable assignment before `return` statement
- airflow/providers/amazon/aws/secrets/systems_manager.py:200:16: RET504 Unnecessary variable assignment before `return` statement
- airflow/providers/docker/operators/docker.py:479:16: RET504 Unnecessary variable assignment before `return` statement
- airflow/providers/oracle/hooks/oracle.py:42:12: RET504 Unnecessary variable assignment before `return` statement
- airflow/security/utils.py:83:12: RET504 Unnecessary variable assignment before `return` statement
- airflow/www/extensions/init_appbuilder.py:359:16: RET504 Unnecessary variable assignment before `return` statement
- tests/test_utils/gcp_system_helpers.py:65:12: RET504 Unnecessary variable assignment before `return` statement
```

</p>
</details>
<details><summary>bokeh (+0, -1)</summary>
<p>

```diff
- src/bokeh/core/property/datetime.py:165:16: RET504 Unnecessary variable assignment before `return` statement
```

</p>
</details>
<details><summary>zulip (+0, -8)</summary>
<p>

```diff
- zerver/data_import/rocketchat.py:141:12: RET504 Unnecessary variable assignment before `return` statement
- zerver/lib/message.py:186:12: RET504 Unnecessary variable assignment before `return` statement
- zerver/lib/narrow.py:891:12: RET504 Unnecessary variable assignment before `return` statement
- zerver/lib/url_preview/oembed.py:50:12: RET504 Unnecessary variable assignment before `return` statement
- zerver/models.py:184:12: RET504 Unnecessary variable assignment before `return` statement
- zerver/webhooks/basecamp/view.py:115:12: RET504 Unnecessary variable assignment before `return` statement
- zerver/webhooks/bitbucket2/view.py:436:12: RET504 Unnecessary variable assignment before `return` statement
- zerver/webhooks/zendesk/view.py:14:12: RET504 Unnecessary variable assignment before `return` statement
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.06ms     2.7 MB/sec    1.00     14.9±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    380.3±1.31µs     7.8 MB/sec    1.00    378.7±1.49µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.01ms     5.3 MB/sec    1.00      7.6±0.03ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1596.6±2.95µs    10.4 MB/sec    1.00   1602.0±2.59µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.3±0.29µs    16.9 MB/sec    1.00    174.0±0.63µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.01      5.9±0.00ms     6.8 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1144.2±2.10µs    14.6 MB/sec    1.00   1138.6±2.20µs    14.6 MB/sec
parser/numpy/globals.py                    1.00    116.8±0.22µs    25.3 MB/sec    1.00    117.1±0.19µs    25.2 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     25.1±0.86ms  1657.1 KB/sec    1.00     24.3±0.86ms  1717.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      6.2±0.44ms     2.7 MB/sec    1.00      6.1±0.30ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   707.5±38.28µs     4.2 MB/sec    1.00   710.4±43.20µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.00     10.1±0.47ms     2.5 MB/sec    1.02     10.3±0.40ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     12.3±0.87ms     3.3 MB/sec    1.00     12.2±0.46ms     3.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.5±0.13ms     6.6 MB/sec    1.02      2.6±0.13ms     6.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   300.2±17.16µs     9.8 MB/sec    1.02   305.2±19.67µs     9.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.4±0.25ms     4.7 MB/sec    1.01      5.5±0.25ms     4.7 MB/sec
parser/large/dataset.py                    1.00      9.9±0.48ms     4.1 MB/sec    1.01      9.9±0.35ms     4.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1864.2±78.93µs     8.9 MB/sec    1.01  1881.3±62.66µs     8.9 MB/sec
parser/numpy/globals.py                    1.00    193.7±9.50µs    15.2 MB/sec    1.02   196.8±15.61µs    15.0 MB/sec
parser/pydantic/types.py                   1.00      4.1±0.17ms     6.2 MB/sec    1.01      4.1±0.18ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/actions.rs`:143 on 2023-04-14 22:30_

This returns the char offset instead if the byte offset. Use find instead 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/imports.rs`:34 on 2023-04-14 22:39_

Change to range?

---

_Review comment by @MichaReiser on `crates/ruff/src/directives.rs`:112 on 2023-04-15 07:14_

Delete?

---

_Review comment by @MichaReiser on `crates/ruff/src/doc_lines.rs`:85 on 2023-04-15 07:18_

Should this be the line range? What is it used for?

---

_Review comment by @MichaReiser on `crates/ruff/src/docstrings/definition.rs`:93 on 2023-04-15 07:19_

Use self.range

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:478 on 2023-04-15 07:35_

Always add a mapping that maps the eof to the last line?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/eradicate/rules.rs`:62 on 2023-04-15 07:37_

Use full_lines above and compute range from start + line.len

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/rules/assert_used.rs`:43 on 2023-04-15 07:38_

Use + or TextRange.at

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_commas/rules.rs`:299 on 2023-04-15 07:45_

The fix range should be comma.0..comma.2

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:123 on 2023-04-15 07:46_

range_replacement

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:193 on 2023-04-15 07:46_

range_replacement

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:1111 on 2023-04-15 07:48_

range_replacement

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_executable/helpers.rs`:19 on 2023-04-15 07:52_

Use TextRange?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pytest_style/rules/assertion.rs`:415 on 2023-04-15 08:01_

Use full_lines above and compute range here using TextRange.at

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:263 on 2023-04-15 08:02_

Use range

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pytest_style/rules/fixture.rs`:325 on 2023-04-15 08:02_

Pass range?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pytest_style/rules/marks.rs`:117 on 2023-04-15 08:04_

TextRange.sub_start?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_quotes/rules.rs`:260 on 2023-04-15 08:06_

Pass TextRange

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_quotes/rules.rs`:301 on 2023-04-15 08:07_

Pass TextRange

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_return/rules.rs`:323 on 2023-04-15 08:09_

This file could use some extra review. I doubt I understood the logic 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_return/visitor.rs`:17 on 2023-04-15 08:10_

Use TextRange

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_bool_op.rs`:498 on 2023-04-15 08:11_

Return TextRange?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_expr.rs`:195 on 2023-04-15 08:12_

use range_replacement

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:531 on 2023-04-15 08:13_

Use range_replacement

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:878 on 2023-04-15 08:13_

Use range_replacement

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/fix_if.rs`:42 on 2023-04-15 08:15_

Use full lines here and replace full_ranges below with TextRange.at

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/fix_with.rs`:24 on 2023-04-15 08:16_

Use full_lines and replace _lines_range with TextRange.at belowe

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/suppressible_exception.rs`:101 on 2023-04-15 08:18_

Use rangereplacement with TextRange.at

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/rules/organize_imports.rs`:103 on 2023-04-15 08:22_

Use full_line_end?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pandas_vet/fixes.rs`:25 on 2023-04-15 08:23_

Use TextRange

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:89 on 2023-04-15 08:24_

Pass range?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/errors.rs`:40 on 2023-04-15 08:28_

Use TextRange::empty

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:61 on 2023-04-15 08:29_

Pass TextRange 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:79 on 2023-04-15 08:29_

Do we need lines?

---

_@MichaReiser reviewed on 2023-04-15 09:35_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace.rs`:84 on 2023-04-15 09:47_

```suggestion
                    let mut diagnostic = Diagnostic::new(kind, TextRange::empty(token.start());
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:472 on 2023-04-15 09:47_

```suggestion
    pub fn range(&self) -> TextRange {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:567 on 2023-04-15 09:48_

```suggestion
    fn push_token(&mut self, kind: TokenKind, range: TextRange {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:673 on 2023-04-15 09:48_

```suggestion
    fn push(&mut self, kind: TokenKind, range: TextRange) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/backslashes.rs`:34 on 2023-04-15 11:59_

Docstring.range

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/ends_with_punctuation.rs`:61 on 2023-04-15 12:32_

Use `UniversalNewLines::with_offset(...).nth()` instead. `line.text_len()` does not include the trialing new lines.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/indent.rs`:110 on 2023-04-15 12:33_

Use `TextRange`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/newline_after_last_paragraph.rs`:43 on 2023-04-15 12:35_

```suggestion
                        Diagnostic::new(NewLineAfterLastParagraph, docstring.range());
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/one_liner.rs`:39 on 2023-04-15 12:36_

```suggestion
        let mut diagnostic = Diagnostic::new(FitsOnOneLine, docstring.range());
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:1 on 2023-04-15 12:39_

This file could use a good review 🤭 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/rules/f_string_missing_placeholders.rs`:74 on 2023-04-15 13:30_

```suggestion
                    TextRange::at(
                        location + f_position,
                        TextSize::from(f_position + 1),
                    ),
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/bad_string_format_type.rs`:247 on 2023-04-15 13:33_

```suggestion
    let mut strings: Vec<TextRange> = vec![];
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:177 on 2023-04-15 13:35_

```suggestion
    range: TextRange
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/manual_import_from.rs`:62 on 2023-04-15 13:36_

nit: `with_range`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/fixes.rs`:124 on 2023-04-15 13:38_

Nit use `TextRange`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/deprecated_mock_import.rs`:331 on 2023-04-15 13:39_

```suggestion
                                .map(|content| Edit::range_replacement(content, stmt.range()))
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/printf_string_formatting.rs`:311 on 2023-04-15 13:46_

```suggestion
    let mut strings: Vec<TextRange> = vec![];
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/replace_stdout_stderr.rs`:46 on 2023-04-15 13:47_

Use `TextRange`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/replace_universal_newlines.rs`:38 on 2023-04-15 13:48_

```suggestion
        let range = TextRange::at(
            kwarg.start(),
            "universal_newlines".text_len(),
        );
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:1688 on 2023-04-15 13:51_

```suggestion
    range: TextRange
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/imports.rs`:110 on 2023-04-15 14:08_

Use range

---

_@MichaReiser reviewed on 2023-04-15 14:11_

---

_Comment by @MichaReiser on 2023-04-15 21:05_

> Thanks for the detailed explanation @MichaReiser! If you don't mind - where do the offsets come from? Column offset makes sense (obviously just offsetting from index 0), but how are rows represented? Or is a total byte-offset calculated from what is effectively row 0, column 0?
> 
> E: Never mind, just found `locator.rs`. Seems like we essentially treat the file as one giant string, then define rows as being delimited by `\n`/`\r` and then the columns as the offsets from _that_ offset?

@evanrittenhouse, sorry for the late reply.

The RustPython Lexer generates the offsets. The old implementation counted the rows and columns (from the start of the row). The lexer increments the current row index and resets the column to zero for every new line character. 

Byte offsets don't use row or columns. Instead, it's an offset from the beginning of the file. Think of the string as a byte array and the byte offset is the index into that array:

```python
def f(): pass
x = 20
```

The position of the identifier `f`:

* row/column representation: `Location { row: 1, column 4 }`
* byte offsets: `TextSize::from(4)`

The position of the `=` sign

* row/column representation: `Location { row: 2, column: 2 }`
* byte offsets: `TextSize::from(16)`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__E111_E11.py.snap`:9 on 2023-04-17 12:40_

I replaced the empty `TextRange` with the range pointing to the incorrect indention. That's why the formatting and line numbers changed

---

_@MichaReiser reviewed on 2023-04-17 12:40_

---

_@MichaReiser reviewed on 2023-04-17 13:24_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__W191_W19.py.snap`:285 on 2023-04-17 13:24_

My understanding is that this was a false negative because `Indexer.strings` incorrectly suppressed this violation because it is on a line with a string. This now gets correctly reported because we test if the tab is inside of a string range (rather than on a line)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__E501_E501.py.snap`:17 on 2023-04-17 13:25_

The old implementation set the `column` to `limit` which is incorrect if the line contains characters that are two or more characters wide. 

---

_@MichaReiser reviewed on 2023-04-17 13:25_

---

_Marked ready for review by @MichaReiser on 2023-04-17 14:54_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-04-17 14:54_

---

_@MichaReiser reviewed on 2023-04-18 06:46_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__E111_E11.py.snap`:9 on 2023-04-18 06:46_

I backported the fix in https://github.com/charliermarsh/ruff/pull/4005

---

_@MichaReiser reviewed on 2023-04-18 06:47_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__E501_E501.py.snap`:17 on 2023-04-18 06:47_

I backported the fix in https://github.com/charliermarsh/ruff/pull/4006

---

_Label `breaking` removed by @MichaReiser on 2023-04-18 06:48_

---

_Label `internal` added by @MichaReiser on 2023-04-18 07:06_

---

_Comment by @MichaReiser on 2023-04-18 15:38_

The fixes to the `UnnecessaryAssign` rule look correct to me: 

https://github.com/apache/airflow/blob/bb5f63a971f29702294e88853130bb31292fe08b/airflow/www/extensions/init_appbuilder.py#L354-L359

https://github.com/apache/airflow/blob/bb5f63a971f29702294e88853130bb31292fe08b/airflow/security/utils.py#L78-L83

https://github.com/apache/airflow/blob/bb5f63a971f29702294e88853130bb31292fe08b/airflow/api_connexion/endpoints/task_instance_endpoint.py#L271-L274

---

_@MichaReiser reviewed on 2023-04-18 15:48_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_return/rules.rs`:360 on 2023-04-18 15:48_

Future if this turns out to be slow: Use a binary search to find the right range.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/stylist.rs`:210 on 2023-04-19 02:55_

(Looks like an unrelated refactor, just confirming no change in behavior here.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/mod.rs`:5 on 2023-04-19 03:02_

Should we change all of these (or revert this)?

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/edit.rs`:40 on 2023-04-19 03:03_

What's the benefit to having both the `TextSize` and `range_*` variants?

---

_Comment by @youknowone on 2023-04-19 07:17_

maybe you are interested in this change in lalrpop
https://github.com/lalrpop/lalrpop/pull/743

---

_Comment by @MichaReiser on 2023-04-19 07:28_

> maybe you are interested in this change in lalrpop [lalrpop/lalrpop#743](https://github.com/lalrpop/lalrpop/pull/743)

Nice! Improvements to the parser are extremely impactful for ruff because it's where it spends the majority of the time if only few rules are enabled.

---

_Comment by @youknowone on 2023-04-19 07:29_

ah, i am sorry, I didn't check if it is a parse speed up or lalrpop compliation speed up.

---

_Comment by @MichaReiser on 2023-04-19 07:32_

> ah, i am sorry, I didn't check if it is a parse speed up or lalrpop compliation speed up.

We happily take any improvements to the compile time :) 

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:2 on 2023-04-19 21:08_

Very tempted to run an import organizer over our codebase, must resist 😅 

---

_Review comment by @charliermarsh on `crates/ruff/src/doc_lines.rs`:40 on 2023-04-19 21:10_

Why is this condition no longer necessary?

---

_Review comment by @charliermarsh on `crates/ruff/src/docstrings/definition.rs`:31 on 2023-04-19 21:10_

Nit: incomplete comment here

---

_Review comment by @charliermarsh on `crates/ruff/src/docstrings/sections.rs`:271 on 2023-04-19 21:11_

Nit: two spaces between "name" and "of"

---

_Review comment by @charliermarsh on `crates/ruff/src/docstrings/sections.rs`:295 on 2023-04-19 21:12_

Nit: new line => newline

---

_Review comment by @charliermarsh on `crates/ruff/src/docstrings/sections.rs`:385 on 2023-04-19 21:14_

I guess this is really `section_name_range` or something? Had to go to the call site to understand why the logic here was the same as before.

---

_Review comment by @charliermarsh on `crates/ruff/src/logging.rs`:161 on 2023-04-19 21:17_

(Lots of good little changes like this falling out from the PR.)

---

_Review comment by @charliermarsh on `crates/ruff/src/message/mod.rs`:60 on 2023-04-19 21:18_

Is this expensive?

---

_Review comment by @charliermarsh on `crates/ruff_benchmark/Cargo.toml`:43 on 2023-04-19 21:19_

Nit: revert? Looks like an accident.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/indexer.rs`:14 on 2023-04-19 21:22_

Nit: probably worth a comment for consistency.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/indexer.rs`:127 on 2023-04-19 21:26_

Was this intentional? It's hard to read from the diff, but it may now be invalid Python because the `.trim()` only affects the leading and trailing space, but the `x = 1` is now indented.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/indexer.rs`:11 on 2023-04-19 21:26_

Nit: `comment_ranges`, maybe, for consistency with `string_ranges`? Since it's changing anyway.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/indexer.rs`:43 on 2023-04-19 21:28_

Can you explain or comment on what's happening here? We're relying on both the trivia contents, and the token type?

---

_Review comment by @charliermarsh on `scripts/Dockerfile.ecosystem`:23 on 2023-04-19 21:30_

I think these need to be reverted, they assume `ruff-main` etc.?

---

_Review comment by @charliermarsh on `playground/src/Editor/SourceEditor.tsx`:40 on 2023-04-19 22:51_

Just want to confirm that this is intended, despite the JSON API change being moved into a separate PR.

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:449 on 2023-04-19 22:57_

So in practice, this is effectively the list of all multi-line string ranges -- and we check to see if an offset is in one of those ranges, then return the end of the range. Is that right?

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:186 on 2023-04-19 22:59_

There's very little automated test coverage for this unfortunately -- maybe none. So you probably want to run this on a few files just to spot-check that there are no obvious errors. `crates/ruff/resources/test/fixtures/ruff/RUF100_1.py` is an example file that you might want to test. You could also take a codebase, run Ruff, then run with `--add-noqa`, and verify that running Ruff again doesn't yield any violations.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4220 on 2023-04-19 23:01_

Nice, this is a good measure.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__W191_W19.py.snap`:285 on 2023-04-19 23:02_

I think you're right.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__E999_E999.py.snap`:8 on 2023-04-19 23:02_

Is this intended?

---

_@charliermarsh approved on 2023-04-19 23:09_

Ok, I made it through 115 / 418 files, but skipped most of the `rules`. Overall, I think I have a good sense for what's going on here: the motivation, the scope of the changes, the implementation, and the impact -- enough so that I'm definitely comfortable approving.

It feels like an understatement to call this work "impressive". It's ridiculously good -- a significant improvement across the codebase, and one that required navigating nearly every part of Ruff (not only every file, but every concept and abstraction).

(I left a few nits and questions, but as always nits are at your discretion, and I trust you to address + merge as you see fit.)


---

_Comment by @charliermarsh on 2023-04-19 23:13_

A couple questions on benchmarks and performance (which don't map to specific lines of code):

1. "Performance regresses for loading cached results." Am I correct that the regression here is, e.g., between the first two rows of the table? From 37.9ms to 44.4ms? And then the fifth and six lines, from 395.6ms to 401.6ms? So it's like a ~fixed 6ms regression? Seems totally fine, but confirming my understanding.
2. I am a little surprised that we see an improvement in the cpython default ruleset benchmark (without caching) -- lines 3 and 4. We have to build the line index for every file that contains at least one violation, right? So aren't we building the line index _more often_ now than we were before in that case?

---

_@MichaReiser reviewed on 2023-04-21 00:58_

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:449 on 2023-04-21 00:58_

That's correct. Proved to be sufficient for our need. I initially stored a `(TextRange, TextSize)` but then realized that I always map the range to the end position... So I removed the `TextSize`.

---

_@MichaReiser reviewed on 2023-04-21 01:01_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/mod.rs`:60 on 2023-04-21 01:01_

It can be, that's why I called it `compute` to motivate callers to hold on to it, rather than calling `start_location` over and over again. It's somewhat expensive because:

* It needs to compute the `LineIndex` if it the `index` hasn't yet been computed for this file.
* It performs a binary search to find the index fo the given offset. This isn't expensive, but it's still better to hold on to the location rather than performing the binary search over and over again :)

---

_@MichaReiser reviewed on 2023-04-21 01:02_

---

_Review comment by @MichaReiser on `playground/src/Editor/SourceEditor.tsx`:40 on 2023-04-21 01:02_

Yes, this is intended. The playground uses the WASM API that is independent of the JSON serializer and it now uses `SourceLocation` (one indexed)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/stylist.rs`:210 on 2023-04-23 13:50_

Yes, this should still do the same.

---

_@MichaReiser reviewed on 2023-04-23 13:51_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/mod.rs`:5 on 2023-04-23 13:51_

I think we should change all, but I rather prefer not to do this as part of this PR. I'll undo

---

_@MichaReiser reviewed on 2023-04-23 13:53_

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/edit.rs`:40 on 2023-04-23 13:53_

I first wanted to only have `TextRange` variants because they are very convenient to create an edit that e.g. replaces a node: `Edit::range_replacement(new_text, stmt.range())`. 

But we also have many edits where we compute the range and adding `TextRange::new(...)` in the call sites made everything too verbose for my taste.

```rust
Edit::range("text", TextRange::new(start_offset + relative_offset, end_offset - trailing_end)) # vs
Edit::range("text", start_offset + relative_offset, end_offset - trailing_end)
```

But not using `TextRange` has the disadvantage that it's less clear what the meaning of the second and third argument is. What do you prefer (I would recommend it as a follow up refactor to avoid increasing this PR further)?

---

_Review comment by @MichaReiser on `crates/ruff/src/codes.rs`:2 on 2023-04-23 13:56_

Please wait till after I merged this PR 😅

---

_@MichaReiser reviewed on 2023-04-23 13:56_

---

_Review comment by @MichaReiser on `crates/ruff/src/doc_lines.rs`:40 on 2023-04-23 14:13_

Good point. I should probably undo the check. I probably removed it to see if any test fails and none does but this is only because we have no test case for `x = 1\n#comment here`. 

---

_@MichaReiser reviewed on 2023-04-23 14:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/indexer.rs`:43 on 2023-04-23 17:08_

I commented the code. We may want to change `RustPython` to emit all newline tokens. 

---

_@MichaReiser reviewed on 2023-04-23 17:11_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__E999_E999.py.snap`:8 on 2023-04-23 17:11_

I can revert it and move it to another PR, but I thought having a caret rendered under the syntax error is nice. 

Why  I added it is because I had to introduce the `DisplaySyntaxError` struct to render `SyntaxErrors` with row/column information. I also changed `RustPython` to set the `location` for syntax errors to the start of the token causing the error (rather than past it???)

---

_Comment by @MichaReiser on 2023-04-23 17:14_

> A couple questions on benchmarks and performance (which don't map to specific lines of code):
> 
>     1. "Performance regresses for loading cached results." Am I correct that the regression here is, e.g., between the first two rows of the table? From 37.9ms to 44.4ms? And then the fifth and six lines, from 395.6ms to 401.6ms? So it's like a ~fixed 6ms regression? Seems totally fine, but confirming my understanding.

That's correct. Interesting that it is 6ms in both cases

> 
>     2. I am a little surprised that we see an improvement in the cpython default ruleset benchmark (without caching) -- lines 3 and 4. We have to build the line index for every file that contains at least one violation, right? So aren't we building the line index _more often_ now than we were before in that case?

It's correct that we build the `LineIndex` more often. But it seems that the performance improvements to the parser and AST traversal outweigh the cost for building the additional `LineIndex`s, so that, overall, it still results in a perf win. 



---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:186 on 2023-04-23 18:00_

Good call! It was completely broken... I tested it with airflow and running check after adding the `noqa` directives yields the same violations as on main (it seems we miss some noqa directives)

---

_@MichaReiser reviewed on 2023-04-23 18:00_

---

_Merged by @MichaReiser on 2023-04-26 18:11_

---

_Closed by @MichaReiser on 2023-04-26 18:11_

---

_Branch deleted on 2023-04-26 18:11_

---
