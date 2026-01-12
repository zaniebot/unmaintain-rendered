```yaml
number: 4539
title: malachite_bigint
type: pull_request
state: closed
author: youknowone
labels: []
assignees: []
draft: true
base: main
head: malachite-bigint
created_at: 2023-05-20T06:59:04Z
updated_at: 2023-05-25T12:33:32Z
url: https://github.com/astral-sh/ruff/pull/4539
synced_at: 2026-01-12T03:50:03Z
```

# malachite_bigint

---

_Pull request opened by @youknowone on 2023-05-20 06:59_

test of https://github.com/RustPython/Parser/pull/18

---

_Comment by @github-actions[bot] on 2023-05-20 07:12_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.11ms     2.9 MB/sec    1.00     13.9±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    423.2±1.16µs     7.0 MB/sec    1.00    420.7±1.41µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1499.8±3.63µs    11.1 MB/sec    1.00   1493.1±4.31µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.2±0.23µs    17.4 MB/sec    1.00    169.8±0.27µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.8 MB/sec    1.01      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1021.6±5.07µs    16.3 MB/sec    1.01   1031.0±0.59µs    16.2 MB/sec
parser/numpy/globals.py                    1.00    104.6±0.46µs    28.2 MB/sec    1.00    104.9±0.27µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.3±0.01ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     21.6±0.41ms  1928.8 KB/sec    1.00     21.1±0.56ms  1978.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.14ms     3.1 MB/sec    1.03      5.5±0.22ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   640.7±36.57µs     4.6 MB/sec    1.02   653.9±25.51µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.31ms     2.8 MB/sec    1.00      9.1±0.32ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.3±0.45ms     3.9 MB/sec    1.00     10.3±0.60ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.08ms     7.9 MB/sec    1.00      2.1±0.11ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.01   257.5±14.02µs    11.5 MB/sec    1.00   253.8±10.50µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.16ms     5.7 MB/sec    1.00      4.5±0.23ms     5.7 MB/sec
parser/large/dataset.py                    1.00      8.2±0.33ms     5.0 MB/sec    1.01      8.3±0.29ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1546.5±46.54µs    10.8 MB/sec    1.02  1571.3±27.83µs    10.6 MB/sec
parser/numpy/globals.py                    1.01    158.7±7.49µs    18.6 MB/sec    1.00    157.5±4.74µs    18.7 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.10ms     7.5 MB/sec    1.03      3.5±0.07ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @youknowone on 2023-05-20 08:12_

@MichaReiser looking good?

---

_Review comment by @MichaReiser on `Cargo.lock`:201 on 2023-05-20 10:06_

Hmm. This is adding a lot new dependencies. 

---

_@MichaReiser reviewed on 2023-05-20 10:08_

We need to upgrade Ruff to the latest RustPython version first to measure the bigint change in isolation (e.g does the diff include the statement parsing changes?)

Another way is to run the benchmarks locally. Once with RustPython pointing to main and once with the new changes 

---

_Comment by @MichaReiser on 2023-05-24 17:23_

I ran the benchmarks locally and can't see a significant change in performance. This doesn't mean that performance isn't improving, we just don't see any improvements for our benchmarks. For example, it could just be that our sample files use too few ints for it to be significant. 

<pre>group                                      main                                   malachite
-----                                      ----                                   ---------
linter/all-rules/large/dataset.py          1.01      9.0±0.04ms     4.5 MB/sec<span style="color:#26A269"><b>    1.00      8.9±0.19ms     4.6 MB/sec</b></span>
linter/all-rules/numpy/ctypeslib.py        1.01      2.1±0.02ms     8.0 MB/sec<span style="color:#26A269"><b>    1.00      2.1±0.03ms     8.1 MB/sec</b></span>
linter/all-rules/numpy/globals.py          1.01    226.1±0.82µs    13.1 MB/sec<span style="color:#26A269"><b>    1.00    224.0±0.47µs    13.2 MB/sec</b></span>
linter/all-rules/pydantic/types.py<span style="color:#26A269"><b>         1.00      3.6±0.02ms     7.1 MB/sec</b></span>    1.00      3.6±0.01ms     7.1 MB/sec
linter/default-rules/large/dataset.py      1.00      4.5±0.01ms     9.1 MB/sec<span style="color:#26A269"><b>    1.00      4.5±0.01ms     9.1 MB/sec</b></span>
linter/default-rules/numpy/ctypeslib.py    1.01    924.8±2.10µs    18.0 MB/sec<span style="color:#26A269"><b>    1.00    913.7±4.23µs    18.2 MB/sec</b></span>
linter/default-rules/numpy/globals.py<span style="color:#26A269"><b>      1.00     98.3±0.49µs    30.0 MB/sec</b></span>    1.01     99.1±0.25µs    29.8 MB/sec
linter/default-rules/pydantic/types.py<span style="color:#26A269"><b>     1.00   1930.8±4.73µs    13.2 MB/sec</b></span>    1.01  1951.5±13.11µs    13.1 MB/sec
parser/large/dataset.py<span style="color:#26A269"><b>                    1.00      3.4±0.09ms    12.0 MB/sec</b></span>    1.01      3.4±0.01ms    11.8 MB/sec
parser/numpy/ctypeslib.py                  1.00    664.9±2.10µs    25.0 MB/sec<span style="color:#26A269"><b>    1.00    661.9±1.81µs    25.2 MB/sec</b></span>
parser/numpy/globals.py<span style="color:#26A269"><b>                    1.00     64.3±1.24µs    45.9 MB/sec</b></span>    1.03     65.9±2.33µs    44.7 MB/sec
parser/pydantic/types.py                   1.01   1437.8±4.56µs    17.7 MB/sec<span style="color:#26A269"><b>    1.00  1424.7±12.89µs    17.9 MB/sec</b></span>
</pre>

I'm going to close this PR for now. You can run the benchmarks locally with

```
# Run baseline (once)
cargo benchmark --save-baseline main

# Make changes
cargo benchmark --save-baseline malachite

# compare baseline
critcmp main malachite
```

---

_Closed by @MichaReiser on 2023-05-24 17:23_

---

_Comment by @youknowone on 2023-05-24 17:50_

The malachite patch may not affect Ruff that much. The underlying data structure is very similar. Operation is faster, but I don't think Ruff has int operation that much.

---

_Comment by @MichaReiser on 2023-05-24 18:32_

> The malachite patch may not affect Ruff that much. The underlying data structure is very similar. Operation is faster, but I don't think Ruff has int operation that much.

My understanding is that it avoids allocation for small numbers. Shouldn't that improve parse time (but again, probably not significant enough).

---

_Comment by @youknowone on 2023-05-25 11:13_

Oh, yes, that's also right.

In future, I expect parser returns `&str` as unparsed int string and actual BigInt build can be deferred when it actually needed to be created. Does it make sense?

---

_Comment by @MichaReiser on 2023-05-25 12:33_

That makes sense. That would also simplify the lexer

---
