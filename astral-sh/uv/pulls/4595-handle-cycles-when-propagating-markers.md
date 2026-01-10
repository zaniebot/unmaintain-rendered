```yaml
number: 4595
title: Handle cycles when propagating markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cycle
created_at: 2024-06-27T18:19:46Z
updated_at: 2024-06-29T09:25:57Z
url: https://github.com/astral-sh/uv/pull/4595
synced_at: 2026-01-10T13:48:28Z
```

# Handle cycles when propagating markers

---

_Pull request opened by @charliermarsh on 2024-06-27 18:19_

## Summary

It turns out that `Topo` only works on graphs without cycles. If a graph has a cycle, it seems to bail early. So we were losing markers for trees that contain cycles (like Poetry, which depends on `poetry-plugin-export`, which depends on Poetry).

Now, we remove cycles beforehand and re-add those edges afterwards.

It's a bit hard for me to reason about the implications of this. The way that marker propagation works is that we do visit the nodes in-order and propagate the markers from any incoming to any outgoing edges. We only do this at a single depth (rather than recursively) because we visit the nodes in-order anyway. But if you have a cycle... then in theory you might need to propagate the markers recursively? Or maybe not?

As an example:

`A -> B -> C -> D -> B`

If `A -> B` has `sys_platform == 'darwin'`, and then `D -> B` has `python_version >= '3.7`... then we don't need to propagate `python_version >= '3.7'` back to `B` or any of its dependencies, because the condition would be `(sys_platform == 'darwin' or python_version >= '3.7) or sys_platform == 'darwin'`, which is equivalent to `sys_platform == 'darwin'`.

Closes #4584.


---

_Label `bug` added by @charliermarsh on 2024-06-27 18:19_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-27 18:19_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-27 18:19_

---

_Comment by @charliermarsh on 2024-06-27 19:34_

The trio resolution changed; need to check if it was incorrect before or is incorrect now.

---

_Comment by @charliermarsh on 2024-06-27 20:56_

Ok, this is a clear improvement, but I'm going to look into doing something more rigorous.

---

_@zanieb approved on 2024-06-27 21:13_

---

_Comment by @zanieb on 2024-06-27 21:14_

This looks like a pretty reasonable start.

---

_Merged by @charliermarsh on 2024-06-27 21:30_

---

_Closed by @charliermarsh on 2024-06-27 21:30_

---

_Branch deleted on 2024-06-27 21:30_

---

_@BurntSushi reviewed on 2024-06-28 13:14_

LGTM!

I can't immediately see where your approach would fail. Maybe something to do with optional dependencies? But I can't think of a way that can go wrong.

---

_Comment by @charliermarsh on 2024-06-28 13:37_

I really wish there was a way to do this without modifying the graph structure... I've thought a lot about it and I can't find a way to do it with a traditional BFS or DFS.

---

_Review comment by @bluss on `crates/uv-resolver/src/resolution/display.rs`:369 on 2024-06-28 20:58_

Hm is this maybe not a a StableGraph so not safe to remove edge ids like this probably, unless you sort the edge ids?

This is a drive by comment, but petgraph caught my attention (even though it doesn't do that much anymore, sorry about that :slightly_frowning_face: )

About your general questions in this PR - the back of my mind says reverse postorder to do something similar to Topo but accomodating cycles to some extent. Petgraph has DfsPostOrder among other things. (To reverse it, its output must be collected and then reversed.)

An algorithm like scc https://docs.rs/petgraph/latest/petgraph/algo/fn.kosaraju_scc.html  finds both the sccs (they are the cyclic parts) as well as a topological order between each component too, which is not cyclic.

---

_@bluss reviewed on 2024-06-28 20:58_

---

_@charliermarsh reviewed on 2024-06-28 23:51_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/display.rs`:369 on 2024-06-28 23:51_

Oh that’s a very smart comment — the edge IDs could change, right!

I would love help with this problem BTW…

---

_@bluss reviewed on 2024-06-29 09:25_

---

_Review comment by @bluss on `crates/uv-resolver/src/resolution/display.rs`:369 on 2024-06-29 09:25_

Well, I am acutely aware of petgraph's shortcomings but I'm happy that it's still useful. You managed to snipe me, so I've made a try :slightly_smiling_face: 

---
