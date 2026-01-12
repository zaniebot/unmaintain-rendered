```yaml
number: 5954
title: "Use Ruff's import resolver to power import sorting"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/import-resolver
created_at: 2023-07-21T19:49:20Z
updated_at: 2023-07-27T13:22:17Z
url: https://github.com/astral-sh/ruff/pull/5954
synced_at: 2026-01-12T03:30:22Z
```

# Use Ruff's import resolver to power import sorting

---

_Pull request opened by @charliermarsh on 2023-07-21 19:49_

## Summary

Work-in-progress PR to migrate our import sorting categorizer over to Ruff's import resolver. Previously, the import categorization logic just looked at the first part of an import (e.g., `import foo.bar`, take `foo`) and tried to find either a `foo` directory or a `foo.py` file. This often worked, but (e.g.) doesn't properly handle namespace packages, and improperly categorizes projects with structures like:

```py
bar/
   bar/
     __init__.py
```

## Test Plan

I tested over a bunch of repos and verified that sorting is as-before:

- **Airflow** (able to remove their `known-first-party` and `known-third-party` classifications while retaining parity with the existing sort)
- **Zulip**
- **FastAPI**
- **Pydantic** (able to remove `known-third-party`)
- **Poetry**

This PR also passes all existing import-sorting tests in Ruff.

This needs more testing in the wild though. For example, it dramatically changed Dagster's sorting, I think because Dagster is implicitly relying on the above behavior to treat Dagster as first-party in various contexts. I'd need to come up with a solution for Dagster specifically prior to merging.


---

_@charliermarsh reviewed on 2023-07-21 19:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/categorize.rs`:111 on 2023-07-21 19:49_

We also _probably_ want to add caching here.

---

_Comment by @github-actions[bot] on 2023-07-21 20:03_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.03ms     4.2 MB/sec    1.01      9.9±0.03ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1904.8±9.20µs     8.7 MB/sec    1.00   1911.1±6.20µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    205.3±0.57µs    14.4 MB/sec    1.01    208.1±2.75µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.01      4.2±0.01ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.08ms     3.0 MB/sec    1.01     13.8±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.03      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.5±0.72µs     7.9 MB/sec    1.00    374.2±0.88µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.8 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1437.3±2.70µs    11.6 MB/sec    1.00   1429.5±3.91µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.7±0.23µs    19.6 MB/sec    1.00    150.4±0.32µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.00      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.9±0.20ms     3.7 MB/sec    1.00     10.8±0.09ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.03ms     7.8 MB/sec    1.00      2.1±0.06ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    241.2±5.54µs    12.2 MB/sec    1.01   242.5±10.91µs    12.2 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.00      4.7±0.07ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.25ms     2.6 MB/sec    1.03     15.9±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.14      4.6±0.05ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    491.3±8.86µs     6.0 MB/sec    1.00    487.2±6.54µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.7 MB/sec    1.03      7.2±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.08ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1661.4±16.61µs    10.0 MB/sec    1.01  1676.4±15.28µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.7±3.02µs    15.6 MB/sec    1.02    192.6±3.94µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-21 20:10_

Sweet!

I see a mix of correct and incorrect changes over in Prefect, e.g.

```diff

--- /Users/mz/eng/src/prefecthq/prefect/tests/server/orchestration/test_rules.py
+++ /Users/mz/eng/src/prefecthq/prefect/tests/server/orchestration/test_rules.py
@@ -1,13 +1,12 @@
 import contextlib
 import random
-import sqlalchemy.exc
+import sqlite3
 from itertools import product
 from unittest.mock import MagicMock
 
 import pendulum
 import pytest
-import sqlite3
-
+import sqlalchemy.exc
 from prefect.server import models, schemas
 from prefect.server.database.dependencies import provide_database_interface
 from prefect.server.exceptions import OrchestrationError
```

```diff
--- /Users/mz/eng/src/prefecthq/prefect/tests/workers/test_process_worker.py
+++ /Users/mz/eng/src/prefecthq/prefect/tests/workers/test_process_worker.py
@@ -9,10 +9,8 @@
 import anyio
 import anyio.abc
 import pendulum
-import pytest
-from pydantic import BaseModel
-
 import prefect
+import pytest
 from prefect import flow
 from prefect.client.orchestration import PrefectClient
 from prefect.client.schemas import State
@@ -25,6 +23,7 @@
     ProcessWorker,
     ProcessWorkerResult,
 )
+from pydantic import BaseModel
```

---

_Comment by @charliermarsh on 2023-07-21 20:11_

I tried to test on Prefect but it didn't seem to have the `I` rules enabled, I'll take another look.

---

_Comment by @charliermarsh on 2023-07-21 20:12_

How'd you test it? Running latest `ruff` on Prefect for import sorting shows a bunch of changes, so trying to generate the above diff :)

---

_Comment by @zanieb on 2023-07-21 21:07_

Oh funny. We used `isort` previously and I explicitly ignored `I` rules in some files so they must have been being applied at some point? Maybe I'll open a pull request over there to fix the baseline. Anyway, I just explicitly selected them to get a sense for what would change with this pull request e.g. `./target/debug/ruff --select TCH001,TCH002,I001 /Users/mz/eng/src/prefecthq/prefect/src/prefect --diff` and there are indeed a lot of changes. The diff above is just a subset, for a couple files where I thought the behavior was weird.

---

_Comment by @charliermarsh on 2023-07-22 02:12_

Okay for Prefect, I get stable behavior (between latest `ruff` and this branch) if I add `src = ["src"]` to the `.ruff.toml`.

---

_Comment by @charliermarsh on 2023-07-27 01:46_

A couple comments from playing around with this a bunch:

- I think we still need a reliable way to determine the "root" of a Python package that works with namespace packages. The module resolve supports namespace packages, but we still need to provide it with a "package root" from which to resolve.
- Dagster in particular is pointing out that there's a difference in definition RE "Which imports are first-party?" for different projects. For Dagster, they only want imports to be considered first-party if they're in the same package; otherwise, they're third-party, even though they're defined in the same monorepo. The way that works today is that they don't set `src`, and they rely on the "same-package" heuristic.

---

_Comment by @charliermarsh on 2023-07-27 13:22_

I'm going to call this a non-goal. It's been really helpful for re-learning and documenting our approach to import classification and module discovery, but using the import resolver here has few benefits and some downsides (mainly, that it's going to be a lot slower, and it's going to have false negatives when users have imports that don't correspond to existing files, as in Airflow).

There are some improvements that I want to make to import classification and module discovery, but they're independent of the import resolver.

For the resolver itself, there's at least one bugfix in this PR that I'll pull out separately, and the other TODOs are just around using a shared resolver throughout Ruff, improving performance, and making the implementation and API more Rust-y.

---

_Closed by @charliermarsh on 2023-07-27 13:22_

---
