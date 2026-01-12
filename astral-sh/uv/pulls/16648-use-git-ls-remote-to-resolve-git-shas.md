```yaml
number: 16648
title: "Use `git ls-remote` to resolve Git SHAs"
type: pull_request
state: open
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/ls-remote
created_at: 2025-11-09T04:13:39Z
updated_at: 2025-11-09T04:31:29Z
url: https://github.com/astral-sh/uv/pull/16648
synced_at: 2026-01-12T16:12:22Z
```

# Use `git ls-remote` to resolve Git SHAs

---

_@charliermarsh_

## Summary

This enables us to take a fast-path for commit resolution even for private repositories. This can be beneficial for cases in which we have a tag or branch name, but it's not clear that it's a tag or branch name, since we can skip the expensive path where we fetch all tags and branches.

Closes #16604.


---

_Marked ready for review by @charliermarsh on 2025-11-09 04:23_

---
