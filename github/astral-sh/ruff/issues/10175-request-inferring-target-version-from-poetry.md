---
number: 10175
title: "Request: inferring `target_version` from `poetry` config"
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2024-02-29T16:50:51Z
updated_at: 2024-03-05T13:54:54Z
url: https://github.com/astral-sh/ruff/issues/10175
synced_at: 2026-01-07T13:12:15-06:00
---

# Request: inferring `target_version` from `poetry` config

---

_Issue opened by @jamesbraza on 2024-02-29 16:50_

As of `ruff==0.2.2`, the [`target_version`](https://docs.astral.sh/ruff/settings/#target-version) can be inferrred from `pyproject.toml` package metadata in `project`.

It would be cool if, for people managing their code using `poetry`, if the `target_version` could infer from the `python` range:

```toml
[tool.poetry.dependencies]
python = ">=3.9,<3.13"
```

Thank you in advance for your consideration!

---

_Comment by @charliermarsh on 2024-02-29 17:23_

Hey! This was requested once before and a Poetry maintainer suggested that we not bother since they want to support PEP 621: https://github.com/astral-sh/ruff/issues/5448. So we've held off on it...

---

_Comment by @jamesbraza on 2024-02-29 17:45_

Okay sounds good, I follow. Feel free to leave this open or close it out then, it's up to you

---

_Comment by @charliermarsh on 2024-03-05 13:54_

Gonna close based on the above, though we may revisit if their stance on PEP 621 changes.

---

_Closed by @charliermarsh on 2024-03-05 13:54_

---
