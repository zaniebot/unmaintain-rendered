```yaml
number: 2118
title: "Implement `EXE001` and `EXE002` from `flake8-executable`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: feat/exe001-exe002
created_at: 2023-01-24T05:25:35Z
updated_at: 2023-02-22T03:53:53Z
url: https://github.com/astral-sh/ruff/pull/2118
synced_at: 2026-01-12T04:39:44Z
```

# Implement `EXE001` and `EXE002` from `flake8-executable`

---

_Pull request opened by @edgarrmondragon on 2023-01-24 05:25_

https://github.com/charliermarsh/ruff/issues/2024


---

_Comment by @edgarrmondragon on 2023-01-24 05:36_

I think the clippy check for wasm is failing because `Metadata.mode()` is only available on Unix

---

_Merged by @charliermarsh on 2023-01-24 13:02_

---

_Closed by @charliermarsh on 2023-01-24 13:02_

---

_Comment by @basnijholt on 2023-02-22 03:29_

These shouldn't be executed on Windows.

---

_Comment by @charliermarsh on 2023-02-22 03:47_

How do the Unix extensions even compile on Windows?

---

_Comment by @basnijholt on 2023-02-22 03:53_

I noticed this because pre-commit running on `windows-latest` [Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#software) agents, failed, where the Linux jobs pass.

I just `pip install ruff` there ¯\\_(ツ)_/¯ 

---
