```yaml
number: 6959
title: Prune unreachable packages from lockfile
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/remove-dead-packages
created_at: 2024-09-03T08:31:26Z
updated_at: 2024-09-04T12:51:56Z
url: https://github.com/astral-sh/uv/pull/6959
synced_at: 2026-01-10T12:53:37Z
```

# Prune unreachable packages from lockfile

---

_Pull request opened by @konstin on 2024-09-03 08:31_

In transformers, we have:

* `tensorflow-text`: `tensorflow-macos; python_full_version >= '3.13' and platform_machine == 'arm64' and platform_system == 'Darwin'`
* `tensorflow-macos`: `tensorflow-cpu-aws; (python_full_version < '3.10' and platform_machine == 'aarch64' and platform_system == 'Linux') or (python_full_version >= '3.13' and platform_machine == 'aarch64' and platform_system == 'Linux') or (python_full_version >= '3.13' and platform_machine == 'arm64' and platform_system == 'Linux')`
* `tensorflow-macos`: `tensorflow-intel; python_full_version >= '3.13' and platform_system == 'Windows'`

This means that `tensorflow-cpu-aws` and `tensorflow-intel` can never be installed, and we can drop them from the lockfile.

---

_Label `enhancement` added by @konstin on 2024-09-03 08:31_

---

_@charliermarsh reviewed on 2024-09-03 17:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/graph.rs`:303 on 2024-09-03 17:07_

A DFS / BFS wasn't sufficient for creating the marker expressions for `pip compile --universal`. That's why we have `propagate_markers`. Is there a reason that it's different here?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/graph.rs`:303 on 2024-09-03 17:07_

I think you need to guarantee that you've visited _every_ parent before you visit any child.

---

_@charliermarsh reviewed on 2024-09-03 17:07_

---

_Comment by @charliermarsh on 2024-09-03 17:08_

How do these even make it into the graph in the first place? 🤔  Is there any way for us to prevent it?

---

_@konstin reviewed on 2024-09-04 08:27_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/graph.rs`:303 on 2024-09-04 08:27_

`propagate_markers` is updating the graph itself and not considering the starting environments, so i found it easier to create a separate method.

The algorithm is a variant of Dijkstra's algorithm for not totally ordered distances: Whenever we find a shorter distance to a node (a marker that is not a subset of the existing marker), we re-queue the node and update all its children: We may not have visited all parents yet, but if a parent propagates an update, we will also propagate it to all children. This implicitly handles cycles, whenever we re-reach a node through a cycle the marker we have is a more specific marker/longer path, so we don't update the node and don't re-queue it.

---

_Comment by @konstin on 2024-09-04 08:57_

tl;dr: Not without an extra layer of virtual packages or forking.

Say we have A -> B on linux and B -> C on windows. We first process A with A -> B, and then B with B -> C. At this point, we know that B is linux only, so we could drop the B -> C edge. Later, we process a package D (universally included), with D -> B. Now we would need to add back C since it is included again, but this doesn't work since pubgrub needs all it's incompatibilities to be immutable.

To mitigate this, we would need a virtual package for each platform, treating platforms like extras, so e.g. splitting the real B into B[not(windows)] and B[windows], with A[universal] -> B[not(windows)], B[windows] -> C[...], B[not(windows)] -> B, B[windows] -> B, and all packages without platforms being dummies that only force the same version of a package. Since we need to propagate this information across intermediary dependencies, we would have to create virtual packages for all intermediary dependencies, creating a massive virtual dependency tree.

We could also fork every time we see a platform marker, for the known trade-offs.

Cargo has this problem too.

---

_Merged by @konstin on 2024-09-04 08:57_

---

_Closed by @konstin on 2024-09-04 08:57_

---

_Branch deleted on 2024-09-04 08:57_

---

_@charliermarsh reviewed on 2024-09-04 12:51_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/graph.rs`:303 on 2024-09-04 12:51_

If this is true, can you update propagate_markers to use the same algorithm? We shouldn’t have two implementations of the same technique.

---
