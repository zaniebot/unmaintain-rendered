```yaml
number: 5750
title: "Allow specification of `logging.Logger` re-exports via `logger-objects`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: charlie/log
created_at: 2023-07-13T23:03:00Z
updated_at: 2023-07-24T04:52:04Z
url: https://github.com/astral-sh/ruff/pull/5750
synced_at: 2026-01-12T03:30:21Z
```

# Allow specification of `logging.Logger` re-exports via `logger-objects`

---

_Pull request opened by @charliermarsh on 2023-07-13 23:03_

## Summary

This PR adds a `logger-objects` setting that allows users to mark specific symbols a `logging.Logger` objects. Currently, if a `logger` is imported, we only flagged it as a `logging.Logger` if it comes exactly from the `logging` module or is `flask.current_app.logger`.

This PR allows users to mark specific loggers, like `logging_setup.logger`, to ensure that they're covered by the `flake8-logging-format` rules and others.

For example, if you have a module `logging_setup.py` with the following
contents:

```python
import logging

logger = logging.getLogger(__name__)
```

Adding `"logging_setup.logger"` to `logger-objects` will ensure that
`logging_setup.logger` is treated as a `logging.Logger` object when
imported from other modules (e.g., `from logging_setup import logger`).

Closes https://github.com/astral-sh/ruff/issues/5694.

---

_Label `bug` added by @charliermarsh on 2023-07-13 23:03_

---

_Comment by @github-actions[bot] on 2023-07-13 23:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.04ms     4.4 MB/sec    1.00      9.3±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1861.3±1.53µs     8.9 MB/sec    1.00   1861.6±2.16µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    210.0±2.49µs    14.0 MB/sec    1.00    210.2±0.24µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.01     13.0±0.06ms     3.1 MB/sec    1.00     13.0±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.02ms     5.0 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    431.3±1.33µs     6.8 MB/sec    1.00    428.5±0.61µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.03ms     6.2 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1411.6±1.80µs    11.8 MB/sec    1.00   1401.5±3.13µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.9±0.37µs    18.8 MB/sec    1.00    155.4±0.83µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.4±0.30ms     3.0 MB/sec    1.01     13.6±0.36ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.08ms     6.3 MB/sec    1.00      2.7±0.08ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00    295.3±9.64µs    10.0 MB/sec    1.04   308.5±18.08µs     9.6 MB/sec
formatter/pydantic/types.py                1.00      5.8±0.13ms     4.4 MB/sec    1.01      5.8±0.16ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.3±0.37ms     2.1 MB/sec    1.02     19.7±0.34ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.10ms     3.3 MB/sec    1.02      5.1±0.18ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   601.7±19.88µs     4.9 MB/sec    1.01   609.1±19.40µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.29ms     2.8 MB/sec    1.01      9.1±0.37ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.21ms     4.1 MB/sec    1.01     10.0±0.35ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.05ms     8.1 MB/sec    1.01      2.1±0.05ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    235.3±9.37µs    12.5 MB/sec    1.03   242.8±11.26µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.16ms     5.8 MB/sec    1.02      4.5±0.11ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-13 23:16_

For what it's worth, these are false positives in cibuildwheel because they use a custom logger without deferred formatting https://github.com/pypa/cibuildwheel/blob/66b46d086804a9e9782354100d96a3a445431bca/cibuildwheel/logger.py

---

_Comment by @charliermarsh on 2023-07-13 23:16_

Sadly those errors in cibuildwheel are false positives. They have a custom logger that matches our heuristic.

---

_Comment by @zanieb on 2023-07-13 23:17_

It does seem weird / unlikely to have this rule enabled at all if you are using a custom logger though

---

_@MichaReiser approved on 2023-07-14 06:10_

---

_Comment by @petermattia on 2023-07-15 15:25_

In our use case, we configure the standard logger in the package-level `__init.py__` and then import the configured `logger` object into our other modules. I imagine this use case is pretty common, but not positive.

---

_Comment by @charliermarsh on 2023-07-15 15:55_

Yeah your use-case is very reasonable I think. The case I’m worried about is someone exporting something that matches the logger-like naming patterns we detect that isn’t actually a standard-library logger.

---

_Comment by @petermattia on 2023-07-17 14:57_

Understood. How do you resolve situations like this? I suppose one option is to make the intended behavior configurable.

FWIW, I think the affected logging rules make sense for any logger, not just the standard library logger.

---

_Comment by @charliermarsh on 2023-07-23 00:44_

What we may want to do for now is add a `logging-modules` setting similar to [`typing-modules`](https://beta.ruff.rs/docs/settings/#typing-modules) that lets you specify first-party modules that export loggers.

---

_Comment by @petermattia on 2023-07-23 05:15_

That sounds perfect!

---

_Renamed from "Detect local logging module re-exports in flake8-logging-format" to "Allow users to specify re-exports `logging.Logger` objects via `logger-objects`" by @charliermarsh on 2023-07-24 04:25_

---

_Renamed from "Allow users to specify re-exports `logging.Logger` objects via `logger-objects`" to "Allow specification of `logging.Logger` re-exports via `logger-objects`" by @charliermarsh on 2023-07-24 04:25_

---

_Label `configuration` added by @charliermarsh on 2023-07-24 04:26_

---

_Merged by @charliermarsh on 2023-07-24 04:38_

---

_Closed by @charliermarsh on 2023-07-24 04:38_

---

_Branch deleted on 2023-07-24 04:38_

---
