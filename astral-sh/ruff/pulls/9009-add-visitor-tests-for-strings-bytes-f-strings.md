```yaml
number: 9009
title: Add visitor tests for strings, bytes, f-strings
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/visitor-tests
created_at: 2023-12-05T15:33:50Z
updated_at: 2023-12-06T16:52:23Z
url: https://github.com/astral-sh/ruff/pull/9009
synced_at: 2026-01-10T23:40:55Z
```

# Add visitor tests for strings, bytes, f-strings

---

_Pull request opened by @dhruvmanila on 2023-12-05 15:33_

This PR adds tests for visitor implementation for string literals, bytes literals and f-strings.


---

_Label `internal` added by @dhruvmanila on 2023-12-05 15:34_

---

_Comment by @dhruvmanila on 2023-12-05 15:34_

Current dependencies on/for this PR:
* main
  * **PR #9009** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9009?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #9025** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9025?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9009?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-12-05 15:50_

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

_Review comment by @MichaReiser on `crates/ruff_python_ast/tests/preorder.rs`:309 on 2023-12-06 00:16_

We can replace the implementations with:

```

impl RecordVisitor {
    fn emit(&mut self, text: &dyn Debug) {
        for _ in 0..self.depth {
            self.output.push_str("  ");
        }

        writeln!(self.output, "- {text:?}").unwrap();
    }
}

impl<'a> PreorderVisitor<'a> for RecordVisitor {
    fn enter_node(&mut self, node: AnyNodeRef<'a>) -> TraversalSignal {
        self.emit(&node.kind());
        self.depth += 1;

        TraversalSignal::Traverse
    }

    fn leave_node(&mut self, _node: AnyNodeRef<'a>) {
        self.depth -= 1;
    }

    fn visit_bool_op(&mut self, bool_op: &BoolOp) {
        self.emit(&bool_op);
    }

    fn visit_operator(&mut self, operator: &Operator) {
        self.emit(&operator);
    }

    fn visit_unary_op(&mut self, unary_op: &UnaryOp) {
        self.emit(&unary_op);
    }

    fn visit_cmp_op(&mut self, cmp_op: &CmpOp) {
        self.emit(&cmp_op);
    }
}
```

Which fixes some other bugs because of missing overrides.

---

_@MichaReiser approved on 2023-12-06 00:17_

Thanks

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/tests/preorder.rs`:309 on 2023-12-06 16:47_

Oh yes. I'll fix this in a follow-up PR. Thanks for the suggestion!

---

_@dhruvmanila reviewed on 2023-12-06 16:47_

---

_Merged by @dhruvmanila on 2023-12-06 16:52_

---

_Closed by @dhruvmanila on 2023-12-06 16:52_

---

_Branch deleted on 2023-12-06 16:52_

---
