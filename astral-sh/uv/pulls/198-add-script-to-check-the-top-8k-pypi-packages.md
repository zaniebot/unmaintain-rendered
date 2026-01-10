```yaml
number: 198
title: Add script to check the top 8k pypi packages
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: test-top-pypi-packages
created_at: 2023-10-26T11:56:22Z
updated_at: 2023-10-26T12:04:00Z
url: https://github.com/astral-sh/uv/pull/198
synced_at: 2026-01-10T15:50:28Z
```

# Add script to check the top 8k pypi packages

---

_Pull request opened by @konstin on 2023-10-26 11:56_

To check to top 1k (current state):

```bash
scripts/resolve/get_pypi_top_8k.sh
cargo run --bin puffin-dev -- resolve-many scripts/resolve/pypi_top_8k_flat.txt --limit 1000
```

Results:
```
Errors: pywin32, geoip2, maxminddb, pypika, dirac
Success: 995, Error: 5
```
pywin32 has no solution for the build environment, 3 have no `[build-system]` entry in pyproject.toml, `dirac` is missing cmake

---

_Merged by @konstin on 2023-10-26 12:03_

---

_Closed by @konstin on 2023-10-26 12:03_

---

_Branch deleted on 2023-10-26 12:04_

---
