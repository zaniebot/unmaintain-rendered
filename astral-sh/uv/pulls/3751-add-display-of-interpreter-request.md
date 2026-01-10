```yaml
number: 3751
title: Add display of interpreter request
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/display-search
created_at: 2024-05-22T18:16:24Z
updated_at: 2024-05-22T18:26:36Z
url: https://github.com/astral-sh/uv/pull/3751
synced_at: 2026-01-10T14:32:20Z
```

# Add display of interpreter request

---

_Pull request opened by @zanieb on 2024-05-22 18:16_

e.g. 

```
❯ echo "anyio" | cargo run -q -- pip compile - --python 3.9 -v
DEBUG Searching for interpreter that fulfills Python @ 3.9
DEBUG Found a virtual environment at: /Users/zb/workspace/uv/.venv
DEBUG Using Python 3.9.18 interpreter at bin/cpython-3.9.18-macos-aarch64-none/install/bin/python3 for builds
```


---

_Label `tracing` added by @zanieb on 2024-05-22 18:16_

---

_Merged by @zanieb on 2024-05-22 18:26_

---

_Closed by @zanieb on 2024-05-22 18:26_

---

_Branch deleted on 2024-05-22 18:26_

---
