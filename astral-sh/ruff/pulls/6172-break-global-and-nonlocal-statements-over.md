```yaml
number: 6172
title: Break global and nonlocal statements over continuation lines
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/global-nonlocal-break
created_at: 2023-07-29T13:03:44Z
updated_at: 2023-08-02T20:12:47Z
url: https://github.com/astral-sh/ruff/pull/6172
synced_at: 2026-01-12T15:55:20Z
```

# Break global and nonlocal statements over continuation lines

---

_@charliermarsh_

## Summary

Builds on #6170 to break `global` and `nonlocal` statements, such that we get:

```python
def f():
    global \
        analyze_featuremap_layer, \
        analyze_featuremapcompression_layer, \
        analyze_latencies_post, \
        analyze_motions_layer, \
        analyze_size_model
```

Instead of:

```python
def f():
    global analyze_featuremap_layer, analyze_featuremapcompression_layer, analyze_latencies_post, analyze_motions_layer, analyze_size_model
```

Notably, we avoid applying this formatting if the statement ends in a comment. Otherwise, the comment would _need_ to be placed after the last item, like:

```python
def f():
    global \
        analyze_featuremap_layer, \
        analyze_featuremapcompression_layer, \
        analyze_latencies_post, \
        analyze_motions_layer, \
        analyze_size_model  # noqa
```

To me, this seems wrong (and would break the `# noqa` comment). Ideally, the items would be parenthesized, and the comment would be on the inner parenthesis, like:

```python
def f():
    global (  # noqa
        analyze_featuremap_layer,
        analyze_featuremapcompression_layer,
        analyze_latencies_post,
        analyze_motions_layer,
        analyze_size_model
    )
```

But that's not valid syntax.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-29 13:03_

---

_Review requested from @konstin by @charliermarsh on 2023-07-29 13:03_

---

_Label `formatter` added by @charliermarsh on 2023-07-29 13:05_

---

_Comment by @github-actions[bot] on 2023-07-29 13:36_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.05ms     4.9 MB/sec    1.02      8.4±0.18ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1656.7±47.79µs    10.1 MB/sec    1.00  1652.9±28.64µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    181.2±2.85µs    16.3 MB/sec    1.01    182.3±2.01µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.07ms     7.2 MB/sec    1.01      3.6±0.08ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.01     11.0±0.12ms     3.7 MB/sec    1.00     10.9±0.13ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.00ms     6.0 MB/sec    1.00      2.8±0.00ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    380.2±0.70µs     7.8 MB/sec    1.00    381.0±1.54µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.05ms     5.2 MB/sec    1.03      5.1±0.14ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.01      5.8±0.06ms     7.0 MB/sec    1.00      5.7±0.05ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1202.8±7.03µs    13.8 MB/sec    1.00   1195.7±7.66µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    133.8±0.31µs    22.0 MB/sec    1.00    132.9±0.36µs    22.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.6±0.03ms    10.0 MB/sec    1.00      2.5±0.05ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.16ms     4.1 MB/sec    1.01     10.0±0.16ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1922.9±41.65µs     8.7 MB/sec    1.00  1926.6±43.58µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    199.1±4.77µs    14.8 MB/sec    1.03   204.8±14.59µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.13ms     6.0 MB/sec    1.01      4.3±0.11ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.31ms     3.0 MB/sec    1.00     13.4±0.18ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.06ms     4.6 MB/sec    1.01      3.6±0.11ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   377.3±14.16µs     7.8 MB/sec    1.00   372.8±11.62µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.10ms     4.2 MB/sec    1.00      6.1±0.12ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.10ms     5.5 MB/sec    1.00      7.4±0.11ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1490.3±36.17µs    11.2 MB/sec    1.00  1481.1±27.52µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    150.2±6.47µs    19.6 MB/sec    1.00    147.3±3.95µs    20.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     7.9 MB/sec    1.00      3.2±0.05ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-31 10:47_

Looks good. It may now be worth factoring the shared logic into a shared `Format*` utility where you pass the `keyword` text and the `names`.

---

_Comment by @konstin on 2023-07-31 13:04_

We should lobby for
```python
def f():
    global (  # noqa
        analyze_featuremap_layer,
        analyze_featuremapcompression_layer,
        analyze_latencies_post,
        analyze_motions_layer,
        analyze_size_model
    )
```
to become valid python syntax. It's inconsistent that some nodes aren't allowed parentheses, while expressions might be wrapped in an arbitrary number of them. 

> To me, this seems wrong (and would break the # noqa comment).

This is currently a pervasive problem in the formatter. I'm not sure if special casing here is worth it.

---

_@konstin approved on 2023-07-31 13:04_

---

_Merged by @charliermarsh on 2023-08-02 19:55_

---

_Closed by @charliermarsh on 2023-08-02 19:55_

---

_Branch deleted on 2023-08-02 19:55_

---
