```yaml
number: 17464
title: Adjust the process ulimit to the maximum allowed on startup
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: claude/add-ulimit-adjustment-pbq7y
created_at: 2026-01-14T14:49:11Z
updated_at: 2026-01-14T15:22:01Z
url: https://github.com/astral-sh/uv/pull/17464
synced_at: 2026-01-14T15:39:55Z
```

# Adjust the process ulimit to the maximum allowed on startup

---

_@zanieb_

Closes #16999

See the commentary at https://github.com/astral-sh/uv/issues/16999#issuecomment-3641417484 for precedence in other tools.

This is non-fatal on error, which may make it safe to roll out without preview gating.


---
