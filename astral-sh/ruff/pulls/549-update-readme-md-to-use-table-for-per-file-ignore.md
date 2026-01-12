```yaml
number: 549
title: Update README.md to use table for per-file-ignore
type: pull_request
state: merged
author: StefanBRas
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2022-11-02T11:25:45Z
updated_at: 2022-11-02T13:02:37Z
url: https://github.com/astral-sh/ruff/pull/549
synced_at: 2026-01-12T15:55:05Z
```

# Update README.md to use table for per-file-ignore

---

_@StefanBRas_

It is however a bit unhandy like this, because you're not allowed to have newlines in inline tables in TOML.

So either it needs to be on one line or one should use the syntax:

```toml
[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]
"path/to/file.py" = ["F401"]
```

---

_Merged by @charliermarsh on 2022-11-02 13:02_

---

_Closed by @charliermarsh on 2022-11-02 13:02_

---

_Comment by @charliermarsh on 2022-11-02 13:02_

Thank you! (Agree that separating into its own table is best if you have more than one or two ignores.)

---
