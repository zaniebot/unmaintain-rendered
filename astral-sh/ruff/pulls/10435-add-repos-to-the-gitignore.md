```yaml
number: 10435
title: "Add `repos/` to the gitignore"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - internal
assignees: []
merged: true
base: main
head: hoel/add_repos_to_gitignore
created_at: 2024-03-17T12:20:33Z
updated_at: 2024-03-17T14:18:30Z
url: https://github.com/astral-sh/ruff/pull/10435
synced_at: 2026-01-12T15:55:32Z
```

# Add `repos/` to the gitignore

---

_@hoel-bagard_

## Summary

I would like to add `repos/` to the gitignore since it is given as an example for the cache directory path in [the ecosystem check's README](https://github.com/astral-sh/ruff/tree/main/python/ruff-ecosystem#development):

```console
ruff-ecosystem check ruff "./target/debug/ruff" --cache ./repos
```

---

_@zanieb approved on 2024-03-17 14:18_

Thanks!

---

_Label `internal` added by @zanieb on 2024-03-17 14:18_

---

_Merged by @zanieb on 2024-03-17 14:18_

---

_Closed by @zanieb on 2024-03-17 14:18_

---
