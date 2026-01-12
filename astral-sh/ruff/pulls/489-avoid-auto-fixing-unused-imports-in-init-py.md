```yaml
number: 489
title: Avoid auto-fixing unused imports in __init__.py
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/init
created_at: 2022-10-27T17:07:20Z
updated_at: 2022-10-27T17:08:05Z
url: https://github.com/astral-sh/ruff/pull/489
synced_at: 2026-01-12T15:55:04Z
```

# Avoid auto-fixing unused imports in __init__.py

---

_@charliermarsh_

Resolves #484:

```
∴ cargo run resources/test/fixtures/__init__.py -n
   Compiling ruff v0.0.85 (/Users/crmarsh/workspace/ruff)
    Finished dev [unoptimized + debuginfo] target(s) in 3.61s
     Running `target/debug/ruff resources/test/fixtures/__init__.py -n`
Found 1 error(s).
resources/test/fixtures/__init__.py:1:1: F401 `os` imported but unused and missing from `__all__`
```

---

_Merged by @charliermarsh on 2022-10-27 17:08_

---

_Closed by @charliermarsh on 2022-10-27 17:08_

---

_Branch deleted on 2022-10-27 17:08_

---
