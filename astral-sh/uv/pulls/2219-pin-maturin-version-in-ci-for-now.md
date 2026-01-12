```yaml
number: 2219
title: Pin maturin version in CI for now
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/maturin
created_at: 2024-03-05T20:39:00Z
updated_at: 2024-03-07T23:52:08Z
url: https://github.com/astral-sh/uv/pull/2219
synced_at: 2026-01-12T16:04:55Z
```

# Pin maturin version in CI for now

---

_@charliermarsh_

## Summary

In v1.5.0, Maturin now produces Metadata 2.3.0, which isn't supported in the GitHub Action: https://github.com/pypa/gh-action-pypi-publish/pull/219.

---

_Marked ready for review by @charliermarsh on 2024-03-05 20:39_

---

_Merged by @charliermarsh on 2024-03-05 20:49_

---

_Closed by @charliermarsh on 2024-03-05 20:49_

---

_Branch deleted on 2024-03-05 20:49_

---

_Comment by @webknjaz on 2024-03-07 23:26_

@charliermarsh FYI the [`pypi-publish`](https://github.com/marketplace/actions/pypi-publish) action has been updated to address this.

---

_Comment by @charliermarsh on 2024-03-07 23:52_

Thank you! Gonna unpin here and in Ruff.

---
