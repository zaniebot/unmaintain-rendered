```yaml
number: 1650
title: Add warning when dependencies are empty with Poetry metadata
type: pull_request
state: merged
author: yasufumy
labels:
  - error messages
assignees: []
merged: true
base: main
head: main
created_at: 2024-02-18T14:38:11Z
updated_at: 2024-02-20T02:45:41Z
url: https://github.com/astral-sh/uv/pull/1650
synced_at: 2026-01-10T15:33:24Z
```

# Add warning when dependencies are empty with Poetry metadata

---

_Pull request opened by @yasufumy on 2024-02-18 14:38_

Resolve #1630 

`PyProjectToml` doesn't seem to have a `tool` field, so instead of checking it,  I check if `requirements` is empty.

https://github.com/astral-sh/uv/blob/c04f597fae7a04be6124d91a9b77f211941f3cd6/crates/uv-build/src/lib.rs#L176-L184

---

_@zanieb reviewed on 2024-02-18 17:14_

---

_Review comment by @zanieb on `crates/uv/src/requirements.rs`:221 on 2024-02-18 17:14_

Could we move this down and make it conditional on `requirements` being empty? It's possible that they've added a dependency section we can read even if they're using Poetry.

We have a similar warning for `requirements.txt` files

https://github.com/astral-sh/uv/blob/2ea44d863aca4b13823c027a21755067e2dc0050/crates/requirements-txt/src/lib.rs#L317-L322

Maybe we can phrase it the same with a hint that we can't read Poetry's dependencies?

---

_@charliermarsh reviewed on 2024-02-18 18:56_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:221 on 2024-02-18 18:56_

(Is it possible to make this conditional on whether `tool.poetry.dependencies` is populated? I assume not, since we don't include that in TOML schema. If so, this is totally fine.)

---

_Label `error messages` added by @zanieb on 2024-02-19 02:20_

---

_Label `tracing` added by @zanieb on 2024-02-19 02:20_

---

_Label `tracing` removed by @zanieb on 2024-02-19 02:20_

---

_@yasufumy reviewed on 2024-02-19 04:50_

---

_Review comment by @yasufumy on `crates/uv/src/requirements.rs`:221 on 2024-02-19 04:50_

@zanieb Thank you for your suggestion. I updated my PR and also fixed a test case. Let me know if I miss anything!

@charliermarsh Thank you for your comment. Are you planning to support the poetry-style format? If so, and no one is working on it, I'm interested in contributing.

---

_Renamed from "Add warning when `poetry` dependencies found." to "Add warning when dependencies are empty." by @yasufumy on 2024-02-19 05:09_

---

_Renamed from "Add warning when dependencies are empty." to "Add warning when dependencies are empty" by @yasufumy on 2024-02-19 05:21_

---

_@charliermarsh approved on 2024-02-19 23:52_

Thanks!

---

_Renamed from "Add warning when dependencies are empty" to "Add warning when dependencies are empty with Poetry metadata" by @charliermarsh on 2024-02-19 23:52_

---

_Merged by @charliermarsh on 2024-02-20 00:08_

---

_Closed by @charliermarsh on 2024-02-20 00:08_

---

_@hauntsaninja reviewed on 2024-02-20 01:34_

---

_Review comment by @hauntsaninja on `crates/uv/src/requirements.rs`:246 on 2024-02-20 01:34_

Maybe this should be more specific, e.g. `hint: specify dependencies in project.dependencies section, tool.poetry.dependencies is not currently supported`. I can imagine users taking "Poetry's format" to mean all kinds of things, like literally the use of pyproject.toml

---

_@charliermarsh reviewed on 2024-02-20 02:45_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:246 on 2024-02-20 02:45_

Nice idea: https://github.com/astral-sh/uv/pull/1730

---
