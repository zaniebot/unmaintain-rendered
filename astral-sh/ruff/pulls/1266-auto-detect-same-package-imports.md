```yaml
number: 1266
title: Auto-detect same-package imports
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/first-party-detection
created_at: 2022-12-17T02:04:54Z
updated_at: 2022-12-17T02:19:12Z
url: https://github.com/astral-sh/ruff/pull/1266
synced_at: 2026-01-12T15:55:06Z
```

# Auto-detect same-package imports

---

_@charliermarsh_

Resolves #1257.

---

_Comment by @charliermarsh on 2022-12-17 02:07_

\cc @smackesey

Just as an example: if I run `cargo run dagster/examples/assets_pandas_type_metadata/assets_pandas_type_metadata/repository.py --select I --verbose --no-cache` on [`f2b8d88`](https://github.com/dagster-io/dagster/commit/f2b8d883c52ff4d7012a772cb00a146f4a50882b), I see the following in the logs:

```
...
[2022-12-16][21:07:23][ruff::isort::categorize][DEBUG] Categorized 'assets_pandas_type_metadata' as FirstParty (SamePackage)
[2022-12-16][21:07:23][ruff::isort::categorize][DEBUG] Categorized 'dagster' as FirstParty (KnownFirstParty)
[2022-12-16][21:07:23][ruff::isort::categorize][DEBUG] Categorized 'assets_pandas_type_metadata' as FirstParty (SamePackage)
...
```


---

_Merged by @charliermarsh on 2022-12-17 02:19_

---

_Closed by @charliermarsh on 2022-12-17 02:19_

---

_Branch deleted on 2022-12-17 02:19_

---
