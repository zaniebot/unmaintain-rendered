```yaml
number: 1704
title: Inverse tree query
type: issue
state: open
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2025-12-01T12:36:37Z
updated_at: 2025-12-02T11:59:56Z
url: https://github.com/astral-sh/ty/issues/1704
synced_at: 2026-01-10T01:58:59Z
```

# Inverse tree query

---

_Issue opened by @MichaReiser on 2025-12-01 12:36_

Implement a salsa query that, given a file, returns an index that allows querying the ancestor chain of any given node. 

Being able to retrieve the ancestors (or parents) is often required when:

* generating fixes
* In LSP methods, e.g. `CoveringNode` is all about having a node + its ancestor chain


The easiest solution is to build a `ParentMap` that stores an `IndexVec<NodeId, NodeId>`: the index is the node, and the value is its parent. This should be very fast to build, but requires `O(n)` storage. It might be possible to store the information in a more condensed form by using the information that the ids are assigned pre-order (the ids of children are always larger than the parent, I think we do something similar for `scopes` in the `SemanticIndex`


The new data structure should provide methods to:

* get a node's parent
* get a node's ancestor



---

_Label `help wanted` added by @MichaReiser on 2025-12-01 12:36_

---

_Label `help wanted` removed by @MichaReiser on 2025-12-01 13:14_

---

_Comment by @Gankra on 2025-12-01 14:18_

Is this showing up as a performance bottleneck somewhere?

---

_Comment by @MichaReiser on 2025-12-01 14:45_

No, it's more that there are cases in type inference where we can't get the necessary information because we lack the ability to do upward traversal.

I'm not sure if performance is an issue in find references where we clone ancestors for every visited node but there are other ways we can mitigate that cost

---

_Comment by @Gankra on 2025-12-01 14:48_

Ah I see, I knew we did it all the time in the LSP, I didn't realize we were flaunting our AST privilege!

---

_Label `internal` added by @carljm on 2025-12-02 00:47_

---

_Comment by @11happy on 2025-12-02 11:52_

@MichaReiser can I work on this , this seems veryy interesting to me.
Thank you : )

---

_Comment by @MichaReiser on 2025-12-02 11:56_

I removed the help wanted label because it's not entirely clear to me what the API should be. Specifically, I ran into issues when using this new api in `CoveringNode` where it ended up being awkward. That's why I don't think it's a good candidate for a contributor issue at the moment. I'm sorry

---

_Comment by @11happy on 2025-12-02 11:59_

Sure no worries , I understand, however I am still interested to work on this whenever things are more clear. 
Thank  you : )

---
