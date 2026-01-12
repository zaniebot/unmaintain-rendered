```yaml
number: 4645
title: Use cycle aware algorithm for propagating markers
type: pull_request
state: closed
author: bluss
labels: []
assignees: []
draft: true
base: main
head: propagate-markers
created_at: 2024-06-29T09:14:23Z
updated_at: 2024-09-05T16:52:22Z
url: https://github.com/astral-sh/uv/pull/4645
synced_at: 2026-01-12T16:06:22Z
```

# Use cycle aware algorithm for propagating markers

---

_@bluss_

This is my attempt at this - I don't exactly have much prior experience with
Uv's codebase and w.r.t graph algorithms I'm pretty rusty.
Let's look at this critically and try to find out if it does the right thing.

The strongly connected components algorithm both finds all cyclic
subgraphs, and a topological order of them. This seems to be a good
way to replace Topo with something cycle-aware.

The new algorithm uses the fact that in a strongly connected component,
all vertices reach each other through some edge. That means we can
combine markers of all internal edges in a component.

As an optimization, only outgoing edges pointing out of the current
component are updated. I didn't look outside this function, so don't
know if that's a reasonable choice.

There are useful pictures in this article:
https://en.wikipedia.org/wiki/Strongly_connected_component

## Test Plan

Using existing tests

---

_Comment by @Peiffap on 2024-06-29 17:02_

Any reason in particular you chose Kosaraju's algorithm over Tarjan's? I thought the latter was faster.

---

_@charliermarsh reviewed on 2024-06-29 17:04_

Thank you for looking at this -- I really appreciate it! I'm trying to reason through this with examples... Given this graph:

