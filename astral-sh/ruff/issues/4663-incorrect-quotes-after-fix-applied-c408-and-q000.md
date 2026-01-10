```yaml
number: 4663
title: Incorrect quotes after fix applied (C408 and Q000 rules fixes)
type: issue
state: open
author: charliermarsh
labels:
  - configuration
assignees: []
created_at: 2023-05-26T03:03:58Z
updated_at: 2023-05-26T03:03:58Z
url: https://github.com/astral-sh/ruff/issues/4663
synced_at: 2026-01-10T11:09:47Z
```

# Incorrect quotes after fix applied (C408 and Q000 rules fixes)

---

_Issue opened by @charliermarsh on 2023-05-26 03:03_

_Originally posted as https://github.com/astral-sh/ruff-lsp/issues/123._

---

Not sure if this should be opened here or in https://github.com/charliermarsh/ruff, but since I am using LSP I posted here

My issue is following: 
`C408` and `Q000` fix rules are incoherent. (And probably other rules besides C408 that produce string literals)

Having 
<img width="835" alt="image" src="https://github.com/astral-sh/ruff-lsp/assets/14924440/f0d9beac-fd3d-4730-817a-2238c94cf31a">
and this lines in `ruff.toml`: 
```toml
[flake8-quotes]
inline-quotes = "single"
```
After applying code action `Fix C408` becomes
<img width="642" alt="image" src="https://github.com/astral-sh/ruff-lsp/assets/14924440/6c59cf0c-8c28-476b-95c1-d30f6e293091">

So I need to fix these string literals after this, which could be avoided. 


---

_Label `configuration` added by @charliermarsh on 2023-05-26 03:03_

---
