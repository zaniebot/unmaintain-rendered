```yaml
number: 913
title: Support find links urls
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/flat-indexes-2-find-links-urls
created_at: 2024-01-14T15:40:04Z
updated_at: 2024-01-15T14:11:45Z
url: https://github.com/astral-sh/uv/pull/913
synced_at: 2026-01-12T16:04:17Z
```

# Support find links urls

---

_@konstin_

The simple html format parser luckily seems to work for find links too, at least it can parse https://storage.googleapis.com/jax-releases/jax_cuda_releases.html.


---

_Comment by @konstin on 2024-01-14 15:40_

> [!WARNING]
> <b>This pull request is not mergeable via GitHub because a downstack PR is open. Once all requirements are satisfied, merge this PR as a stack <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/913?utm_source=stack-comment-downstack-mergeability-warning" >on Graphite</a>.</b>
> <a href="https://graphite.dev/docs/merge-pull-requests">Learn more</a>

Current dependencies on/for this PR:
* `main`
  * **PR #910** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/910?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #912** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/912?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #913** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/913?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
        * **PR #914** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/914?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/913?utm_source=stack-comment).

---

_@konstin reviewed on 2024-01-14 18:24_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:121 on 2024-01-14 18:24_

We currently block everything else on flat links requests. We don't know which packages they will contain, so we can't build any other version map before we have those. If we make them lazy, we can send the root dependency index requests in parallel with the flat index requests. If we want that, i'll do it in a separate PR, i expect a bunch of type changes.

Do you know of a good type for lazy async tasks? Best thing i found is https://docs.rs/async_once/latest/async_once/

---

_@konstin reviewed on 2024-01-14 18:25_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:124 on 2024-01-14 18:25_

I can do this in this PR or in a follow-up PR. Should we make `read_flat_index_dir` async also?

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:124 on 2024-01-15 02:14_

I'm generally okay with file I/O not being async but maybe it should be if we're streaming them all?

---

_@charliermarsh reviewed on 2024-01-15 02:14_

---

_@charliermarsh reviewed on 2024-01-15 02:15_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:121 on 2024-01-15 02:15_

Let's just leave it for now I suppose. Seems hard to make this lazy.

---

_@charliermarsh reviewed on 2024-01-15 02:42_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/provider.rs`:112 on 2024-01-15 02:42_

\cc @konstin - This is somewhat important. Without this change, we require that the package exists in at least one index. But that shouldn't be required, it could _only_ be listed in `--find-links`.

---

_@charliermarsh approved on 2024-01-15 02:52_

---

_Merged by @charliermarsh on 2024-01-15 02:54_

---

_Closed by @charliermarsh on 2024-01-15 02:54_

---

_Branch deleted on 2024-01-15 02:54_

---

_@konstin reviewed on 2024-01-15 14:10_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/provider.rs`:112 on 2024-01-15 14:10_

Good point!

Should we also check here that the overrides actually contain the package?

---

_@charliermarsh reviewed on 2024-01-15 14:11_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/provider.rs`:112 on 2024-01-15 14:11_

Isn’t that what we’re doing via the get call?

---
