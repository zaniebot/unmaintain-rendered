```yaml
number: 914
title: Add find links supports to pip-sync
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/flat-indexes-3-pip-install
created_at: 2024-01-14T15:40:06Z
updated_at: 2024-01-15T03:04:57Z
url: https://github.com/astral-sh/uv/pull/914
synced_at: 2026-01-12T16:04:17Z
```

# Add find links supports to pip-sync

---

_@konstin_

Closes #877


---

_Comment by @konstin on 2024-01-14 15:40_

> [!WARNING]
> <b>This pull request is not mergeable via GitHub because a downstack PR is open. Once all requirements are satisfied, merge this PR as a stack <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/914?utm_source=stack-comment-downstack-mergeability-warning" >on Graphite</a>.</b>
> <a href="https://graphite.dev/docs/merge-pull-requests">Learn more</a>

Current dependencies on/for this PR:
* `main`
  * **PR #910** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/910?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #912** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/912?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #913** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/913?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #914** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/914?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/914?utm_source=stack-comment).

---

_@charliermarsh approved on 2024-01-15 03:03_

Looks good to me. I might play with moving the flat index stuff out of the registry as a separate change, it's not clear to me how it would turn out.

---

_Merged by @charliermarsh on 2024-01-15 03:04_

---

_Closed by @charliermarsh on 2024-01-15 03:04_

---

_Branch deleted on 2024-01-15 03:04_

---
