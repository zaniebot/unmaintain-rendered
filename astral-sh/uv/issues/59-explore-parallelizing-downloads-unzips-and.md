```yaml
number: 59
title: Explore parallelizing downloads, unzips, and installs together
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-08T16:40:25Z
updated_at: 2023-10-16T02:16:58Z
url: https://github.com/astral-sh/uv/issues/59
synced_at: 2026-01-12T15:58:21Z
```

# Explore parallelizing downloads, unzips, and installs together

---

_@charliermarsh_

Right now, these steps are all parallelized to some degree, but the phases themselves are sequential. So, e.g., we don’t start unzipping (the slowest step) until we’ve downloaded everything.

We could instead use channels to ensure that we can schedule all work efficiently across the phases.

This would be a little more complex and makes debugging and logging harder (or more confusing). And it also may not have a significant impact, since the largest wheels are gonna be the bottleneck in each step anyway. (Actually, I think the bottleneck is largest file in any given wheel, like Ruff, which just has one huge executable.) But it’s worth a try.

---

_Closed by @charliermarsh on 2023-10-16 02:16_

---
