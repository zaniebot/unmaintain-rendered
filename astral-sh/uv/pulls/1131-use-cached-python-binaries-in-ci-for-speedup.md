```yaml
number: 1131
title: Use cached Python binaries in CI for speedup
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
base: main
head: zb/cache-python-versions
created_at: 2024-01-26T19:25:57Z
updated_at: 2024-01-26T19:40:41Z
url: https://github.com/astral-sh/uv/pull/1131
synced_at: 2026-01-10T15:39:03Z
```

# Use cached Python binaries in CI for speedup

---

_Pull request opened by @zanieb on 2024-01-26 19:25_

#1105 added [about a minute](https://github.com/astral-sh/puffin/actions/runs/7672068625/job/20911761233) of overhead downloading Python binaries to CI. Unacceptable :)

- Do not re-download already installed Python versions in `install.sh`
- Cache Python binaries in CI using the cache action


---

_Comment by @zanieb on 2024-01-26 19:40_

Yeah uhh installation without direnv is really fast https://github.com/astral-sh/puffin/actions/runs/7672612003/job/20913477094

---

_Closed by @zanieb on 2024-01-26 19:40_

---
