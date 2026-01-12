```yaml
number: 5009
title: Add isort line-length independent config
type: pull_request
state: closed
author: silvanocerza
labels: []
assignees: []
base: main
head: isort-line-length
created_at: 2023-06-10T17:08:05Z
updated_at: 2023-07-21T03:57:20Z
url: https://github.com/astral-sh/ruff/pull/5009
synced_at: 2026-01-12T15:55:17Z
```

# Add isort line-length independent config

---

_@silvanocerza_

## Summary

Fixes #3206.

This PR adds a `line-length` config for `isort`, this way users will be able to specify a different line length that differs from default Ruff config.

I handled three different cases:

* `ruff.isort.line-length` is not set: use `tool.ruff.line-length`
* `ruff.isort.line-length` is smaller than `tool.ruff.line-length`: use `tool.ruff.line-length`
* `ruff.isort.line-length` is greater than `tool.ruff.line-length`: use `tool.ruff.isort.line-length`


## Test Plan

I tested it by adding the `tool.ruff.isort.line-length` config in `crates/ruff/resources/test/fixtures/isort/pyproject.toml` and running the following command:

```
cargo run -p ruff_cli -- check crates/ruff/resources/test/fixtures/isort/fit_line_length.py --no-cache
```

I decided to leave the `tool.ruff.line-length` in `pyproject.toml` to verify that it's ignored if it's greater than the `isort` one.

I'd like to add an automatic test in case `tool.ruff.line-length` is less than `tool.ruff.isort.line-length` but I'm not sure how to proceed on this. If anyone could point to similar tests I'll gladly try to add a new one.

---

_Comment by @github-actions[bot] on 2023-06-10 17:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.2±0.33ms     5.6 MB/sec    1.07      7.7±0.24ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1535.3±67.73µs    10.8 MB/sec    1.05  1613.0±53.98µs    10.3 MB/sec
formatter/numpy/globals.py                 1.05   159.8±11.86µs    18.5 MB/sec    1.00    151.4±8.91µs    19.5 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.11ms     8.1 MB/sec    1.00      3.2±0.27ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.43ms     2.5 MB/sec    1.01     16.6±0.69ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.25ms     4.2 MB/sec    1.03      4.1±0.16ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   520.7±26.81µs     5.7 MB/sec    1.02   529.7±30.66µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.26ms     3.7 MB/sec    1.05      7.2±0.24ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      7.9±0.34ms     5.1 MB/sec    1.00      7.7±0.21ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1655.3±59.68µs    10.1 MB/sec    1.02  1682.8±76.41µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.2±7.43µs    15.1 MB/sec    1.00   195.0±12.19µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.14ms     7.2 MB/sec    1.00      3.5±0.12ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.6±0.30ms     4.7 MB/sec    1.23     10.5±0.44ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1788.2±62.19µs     9.3 MB/sec    1.24      2.2±0.10ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   176.3±11.39µs    16.7 MB/sec    1.13   199.4±12.60µs    14.8 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.16ms     7.1 MB/sec    1.23      4.4±0.23ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     18.8±0.74ms     2.2 MB/sec    1.03     19.4±0.92ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.8±0.19ms     3.5 MB/sec    1.00      4.8±0.22ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   574.0±23.39µs     5.1 MB/sec    1.05   601.8±43.57µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.41ms     3.2 MB/sec    1.00      8.0±0.41ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.35ms     4.4 MB/sec    1.02      9.4±0.29ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1988.7±85.11µs     8.4 MB/sec    1.05      2.1±0.10ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    231.5±9.78µs    12.7 MB/sec    1.00   230.3±10.92µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.16ms     6.0 MB/sec    1.00      4.2±0.19ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-12 16:18_

Thanks! The code looks reasonable, but what I'm mainly unsure of is how we should handle the case in which the import length is greater than the default length. In your implementation, it falls back to the shorter of the two, which is reasonable given that it'll be much harder to "properly" support varying line lengths... But it's also a confusing user behavior. I'm going to ask in the relevant issue if anyone has such a use case.

---

_Comment by @charliermarsh on 2023-07-21 03:57_

I think we need to close this out until we have a way to support enforcing separate line lengths on imports (e.g., so that we can support _larger_ line length maximums for imports).

---

_Closed by @charliermarsh on 2023-07-21 03:57_

---
