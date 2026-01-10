```yaml
number: 20232
title: "Stabilize `invalid-rule-code` (`RUF102`)"
type: pull_request
state: closed
author: ntBre
labels:
  - rule
assignees: []
draft: true
base: brent/0.13.0
head: brent/ruf102
created_at: 2025-09-04T13:36:07Z
updated_at: 2025-09-04T14:04:18Z
url: https://github.com/astral-sh/ruff/pull/20232
synced_at: 2026-01-10T17:46:21Z
```

# Stabilize `invalid-rule-code` (`RUF102`)

---

_Pull request opened by @ntBre on 2025-09-04 13:36_

Docs and tests look good


---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 13:36_

---

_Label `rule` added by @ntBre on 2025-09-04 13:36_

---

_Comment by @github-actions[bot] on 2025-09-04 13:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/providers/edge3/src/airflow/providers/edge3/worker_api/auth.py#L34'>providers/edge3/src/airflow/providers/edge3/worker_api/auth.py:34:79:</a> RUF102 [*] Invalid rule code in `# noqa`: TCH001
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/providers/edge3/src/airflow/providers/edge3/worker_api/datamodels.py#L28'>providers/edge3/src/airflow/providers/edge3/worker_api/datamodels.py:28:73:</a> RUF102 [*] Invalid rule code in `# noqa`: TCH001
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF102 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-09-04 13:56_

Hmm, this might be a problem. The ecosystem checks are flagging two TCH001 noqas. It seems like the rule should respect our own redirects. This works fine, for example, so I don't think these are actually invalid:

```shell
> cat try.py
from __future__ import annotations

import pandas as pd  # noqa: TCH002


def func(df: pd.DataFrame) -> int:
    return len(df)


> ruff check try.py --select TC002
All checks passed!
```

---

_Closed by @ntBre on 2025-09-04 14:04_

---
