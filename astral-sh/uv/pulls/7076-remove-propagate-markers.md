```yaml
number: 7076
title: Remove propagate_markers
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/propagate_markers
created_at: 2024-09-05T10:54:59Z
updated_at: 2024-09-05T17:26:26Z
url: https://github.com/astral-sh/uv/pull/7076
synced_at: 2026-01-10T12:53:39Z
```

# Remove propagate_markers

---

_Pull request opened by @konstin on 2024-09-05 10:54_

Follow-up to #6959 and #6961: Use the reachability computation instead of `propagate_markers` everywhere.

With `marker_reachability`, we have a function that computes for each node the markers under which it is (`requirements.txt`, no markers provided on installation) or can be (`uv.lock`, depending on the markers provided on installation) included in the installation. Put differently: If the marker computed by `marker_reachability` is not fulfilled for the current platform, the package is never required on the current platform.

We compute the markers for each package in the graph, this includes the virtual extra packages and the base packages. Since we know that each virtual extra package depends on its base package (`foo[bar]` implied `foo`), we only retain the base package marker in the `requirements.txt` graph.

In #6959/#6961 we were only using it for pruning packages in `uv.lock`, now we're also using it for the markers in `requirements.txt`.

I think this closes #4645, CC @bluss.

---

_Label `internal` added by @konstin on 2024-09-05 10:54_

---

_Review requested from @BurntSushi by @konstin on 2024-09-05 10:54_

---

_Review requested from @charliermarsh by @konstin on 2024-09-05 10:54_

---

_@konstin reviewed on 2024-09-05 10:55_

---

_Review comment by @konstin on `crates/uv-resolver/src/graph_ops.rs`:15 on 2024-09-05 10:55_

This function only moved.

---

_Converted to draft by @konstin on 2024-09-05 11:05_

---

_Marked ready for review by @konstin on 2024-09-05 11:15_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/graph_ops.rs`:15 on 2024-09-05 13:09_

If possible I might suggest splitting these moves out into separate commits in the future. Otherwise it can be a little tricky to see which parts of the diff are "uninteresting moves" versus what is an interesting change.

(It can be hard to always do this though, especially if you do the move mid-refactor.)

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/requirements_txt.rs`:204 on 2024-09-05 13:19_

Nice!

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/display.rs`:339 on 2024-09-05 13:22_

This seems like an interesting insight that might be worth copying over to the docs on `PubGrubPackage`. Specifically, is it a guaranteed invariant that for any package `foo`, if there is both a `PubGrubPackage::Package` and a `PubGrubPackage::Marker` for it, then the former is always a superset of the latter?

(I'd love to start collecting invariants in the docs for `PubGrubPackage` because I find that type somewhat difficult to reason about. But I don't have the full shape in my head yet to propose something better.)

---

_@BurntSushi approved on 2024-09-05 13:23_

This makes sense to me.

> Since we know that each virtual extra package depends on its base package (foo[bar] implied foo), we only retain the base package marker in the requirements.txt graph.

Does this lead to any bug fixes? Or is it "just" a simplification?

---

_@charliermarsh approved on 2024-09-05 13:57_

Happy with this as long as tests pass. Thanks!

---

_Review comment by @konstin on `crates/uv-resolver/src/graph_ops.rs`:15 on 2024-09-05 16:37_

Good call: #7091

---

_@konstin reviewed on 2024-09-05 16:37_

---

_Comment by @konstin on 2024-09-05 16:40_

>> Since we know that each virtual extra package depends on its base package (foo[bar] implied foo), we only retain the base package marker in the requirements.txt graph.
>
> Does this lead to any bug fixes? Or is it "just" a simplification?

This is intrinsic to how we handle markers and to marker propagation. This property held true before and after, though the algorithm we arrived at it changed and i made it more explicit.

---

_@konstin reviewed on 2024-09-05 16:44_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/display.rs`:339 on 2024-09-05 16:44_

I have a hard time documenting this because "the base package is always selected when one of its virtual packages is selected" seems so fundamental to me. Do we have a good place to put overall resolver things? Because I think it would be valuable to write some of them up, we bet we come across some improvements to uv by this way.

---

_Merged by @konstin on 2024-09-05 16:52_

---

_Closed by @konstin on 2024-09-05 16:52_

---

_Branch deleted on 2024-09-05 16:52_

---

_@BurntSushi reviewed on 2024-09-05 17:26_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/display.rs`:339 on 2024-09-05 17:26_

I'm not sure that was always even true, was it? The way virtual packages get added has shifted around a lot I think. Particularly in `get_dependencies` in the resolver. I don't think anything fundamental has changed in ~months, but I know @charliermarsh did a lot of work there.

I would start by adding these sorts of things to `PubGrubPackage` I think. It is a very key data type in the resolver and I _think_ there are a lot of interesting invariants around it.

---
