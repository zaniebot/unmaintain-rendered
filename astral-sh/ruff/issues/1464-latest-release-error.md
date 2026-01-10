---
number: 1464
title: Latest release error
type: issue
state: closed
author: ofek
labels: []
assignees: []
created_at: 2022-12-30T05:19:00Z
updated_at: 2022-12-30T13:17:14Z
url: https://github.com/astral-sh/ruff/issues/1464
synced_at: 2026-01-10T01:22:39Z
---

# Latest release error

---

_Issue opened by @ofek on 2022-12-30 05:19_

https://github.com/pypa/hatch/actions/runs/3804943026/jobs/6472530377

```
error missing field `banned-api` for key `tool.ruff.flake8-tidy-imports` at line 182 column 1
```

---

_Comment by @ofek on 2022-12-30 05:25_

Config:

```toml
[tool.ruff.flake8-tidy-imports]
ban-relative-imports = "all"
```

---

_Referenced in [astral-sh/ruff#1465](../../astral-sh/ruff/pulls/1465.md) on 2022-12-30 06:42_

---

_Comment by @not-my-profile on 2022-12-30 06:43_

Good catch ... my bad I missed that when introducing the new `banned-api` setting ... just opened a PR to fix this :)

---

_Closed by @charliermarsh on 2022-12-30 12:09_

---

_Comment by @charliermarsh on 2022-12-30 13:17_

I'm cutting a new release for this ([v0.0.202](https://github.com/charliermarsh/ruff/releases/tag/v0.0.202)).

---
