```yaml
number: 519
title: Clear built wheels when remote changed
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: clear-built-wheels-when-remote-changed
created_at: 2023-11-30T11:36:27Z
updated_at: 2023-12-01T19:56:48Z
url: https://github.com/astral-sh/uv/pull/519
synced_at: 2026-01-10T15:44:44Z
```

# Clear built wheels when remote changed

---

_Pull request opened by @konstin on 2023-11-30 11:36_

Remove built wheels alongside their metadata when their index source dist or url source dist changed. For git source dists, we currently don't clear the previous build but use a new directory (not sure what's right here - are there any generic cache GC approaches out there? I've seen that e.g. spotify keeps its cache at 10GB max, but i also haven't seen any reusable, well tested approaches for this). Path distributions are unchanged (#478).

I like the structure of metadata alongside the wheel for cache invalidation, i'll try to do that for `wheels-v0`/`wheel-metadata-v0` too. (The unzipped wheels afaik currently lack cache invalidation when the remote changed.) This should give is roughly the same structure for wheel and built wheels and a very similar pattern of invalidation.

---

_Comment by @konstin on 2023-12-01 12:06_

Current dependencies on/for this PR:
* main
  * **PR #519** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/519?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #524** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/524?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/519?utm_source=stack-comment).

---

_Marked ready for review by @konstin on 2023-12-01 12:07_

---

_Comment by @konstin on 2023-12-01 12:07_

This and the stacked PR are now ready for review

---

_@charliermarsh approved on 2023-12-01 18:35_

---

_Merged by @charliermarsh on 2023-12-01 19:56_

---

_Closed by @charliermarsh on 2023-12-01 19:56_

---

_Branch deleted on 2023-12-01 19:56_

---
