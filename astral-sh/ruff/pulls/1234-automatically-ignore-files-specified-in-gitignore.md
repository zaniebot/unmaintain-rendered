```yaml
number: 1234
title: "Automatically ignore files specified in `.gitignore`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ignore
created_at: 2022-12-14T04:26:14Z
updated_at: 2022-12-14T20:58:42Z
url: https://github.com/astral-sh/ruff/pull/1234
synced_at: 2026-01-12T15:55:06Z
```

# Automatically ignore files specified in `.gitignore`

---

_@charliermarsh_

This is somewhat inefficient as I have to do two filesystem passes right now -- need to find a way to fix that...

Resolves #174.
Resolves #777.


---

_@charliermarsh reviewed on 2022-12-14 04:39_

---

_Review comment by @charliermarsh on `src/resolver.rs`:212 on 2022-12-14 04:39_

I want to use `WalkBuilder`, which should be pretty much a drop-in replacement for `WalkDir` (and let's me use the same `filter_entry`), but it's requiring that the lifetime of any variable in the closure if `'static`? So I can't iteratively build up the shared `resolver` like I am now.

---

_@charliermarsh reviewed on 2022-12-14 04:40_

---

_Review comment by @charliermarsh on `src/resolver.rs`:212 on 2022-12-14 04:40_

The current code could be slow if you had a very large directory that wasn't marked as `.gitignore` but was excluded by Ruff. E.g., if you had a monorepo with a bunch of directories that contained JS files, we'd spend time traversing all of those.


---

_Merged by @charliermarsh on 2022-12-14 20:58_

---

_Closed by @charliermarsh on 2022-12-14 20:58_

---

_Branch deleted on 2022-12-14 20:58_

---
