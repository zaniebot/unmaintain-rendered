```yaml
number: 912
title: "Implement `--find-links` as flat indexes (directories in pip-compile)"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/flat-indexes-1-pip-compile
created_at: 2024-01-14T15:40:02Z
updated_at: 2024-01-15T02:04:11Z
url: https://github.com/astral-sh/uv/pull/912
synced_at: 2026-01-10T15:39:02Z
```

# Implement `--find-links` as flat indexes (directories in pip-compile)

---

_Pull request opened by @konstin on 2024-01-14 15:40_

Add directory `--find-links` support for local paths to pip-compile.

It seems that pip joins all sources and then picks the best package. We explicitly give find links packages precedence if the same exists on an index and locally by prefilling the `VersionMap`, otherwise they are added as another index and the existing rules of precedence apply.

Internally, the feature is called _flat index_, which is more meaningful than _find links_: We're not looking for links, we're picking up local directories, and (TBD) support another index format that's just a flat list of files instead of a nested index.

`RegistryBuiltDist` and `RegistrySourceDist` now use `WheelFilename` and `SourceDistFilename` respectively. The `File` inside `RegistryBuiltDist` and `RegistrySourceDist` gained the ability to represent both a url and a path so that `--find-links` with a url and with a path works the same, both being locked as `<package_name>@<version>` instead of `<package_name> @ <url>`. (This is more of a detail, this PR in general still work if we strip that and have directory find links represented as `<package_name> @ file:///path/to/file.ext`)

`PrioritizedDistribution` and `FlatIndex` have been moved to locations where we can use them in the upstack PR.

I added a `scripts/wheels` directory with stripped down wheels to use for testing.

We're lacking tests for correct tag priority precedence with flat indexes, i only confirmed this manually since it is not covered in the pip-compile or pip-sync output.

Closes #876



---

_Comment by @konstin on 2024-01-14 15:40_

> [!WARNING]
> <b>This pull request is not mergeable via GitHub because a downstack PR is open. Once all requirements are satisfied, merge this PR as a stack <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/912?utm_source=stack-comment-downstack-mergeability-warning" >on Graphite</a>.</b>
> <a href="https://graphite.dev/docs/merge-pull-requests">Learn more</a>

Current dependencies on/for this PR:
* `main`
  * **PR #910** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/910?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #912** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/912?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #913** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/913?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #914** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/914?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/912?utm_source=stack-comment).

---

_@konstin reviewed on 2024-01-14 17:59_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:128 on 2024-01-14 17:59_

This is fixed in an upstack PR

---

_Review comment by @konstin on `crates/distribution-types/src/prioritized_distribution.rs`:1 on 2024-01-14 18:35_

Open to moving those anywhere else, they just need to be accessible from both the resolver and the dist finder

---

_@konstin reviewed on 2024-01-14 18:35_

---

_@konstin reviewed on 2024-01-14 18:39_

---

_Review comment by @konstin on `crates/puffin-distribution/src/source/mod.rs`:104 on 2024-01-14 18:39_

Not ideal, but feels fine for the few cases and getting consistency.

---

_@charliermarsh reviewed on 2024-01-15 01:40_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:871 on 2024-01-15 01:40_

(May've simplified to do this as a small, separate change.)

---

_@charliermarsh approved on 2024-01-15 01:42_

This looks good to me -- I'm gonna apply some tweaks and merge.

---

_@charliermarsh reviewed on 2024-01-15 01:42_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source/mod.rs`:104 on 2024-01-15 01:42_

I do wonder if these could be separate distribution types, but not critical.

---

_Merged by @charliermarsh on 2024-01-15 02:04_

---

_Closed by @charliermarsh on 2024-01-15 02:04_

---

_Branch deleted on 2024-01-15 02:04_

---
