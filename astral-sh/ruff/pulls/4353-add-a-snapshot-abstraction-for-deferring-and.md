```yaml
number: 4353
title: "Add a `Snapshot` abstraction for deferring and restoring visitor context"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/snapshot
created_at: 2023-05-10T16:17:50Z
updated_at: 2023-05-10T16:57:09Z
url: https://github.com/astral-sh/ruff/pull/4353
synced_at: 2026-01-12T15:55:15Z
```

# Add a `Snapshot` abstraction for deferring and restoring visitor context

---

_@charliermarsh_

## Summary

In the `Checker`, a common pattern we have is that we see something like a function definition, then we add it to a queue for deferred processing (since, e.g., we don't parse function bodies until we've parsed the rest of the containing scope). When we defer these traversals, we have to save some of the `Checker` state (e.g., the current scope), which we then restore later on.

This PR formalizes that pattern using a `Snapshot` struct, along with `Context::snapshot` and `Context::restore` methods.

Honestly, there's more state that we _should_ be saving and restoring in the `Snapshot`, namely all of the boolean flags. We should revisit those omissions, but it's no different than on `main` and it tends not to matter in practice.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-10 16:17_

---

_Review requested from @konstin by @charliermarsh on 2023-05-10 16:17_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/context.rs`:29 on 2023-05-10 16:19_

This is a combination of what was previously called `deferred::Context` and `ast::AnnotationContext`. We're now storing _more_ data than before, since we're now storing `in_type_checking_block` and `in_annotation` for all deferrals, whereas before, we only stored these for deferred type definitions. But I think it's wrong _not_ to snapshot and restore these.

---

_@charliermarsh reviewed on 2023-05-10 16:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2032 on 2023-05-10 16:19_

`visible_scope` and `scope.visibility` are going to be removed in a future PR, so I didn't both preserving them.

---

_@charliermarsh reviewed on 2023-05-10 16:19_

---

_Comment by @github-actions[bot] on 2023-05-10 16:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-05-10 16:42_

---

_Comment by @konstin on 2023-05-10 16:43_

comments i would add after reading, not sure where to put them:

```diff
diff --git a/crates/ruff_python_semantic/src/context.rs b/crates/ruff_python_semantic/src/context.rs
index 9b2fd4fb..f4a2d694 100644
--- a/crates/ruff_python_semantic/src/context.rs
+++ b/crates/ruff_python_semantic/src/context.rs
@@ -18,6 +18,7 @@ use crate::binding::{
 use crate::node::{NodeId, Nodes};
 use crate::scope::{Scope, ScopeId, ScopeKind, Scopes};
 
+/// A subset of a [`Context`] that we store for deferred processing
 #[derive(Clone, Copy, Debug, PartialEq, Eq)]
 pub struct Snapshot {
     scope_id: ScopeId,
diff --git a/crates/ruff_python_semantic/src/node.rs b/crates/ruff_python_semantic/src/node.rs
index 7d84e034..f009e529 100644
--- a/crates/ruff_python_semantic/src/node.rs
+++ b/crates/ruff_python_semantic/src/node.rs
@@ -30,6 +30,7 @@ impl From<NodeId> for usize {
     }
 }
 
+/// A node represents a statement and a link to its parent, if any
 #[derive(Debug)]
 struct Node<'a> {
     /// The statement this node represents.
```

---

_Merged by @charliermarsh on 2023-05-10 16:50_

---

_Closed by @charliermarsh on 2023-05-10 16:50_

---

_Branch deleted on 2023-05-10 16:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:23 on 2023-05-10 16:54_

Do we need all these implementations. E.g is it meaningful to compare to snapshot? From what I understand is that the Snapshot is likely to increase in size over time. Should we remove the Copy implementation? 

One benefit of removing the Copy implementation could also be to consume the snapshot in restore. That can be useful if applying the same snapshot should be discouraged 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:460 on 2023-05-10 16:55_

Nit: It could make sense to destructure the snapshot here so that rust will give us compile errors when adding new fields 

---

_@MichaReiser approved on 2023-05-10 16:55_

Neat!

---

_Comment by @charliermarsh on 2023-05-10 16:57_

Will address these in a small follow-up PR.

---
