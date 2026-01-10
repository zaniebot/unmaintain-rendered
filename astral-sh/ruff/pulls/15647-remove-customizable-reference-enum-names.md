```yaml
number: 15647
title: Remove customizable reference enum names
type: pull_request
state: merged
author: dcreager
labels:
  - internal
assignees: []
merged: true
base: main
head: dcreager/no-ref-type-override
created_at: 2025-01-21T15:52:26Z
updated_at: 2025-01-21T18:46:36Z
url: https://github.com/astral-sh/ruff/pull/15647
synced_at: 2026-01-10T20:05:43Z
```

# Remove customizable reference enum names

---

_Pull request opened by @dcreager on 2025-01-21 15:52_

The AST generator creates a reference enum for each syntax group — an enum where each variant contains a reference to the relevant syntax node.  Previously you could customize the name of the reference enum for a group — primarily because there was an existing `ExpressionRef` type that wouldn't have lined up with the auto-derived name `ExprRef`.  This follow-up PR is a simple search/replace to switch over to the auto-derived name, so that we can remove this customization point.


---

_Review requested from @carljm by @dcreager on 2025-01-21 15:52_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-21 15:52_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-21 15:52_

---

_Review requested from @sharkdp by @dcreager on 2025-01-21 15:52_

---

_Comment by @github-actions[bot] on 2025-01-21 16:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `internal` added by @MichaReiser on 2025-01-21 17:25_

---

_@MichaReiser approved on 2025-01-21 17:27_

I guess it's fine. My long term plan was to go from `Expr` to `Expression`, `Stmt` to `Statement` etc. That's why it was called `ExpressionRef`. I outlined some of my ideas in https://github.com/astral-sh/ruff/discussions/6183

Considering that I'll probably never have time to do this refactor in the short term, making it consistent seems reasonable.

---

_Comment by @dcreager on 2025-01-21 18:31_

> I guess it's fine. My long term plan was to go from `Expr` to `Expression`, `Stmt` to `Statement` etc. That's why it was called `ExpressionRef`. I outlined some of my ideas in #6183

Ah that's interesting, I hadn't seen that!  Skimming through your suggestions, I think I'm :+1: to pretty much all of them.  It would be a lot of churn in the diff to make those changes, but with the AST generator in place, it would at least be easier to implement them.

---

_Comment by @dcreager on 2025-01-21 18:32_

> It would be a lot of churn in the diff to make those changes, but with the AST generator in place, it would at least be easier to implement them.

(Which, to clarify, means that I'm suggesting agreement in doing them, but in follow-on PRs.  I still like this one as-is to at least move the current abbreviation-using names to be more consistent.)

---

_Merged by @dcreager on 2025-01-21 18:46_

---

_Closed by @dcreager on 2025-01-21 18:46_

---

_Branch deleted on 2025-01-21 18:46_

---