![Screenshot 2024-06-29 at 1 02 43 PM](https://github.com/astral-sh/uv/assets/1309177/d1ea1696-3cd7-4a2b-ba71-19a7f8f6bdb2)

I think we _want_ the marker on the blue node to be `A and B and E`, but with this strategy, would it be... `(A or B or C or D) and E`? Since we'd `OR` the edges into the scc along with all the edges within the scc?


---

_Comment by @bluss on 2024-06-29 17:16_

Oops, I haven't given and/or any thought at all, just moved existing code around, so you're probably right.

If we take that picture, but you had two edge at the start, A and A', both with the same target.

- Then you want to start with A or A'?
- Then you update with the cycle  (A or A') and B and C and D
- Then you update E to the following: (A or A') and B and C and D and E?
- Is this what you mean? Then I'd again need to make a difference between edge incoming to scc and edge inside the scc, which is easy to do.

---

_Comment by @charliermarsh on 2024-06-29 17:21_

Same picture but more colors:

![Screenshot 2024-06-29 at 1 19 34 PM](https://github.com/astral-sh/uv/assets/1309177/957addde-9fda-4958-8e19-1e2bac44476b)

Each edge is a marker condition, and we want each node to have the complete marker condition required for it to be included in a a resolution... So I think the desired end state here is:

- Yellow: A
- Red: A and B
- Green: A and B and C
- Blue: A and B and E

Yellow could also be written as: A or (A and B and C and D), but that just simplifies to A. In general, what I was trying to do was: edges into a node are `OR`'d, edges out of a node are `AND`'d.


---

_Comment by @bluss on 2024-06-29 17:27_

@Peiffap the reason was that this implementation is non-recursive so I know it don't have any stack issues. Don't know if it matters, though :slightly_smiling_face: 


---

_Comment by @bluss on 2024-06-29 17:32_

Thanks. So I'm trying to find the rule for the cycle.

Before we propagate, what's the node weight for Yellow, it's already A? And for Red, already B?

---

_Comment by @charliermarsh on 2024-06-29 17:34_

It starts off as `None` actually. Coming into the propagation, all the nodes have a `None` marker -- the markers only exist on the edges, but we want to compute the "full" marker for each node.

---

_Comment by @bluss on 2024-06-29 21:23_

There is no nice property for each component/cyclic part as a unit. I think the current code (feedback arc set) seems ok so far then. I don't know how it selects edges to remove, so if it makes the wrong choice the result could be incorrect?

Based on two cases I've looked at, reverse postorder seems to work well, it would skip the edges we want to skip? (No definition - probably doesn't hold up) I think your graph has exactly one initial node (one root), which makes this easier.

---

_Comment by @Peiffap on 2024-06-29 22:42_

I'm trying to follow along and understand what you are trying to do here (both with this particular implementation and the general propagation problem you are attempting to solve).

If I understand correctly, you are trying to find, for each node N in the graph, all possible paths starting from a root node (i.e. a node with no incoming edges) and ending in N, and then you want to combine the markers found along the various paths. Is that right?

---

_Comment by @charliermarsh on 2024-06-29 23:23_

> If I understand correctly, you are trying to find, for each node N in the graph, all possible paths starting from a root node (i.e. a node with no incoming edges) and ending in N, and then you want to combine the markers found along the various paths. Is that right?

Yes, that's right.

---

_Comment by @Peiffap on 2024-06-29 23:41_

Thanks for confirming.
Okay. What's stopping us from simply doing the following?

DFS from the root nodes, propagating markers along as long as there is no cycle; when there is a cycle, you simply backtrack without propagating (because the propagation would lead to something like (markers to A) or ((markers to A) and (markers in cycle)), which simplifies to (markers to A)).

---

_Comment by @bluss on 2024-06-30 08:22_

I was looking at this graph. It had to be a bit complicated to show that the simple approaches I was thinking of didn't work. (You could hope this graph is not realistic but I think it's a possible graph?)

Finding all paths without cycles in the path sounds right. For example, to the node 3 I think all these are valid:

- A, D
- B, C, F, G
- B, C, F, H, J, D

You could imagine the two last paths being realistic and relevant when the marker A is inactive?

It's probably an optimization to run the all paths search in each SCC separately. I think the cycle-free thing means that we can ask for that no path has a prefix that is another complete path (to that node). It's only necessary to find paths starting with the incident edges to the SCC (we should always have some edge incident, because the root is never in a cycle, hopefully?)

@Peiffap So if we run dfs from both node 2 and 4, then we find all the required paths from 2 and 4 to 3. I think that the dfs from the root (0) would not find all the required paths to 3.

<img src="https://github.com/astral-sh/uv/assets/3209739/7f563393-aa5a-410b-8024-4003db387199" width="500" />

<details>
<summary>graph in Rust</summary>

```rust
    let mut gr = petgraph::Graph::new();
    let n0 = gr.add_node("0");
    let n1 = gr.add_node("1");
    let n2 = gr.add_node("2");
    let n3 = gr.add_node("3");
    let n4 = gr.add_node("4");
    let n5 = gr.add_node("5");
    let n6 = gr.add_node("6");
    let n7 = gr.add_node("7");
    let n8 = gr.add_node("8");
    gr.add_edge(n0, n2, "A");
    gr.add_edge(n0, n1, "B");
    gr.add_edge(n2, n3, "D");
    gr.add_edge(n3, n6, "I");
    gr.add_edge(n3, n4, "E");
    gr.add_edge(n1, n4, "C");
    gr.add_edge(n4, n5, "F");
    gr.add_edge(n5, n6, "H");
    gr.add_edge(n6, n2, "J");
    gr.add_edge(n5, n7, "K");
    gr.add_edge(n6, n8, "L");
    gr.add_edge(n5, n3, "G");
```

</details>

---

_Comment by @Peiffap on 2024-06-30 11:14_

> (You could hope this graph is not realistic but I think it's a possible graph?)

I'm not sure, Charlie should have a clearer picture of the kinds of graphs we can run into here.

> Finding all paths without cycles in the path sounds right. For example, to the node 3 I think all these are valid:
> 
> * A, D
> * B, C, F, G
> * B, C, F, H, J, D
> 
> You could imagine the two last paths being realistic and relevant when the marker A is inactive?

I agree, they should all be considered I think.
 
> It's probably an optimization to run the all paths search in each SCC separately. I think the cycle-free thing means that we can ask for that no path has a prefix that is another complete path (to that node).

Yes, that's how I'm interpreting cycle-free too. Knowing an SCC basically allows us to say that node `N` in the SCC can be reached through `OR`ing all possible paths to any of the nodes in the SCC, where each path represents an `AND`ing of the edges in it, then from that node to `N`.
For example (SCC with nodes `A`, `B`, `N`): if we know all the paths to `A` and `B` and we know that there is an SCC, we can already add

    ((path 1 to `A`) or (path 2 to `A`) ... (path n_a to `A`) and `A -> N`) or 
    ((path 1 to `B`) or (path 2 to `B`) ... (path n_b to `B`) and `B -> N`)
to `N`.
The thing I'm not sure about is that you _need_ to do this -- as I see it, a proper DFS search (more on that later) should take care of discovering this already.

It's only necessary to find paths starting with the incident edges to the SCC (we should always have some edge incident, because the root is never in a cycle, hopefully?)

As I understand it, a node is a root node if and only if it has no incident edges. You shouldn't have any root nodes in cycles.

> So if we run dfs from both node 2 and 4, then we find all the required paths from 2 and 4 to 3. I think that the dfs from the root (0) would not find all the required paths to 3.
> 
> <img alt="" width="500" src="https://private-user-images.githubusercontent.com/3209739/344451575-7f563393-aa5a-410b-8024-4003db387199.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTk3NDQxMjcsIm5iZiI6MTcxOTc0MzgyNywicGF0aCI6Ii8zMjA5NzM5LzM0NDQ1MTU3NS03ZjU2MzM5My1hYTVhLTQxMGItODAyNC00MDAzZGIzODcxOTkucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDYzMCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDA2MzBUMTAzNzA3WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NGRiYjE4Y2NhMmQ2Mzc2NDM4MDQ5OTM1MDMwNWE2NzJmODYyZjViMWNiMGE5ODIyNzBjMWEyMzA2YjYzMzZkMyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ.I2E0BaKZ0dzCbirlgUYBcRR9lSO9LEeVwjXUn7vn_HI">

Thanks for making the example. I meant to use a modified version of DFS that only backtracks when a node is visited twice _in the current path_. I believe that would work, i.e. it would _eventually_ get the right answer, but I'm not sure what implications it has for complexity (we are effectively looking at all possible simple paths which is O(n!) for complete graphs... but that's kind of what we need to do?).


---

_Comment by @bluss on 2024-06-30 16:24_

I looked at computing all non repeating paths here: https://github.com/petgraph/petgraph/pull/650  It can be used on the whole graph or a subgraph (intention: used on one strongly connected component at a time.) I'm not sure if it does the useful thing we need, but it might.

I wanted to write the algorithm separately from the uv logic, I think it's better that way and easier to understand the code. (Not saying the code has to live in petgraph.)
In this case, paths are useful as `Vec<EdgeId>` (does this have a different name than path?) but in some other graph algorithms, paths are `Vec<NodeId>`.

> As I understand it, a node is a root node if and only if it has no incident edges. You shouldn't have any root nodes in cycles.

I meant the root of the graph in uv's code - the white node in @charliermarsh's picture and node 0 in my picture. IIUC, this is always present and it means any nontrivial SCC (has more than one node) does not contain the root and always has edges incident to the SCC (from the root or from other predecessor scc). This is useful in my example - we know that there are incident edges, we only need to compute paths from 2 and 4 in the scc?

---

_Comment by @charliermarsh on 2024-06-30 20:51_

@bluss - did you determine that doing what we did previously but with reverse-postorder rather than topo _doesn't_ work?

---

_Comment by @Peiffap on 2024-06-30 23:10_

That PR looks nice! I'll give it a more detailed look later, but it makes a lot of sense to me to focus on trails (i.e. no repeated edges) rather than paths (no repeated nodes).

I think you can make the assumption that the starting node never has incident edges; I believe it just represents a starting state, i.e. with no propagated markers. If that isn't the case yet for some reason, you should still be able to extend the graph by adding a new "root" node with no incident edges?

I think you're right about incident edges; for the example graph, 2 and 4 are the only possible entry points to the SCC, so you could limit yourself to computing the trails to those and then computing the trails starting in those to the rest of the SCC. Perhaps it also makes sense to only compute trails _to_ nodes that have outgoing edges from the SCC (i.e. 5 and 6), since all the "internal" nodes (only 3 in your example) in the SCC will be covered by the trails from (2 or 4) to (5 or 6)?

---

_Comment by @charliermarsh on 2024-06-30 23:11_

(One slight hitch is that we want to remove markers if you have an inbound edge that doesn't have any markers. Right now that's represented by `None`, but I'm considering using `MarkerTree::And(vec![])` (the `true` marker) so that I can differentiate between "nodes we haven't visited yet" and "nodes that have the `true` marker).)

---

_Comment by @Peiffap on 2024-06-30 23:27_

> if you have an inbound edge that doesn't have any markers

Do you mean an inbound edge from the root node, or any inbound edge? If it's the former and you apply the change you're discussing, wouldn't you end up with `true or ...` (where the ellipsis is all the other trails to that node) which should get simplified to just `true`?



---

_Comment by @charliermarsh on 2024-06-30 23:34_

> Do you mean an inbound edge from the root node, or any inbound edge? If it's the former and you apply the change you're discussing, wouldn't you end up with true or ... (where the ellipsis is all the other trails to that node) which should get simplified to just true?

Yes that's right -- inbound edge from the root node (propagated outward), so we could simplify at the end.

---

_Comment by @bluss on 2024-07-01 17:11_

@charliermarsh yes I think with my example graph it doesn't work to visit in that order.

---

_Comment by @bluss on 2024-07-16 17:35_

The complexity of the solution doesn't feel right, it feels unproportionally complicated. :slightly_smiling_face: 

I think we have an algorithm to do it now, using all_simple_paths_from https://github.com/petgraph/petgraph/pull/650

The choices for how to use node vs edge weights doesn't matter so much (can be changed?) but I think it would be good to use edge weights as read-only inputs and only update node weights.

- Visit each scc from an [SCCs](https://en.wikipedia.org/wiki/Strongly_connected_component) algorithm in topological sort order.
- If it's a single node scc, Update node weight = OR[incoming_weight(E) for incoming edges E]
- incoming_weight(E) = Source node of E's weight AND E weight
- If it's a nontrivial scc
  - Nodes with incoming edges (not from this scc) are input nodes.
  - We know this is not the graph root, so it has input nodes
  - Use `all_simple_paths_from`, restricted to the scc, starting from each input node
    - For each node N in the scc update node weight = OR[ path_weight(P)  for all paths P to N ]
    - path weight(P) = path_start(I) AND AND[edge weights in path P] where I is input node P started in
    - path_start(I) = OR[incoming_weight(E) for incoming edge E to input node I]

Example

- 0 and 1 are trivial single node sccs
- 2, 3, 4 is a scc (all nodes in this group can reach each other)

![Small example graph](https://github.com/user-attachments/assets/ffefb6e5-a57f-4539-9a32-4a75a5d66e91)


Compute

- Node 0 = A
- Node 1 = B
- Input nodes for SCC are 2, 3
- All paths from 2 are
  - To 3: [E]. To 4: [E, F]   (*)
- All paths from 3 are
  - To 4: [F]. To 2: [F, G]   (**)
- Now we do all the scc nodes.
- Node 2 = ((Node 0) AND C) OR ((Node 1) AND D) AND F AND G)
  - Note including path from (**) 
- Node 3 = ((Node 1) and D) OR ((Node 0) AND C AND E)
  - Note including path from (*)
- Node 4 = ((Node 0) AND C AND E AND F) OR ((Node 1) AND D AND F)



We process all paths from all input nodes together because all input nodes to an scc need to be considered in parallel. If there never was more than one input node, most of the complexity would be gone.


We have the following rules for how 'no marker'/None combines I believe: None OR X = None;  None AND X = X;

---

_Converted to draft by @bluss on 2024-07-16 17:36_

---

_Closed by @konstin on 2024-09-05 16:52_

---

_Closed by @konstin on 2024-09-05 16:52_

---
