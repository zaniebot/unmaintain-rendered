```yaml
number: 5327
title: "Fix `python-build-standalone` workflow"
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: gh-action
created_at: 2024-07-23T03:09:33Z
updated_at: 2024-07-23T12:37:14Z
url: https://github.com/astral-sh/uv/pull/5327
synced_at: 2026-01-10T13:37:23Z
```

# Fix `python-build-standalone` workflow

---

_Pull request opened by @j178 on 2024-07-23 03:09_

## Summary

The script reads `GITHUB_TOKEN` instead. And since #4853 merged, there is no need to use `uv run --with`.


---

_Review comment by @zanieb on `.github/workflows/python-build-standalone.yml`:27 on 2024-07-23 03:23_

Huh, TIL both are supported: https://cli.github.com/manual/gh_help_environment

I have a preference for reading `GITHUB_TOKEN` in the script but only a small one.

---

_@zanieb reviewed on 2024-07-23 03:23_

---

_@charliermarsh approved on 2024-07-23 12:20_

---

_Merged by @charliermarsh on 2024-07-23 12:20_

---

_Closed by @charliermarsh on 2024-07-23 12:20_

---

_Label `internal` added by @charliermarsh on 2024-07-23 12:20_

---

_Branch deleted on 2024-07-23 12:37_

---
