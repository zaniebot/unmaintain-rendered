```yaml
number: 545
title: " Add test for cache source dist installing "
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/consider-unpacked-source-dist
created_at: 2023-12-04T14:59:28Z
updated_at: 2023-12-06T11:37:57Z
url: https://github.com/astral-sh/uv/pull/545
synced_at: 2026-01-10T15:44:44Z
```

#  Add test for cache source dist installing 

---

_Pull request opened by @konstin on 2023-12-04 14:59_

The code changes are outdated, now it's only adding a test

---

_Comment by @konstin on 2023-12-04 14:59_

Current dependencies on/for this PR:
* main
  * **PR #543** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/543?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #545** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/545?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #548** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/548?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #544** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/544?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/545?utm_source=stack-comment).

---

_@charliermarsh reviewed on 2023-12-04 15:12_

Can you walk me through why this is happening, in detail?

---

_Comment by @konstin on 2023-12-04 16:18_

There are three cases for a source distribution in the `DistributionDatabase`:
* The cache is outdated: Clear the cache and build a source dist, which is first stored as zip.
* The cache is fresh, but we never installed the built wheel: There is a built source dist stored as a zip.
* The cache is fresh, we previously installed the built wheel: There is a built source dist stored as unzipped directory.

In the first two cases we must unzip, in the last two we must not because there is no more zip to unzip, just the directory (this is where we previously failed). Instead of doing a complex `LocalWheel`, `CachedDist` and `DistributionDatabase` change when i know that i'll have to change this again soon, i'm just checking whether it's a zip (then unzip) or it's a dir (then do nothing, and also report it as cached).

---

_@charliermarsh reviewed on 2023-12-04 17:07_

But does that mean `remote` ends up including source distributions for which we already have a zipped wheel?

---

_Comment by @charliermarsh on 2023-12-04 17:24_

Why isn't the unzipped source distribution part of `local` rather than `remote`?

---

_Comment by @charliermarsh on 2023-12-04 18:06_

To me it seems like the bug is that we don't look in `built-wheels-v0` in the install plan.

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:47 on 2023-12-04 18:26_

Doesn't this represent a bug? See `// If we're replacing an existing directory, warn. If a wheel already exists` below.

---

_@charliermarsh reviewed on 2023-12-04 18:26_

---

_Renamed from "Correct behaviour when installing cached unpacked source dist" to " Add test for cache source dist installing " by @konstin on 2023-12-06 11:34_

---

_Comment by @konstin on 2023-12-06 11:34_

Changed this to only adding a test

---

_Merged by @konstin on 2023-12-06 11:37_

---

_Closed by @konstin on 2023-12-06 11:37_

---

_Branch deleted on 2023-12-06 11:37_

---
