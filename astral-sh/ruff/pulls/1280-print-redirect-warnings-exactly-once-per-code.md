```yaml
number: 1280
title: Print redirect warnings exactly once per code
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/once
created_at: 2022-12-18T17:03:43Z
updated_at: 2022-12-18T17:03:50Z
url: https://github.com/astral-sh/ruff/pull/1280
synced_at: 2026-01-12T15:55:06Z
```

# Print redirect warnings exactly once per code

---

_@charliermarsh_

```
∴ cargo run setup.py --select I252,I25,I25,I252
    Finished dev [unoptimized + debuginfo] target(s) in 0.33s
     Running `target/debug/ruff setup.py --select I252,I25,I25,I252`
warning: `I25` has been remapped to `TID25`
warning: `I252` has been remapped to `TID252`
```

---

_Merged by @charliermarsh on 2022-12-18 17:03_

---

_Closed by @charliermarsh on 2022-12-18 17:03_

---

_Branch deleted on 2022-12-18 17:03_

---
