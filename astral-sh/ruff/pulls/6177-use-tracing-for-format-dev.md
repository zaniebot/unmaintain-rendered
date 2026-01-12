```yaml
number: 6177
title: Use tracing for format_dev
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: format_dev_on_tracing
created_at: 2023-07-30T12:43:06Z
updated_at: 2023-07-31T19:26:11Z
url: https://github.com/astral-sh/ruff/pull/6177
synced_at: 2026-01-12T02:58:30Z
```

# Use tracing for format_dev

---

_Pull request opened by @konstin on 2023-07-30 12:43_

## Summary

[tracing](https://github.com/tokio-rs/tracing) is library for logging, tracing and related features that has a large ecosystem. Using [tracing-subscriber](https://docs.rs/tracing-subscriber) and [tracing-indicatif](https://github.com/emersonford/tracing-indicatif), we get a nice logging output that you can configure with `RUST_LOG` (e.g. `RUST_LOG=debug`) and a live look into the formatter progress.

Default:
![Screenshot from 2023-07-30 13-59-53](https://github.com/astral-sh/ruff/assets/6826232/6432f835-9ff1-4771-955b-398e54c406dc)

`RUST_LOG=debug`:
![Screenshot from 2023-07-30 14-01-32](https://github.com/astral-sh/ruff/assets/6826232/5f2c87da-0867-4159-82e7-b5757eebb8eb)

It's easy to see in this output which files take a disproportionate amount of time.

[Peek 2023-07-30 14-35.webm](https://github.com/astral-sh/ruff/assets/6826232/2c92db5c-1354-465b-a6bc-ddfb281d6f9d)

It opens up further integration with the tracing ecosystem, [tracing-timing](https://docs.rs/tracing-timing/latest/tracing_timing/) and [tokio-console](https://github.com/tokio-rs/console) can e.g. show histograms and the json output allows us building better pipelines than grepping a log file.

One caveat is using `parent: None` for the logging statements because tracing subscriber does not allow deactivating the span without reimplementing all the other log message formatting, too, and we don't need span information, esp. since it would currently show the progress bar span.

## Test Plan

n/a


---

_Label `internal` added by @konstin on 2023-07-30 12:43_

---

_Label `formatter` added by @konstin on 2023-07-30 12:43_

---

_@konstin reviewed on 2023-07-30 12:43_

---

_Review comment by @konstin on `crates/ruff_dev/src/format_dev.rs`:293 on 2023-07-30 12:43_

was experimenting with tracing here

---

_@konstin reviewed on 2023-07-30 12:44_

---

_Review comment by @konstin on `crates/ruff_dev/src/format_dev.rs`:250 on 2023-07-30 12:44_

a nice side effect is that tracing-indicatif makes sure the progress bars and the logging output don't collide

---

_Review comment by @konstin on `crates/ruff_dev/src/format_dev.rs`:289 on 2023-07-30 12:44_

another positive side effect: no more manual stdout/stdout

---

_@konstin reviewed on 2023-07-30 12:44_

---

_Comment by @github-actions[bot] on 2023-07-30 12:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.8±0.47ms     3.8 MB/sec    1.00     10.4±0.38ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.1±0.08ms     7.9 MB/sec    1.00      2.0±0.08ms     8.2 MB/sec
formatter/numpy/globals.py                 1.06   237.0±11.30µs    12.4 MB/sec    1.00   223.5±11.22µs    13.2 MB/sec
formatter/pydantic/types.py                1.08      4.6±0.09ms     5.6 MB/sec    1.00      4.2±0.19ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.25ms     2.7 MB/sec    1.02     15.2±1.67ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.13ms     4.5 MB/sec    1.00      3.7±0.07ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   515.6±15.17µs     5.7 MB/sec    1.00   508.7±11.24µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.7±0.12ms     3.8 MB/sec    1.00      6.6±0.17ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.02      8.1±0.16ms     5.0 MB/sec    1.00      7.9±0.52ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1651.5±46.26µs    10.1 MB/sec    1.00  1635.2±55.72µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.5±4.55µs    15.7 MB/sec    1.03    193.5±9.36µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.6±0.16ms     7.1 MB/sec    1.00      3.5±0.09ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.21ms     3.9 MB/sec    1.00     10.5±0.23ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.0±0.06ms     8.3 MB/sec    1.00  1974.1±45.42µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    216.8±7.47µs    13.6 MB/sec    1.01    218.6±8.33µs    13.5 MB/sec
formatter/pydantic/types.py                1.02      4.4±0.15ms     5.7 MB/sec    1.00      4.4±0.13ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     14.2±0.27ms     2.9 MB/sec    1.00     14.1±0.22ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.07ms     4.6 MB/sec    1.02      3.7±0.14ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   445.2±13.62µs     6.6 MB/sec    1.02   452.7±15.96µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.15ms     4.0 MB/sec    1.04      6.6±0.30ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.8±0.11ms     5.2 MB/sec    1.00      7.7±0.10ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1556.9±20.86µs    10.7 MB/sec    1.01  1565.9±32.01µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.5±4.24µs    16.9 MB/sec    1.00    174.9±5.01µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.06ms     7.6 MB/sec    1.01      3.4±0.11ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_dev/Cargo.toml`:44 on 2023-07-31 08:52_

Can we remove `log` or is it used by other dev tools?

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/format_dev.rs`:202 on 2023-07-31 08:52_

Nit: Maybe move out to its own function to not clutter `main` as much. 

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/format_dev.rs`:341 on 2023-07-31 08:55_

Nit: You could wrap line 341..398 in a block, this way you don't need the explicit drop. 

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/format_dev.rs`:276 on 2023-07-31 08:56_

Why are we passing all these arguments here? Do we want to filter by these? 

---

_@MichaReiser approved on 2023-07-31 08:56_

Love it. We should use tracing in all of ruff ;)

We used the tracing timing in rome to understand formatting/linting times better (down to per rule). That was very useful

---

_@konstin reviewed on 2023-07-31 17:42_

---

_Review comment by @konstin on `crates/ruff_dev/Cargo.toml`:44 on 2023-07-31 17:42_

yes!

---

_@konstin reviewed on 2023-07-31 17:42_

---

_Review comment by @konstin on `crates/ruff_dev/src/format_dev.rs`:276 on 2023-07-31 17:42_

i was experimenting with tracing bars here, i would re-add them if we get a consumer for them

---

_Comment by @konstin on 2023-07-31 18:53_

Current dependencies on/for this PR:
* main
  * **PR #6177** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6177" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6177?utm_source=stack-comment).

---

_Merged by @konstin on 2023-07-31 19:14_

---

_Closed by @konstin on 2023-07-31 19:14_

---

_Branch deleted on 2023-07-31 19:14_

---
