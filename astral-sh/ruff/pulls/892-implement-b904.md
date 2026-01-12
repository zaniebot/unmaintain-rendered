```yaml
number: 892
title: Implement B904
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: B904
created_at: 2022-11-23T14:04:25Z
updated_at: 2022-11-24T14:50:02Z
url: https://github.com/astral-sh/ruff/pull/892
synced_at: 2026-01-12T05:48:46Z
```

# Implement B904

---

_Pull request opened by @harupy on 2022-11-23 14:04_

#389 

---

_@charliermarsh reviewed on 2022-11-23 14:52_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:2024 on 2022-11-23 14:52_

I'd prefer to find a way to implement this that doesn't require introducing a new scope, since the scope semantics are kinda tricky. I'd even prefer adding an `in_excepthandler: bool` to the `Checker` if that's easiest :)

---

_@harupy reviewed on 2022-11-23 15:28_

---

_Review comment by @harupy on `src/check_ast.rs`:2024 on 2022-11-23 15:28_

Got it. How about implementing a visitor that looks like this?

```rust
impl<'a> Visitor<'a> for RaiseVisitor {
    fn visit_stmt(&mut self, stmt: &'a Stmt) {
        match &stmt.node {
            StmtKind::Raise { exc, cause } => {
                if cause.is_none() {
                    if let Some(exc) = exc {
                        match &exc.node {
                            ExprKind::Name { id, .. } if is_lower(id) => {}
                            _ => {
                                self.checks.push(Check::new(
                                    CheckKind::RaiseWithoutFromInsideExcept,
                                    Range::from_located(stmt),
                                ));
                            }
                        }
                    }
                }
            }
            StmtKind::FunctionDef { .. }
            | StmtKind::AsyncFunctionDef { .. }
            | StmtKind::ClassDef { .. } => {}
            StmtKind::Try { .. } => {}
            StmtKind::If { body, .. }
            | StmtKind::With { body, .. }
            | StmtKind::AsyncWith { body, .. }
            | StmtKind::For { body, .. }
            | StmtKind::AsyncFor { body, .. }
            | StmtKind::While { body, .. } => {
                for stmt in body {
                    self.visit_stmt(stmt);
                }
            }
            _ => {}
        }
    }
}
```

---

_@harupy reviewed on 2022-11-23 15:45_

---

_Review comment by @harupy on `src/check_ast.rs`:2024 on 2022-11-23 15:45_

> I'd even prefer adding an in_excepthandler: bool to the Checker if that's easiest :

Can you elaborate on this approach?

---

_@charliermarsh reviewed on 2022-11-23 16:44_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:2024 on 2022-11-23 16:44_

Yeah that looks reasonable to me.

What I was suggesting was something like:

```rust
fn visit_excepthandler(&mut self, excepthandler: &'b Excepthandler) {
    prev_in_excepthandler = self.in_excepthandler;
    self.in_excepthandler = true;
    ...
    self.in_excepthandler = prev_in_excepthandler;
}
 ```

Then here, you could check `if self.in_excepthandler`. It's kind of a mess because we have to do that separate bookkeeping, but it does mean to can avoid multiple passes over the AST as would be required with the visitor.


---

_@harupy reviewed on 2022-11-24 02:42_

---

_Review comment by @harupy on `src/check_ast.rs`:2024 on 2022-11-24 02:42_

Does this approach work when a raise statement is in a function?

```python
except:
   def foo():
       raise Exception  # this is ok
```

---

_@charliermarsh reviewed on 2022-11-24 03:05_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:2024 on 2022-11-24 03:05_

Probably not, no!

---

_@charliermarsh reviewed on 2022-11-24 04:25_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:2024 on 2022-11-24 04:25_

(So I think the visitor you outlined above is probably a better choice.)

---

_@harupy reviewed on 2022-11-24 05:44_

---

_Review comment by @harupy on `src/check_ast.rs`:2024 on 2022-11-24 05:44_

Got it :) I'll push the code tonight.

---

_Merged by @charliermarsh on 2022-11-24 14:49_

---

_Closed by @charliermarsh on 2022-11-24 14:49_

---

_@charliermarsh reviewed on 2022-11-24 14:50_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:2024 on 2022-11-24 14:50_

Thanks!

---
