```yaml
number: 818
title: Use a dedicated flag for each tool in bench script
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/flags
created_at: 2024-01-06T19:39:14Z
updated_at: 2024-01-06T19:45:01Z
url: https://github.com/astral-sh/uv/pull/818
synced_at: 2026-01-12T16:04:12Z
```

# Use a dedicated flag for each tool in bench script

---

_@charliermarsh_

Taking some of Zanie's suggestions to make the custom-path API simpler in the benchmark script. Each tool is now a dedicated argument, like:

```
python -m scripts.bench --pip-sync --poetry requirements.in
```

To provide custom binaries:

```
python -m scripts.bench \
    --puffin-path ./target/release/puffin \
    --puffin-path ./target/release/baseline \
    requirements.in
```

---

_Label `internal` added by @charliermarsh on 2024-01-06 19:40_

---

_Marked ready for review by @charliermarsh on 2024-01-06 19:40_

---

_Merged by @charliermarsh on 2024-01-06 19:45_

---

_Closed by @charliermarsh on 2024-01-06 19:45_

---

_Branch deleted on 2024-01-06 19:45_

---
