```yaml
number: 137
title: Implement E741
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: E741
created_at: 2022-09-10T06:15:58Z
updated_at: 2022-09-11T16:30:28Z
url: https://github.com/astral-sh/ruff/pull/137
synced_at: 2026-01-12T05:48:45Z
```

# Implement E741

---

_Pull request opened by @harupy on 2022-09-10 06:15_

Test:

```bash
> flake8 --version
5.0.4 (mccabe: 0.7.0, pycodestyle: 2.9.1, pyflakes: 2.5.0) CPython 3.8.5 on Linux

> flake8 --select E741 resources/test/fixtures/E741.py
resources/test/fixtures/E741.py:3:1: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:4:1: E741 ambiguous variable name 'I'
resources/test/fixtures/E741.py:5:1: E741 ambiguous variable name 'O'
resources/test/fixtures/E741.py:8:4: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:10:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:11:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:16:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:25:12: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:26:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:30:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:33:18: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:34:9: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:40:8: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:49:16: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:63:22: E741 ambiguous variable name 'l'

> cargo run -- --select=E741 resources/test/fixtures/E741.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/ruff --select=E741 resources/test/fixtures/E741.py`
resources/test/fixtures/E741.py:3:1: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:4:1: E741 ambiguous variable name 'I'
resources/test/fixtures/E741.py:5:1: E741 ambiguous variable name 'O'
resources/test/fixtures/E741.py:6:1: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:8:4: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:9:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:10:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:11:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:16:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:20:8: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:25:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:26:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:30:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:33:9: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:34:9: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:40:8: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:49:16: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:58:20: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:63:1: E741 ambiguous variable name 'l'
```

---

_@harupy reviewed on 2022-09-10 07:43_

---

_Review comment by @harupy on `resources/test/fixtures/E741.py`:14 on 2022-09-10 07:43_

https://github.com/PyCQA/pycodestyle/blob/10a4427c75740717b43448339fcf71f11bc33d1a/pycodestyle.py#L1482

```
@register_check
def ambiguous_identifier(logical_line, tokens):
    r"""Never use the characters 'l', 'O', or 'I' as variable names.

    In some fonts, these characters are indistinguishable from the
    numerals one and zero. When tempted to use 'l', use 'L' instead.

    Okay: L = 0
    Okay: o = 123
    Okay: i = 42
    E741: l = 0
    E741: O = 123
    E741: I = 42

    Variables can be bound in several other contexts, including class
    and function definitions, 'global' and 'nonlocal' statements,
    exception handlers, and 'with' and 'for' statements.
    In addition, we have a special handling for function parameters.

    Okay: except AttributeError as o:
    Okay: with lock as L:
    Okay: foo(l=12)
    Okay: for a in foo(l=12):
    E741: except AttributeError as O:
    E741: with lock as l:
    E741: global I
    E741: nonlocal l
    E741: def foo(l):
    E741: def foo(l=12):
    E741: l = foo(l=12)
    E741: for l in range(10):
    E742: class I(object):
    E743: def l(x):
    """
```

Looks like I need to add more test cases.

---

_@charliermarsh reviewed on 2022-09-10 16:41_

---

_Review comment by @charliermarsh on `src/message.rs`:23 on 2022-09-10 16:41_

Oof, thanks!

---

_@charliermarsh reviewed on 2022-09-10 16:43_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:408 on 2022-09-10 16:43_

Do you have a sense for how many of these would be caught by instead hooking into `handle_node_store`? Could it help reduce the number of cases we have to handle?

---

_@harupy reviewed on 2022-09-11 05:30_

---

_Review comment by @harupy on `src/message.rs`:23 on 2022-09-11 05:30_

@charliermarsh I opened https://github.com/charliermarsh/ruff/pull/152 to fix this line.

---

_@harupy reviewed on 2022-09-11 12:57_

---

_Review comment by @harupy on `src/check_ast.rs`:408 on 2022-09-11 12:57_

> Do you have a sense for how many of these would be caught by instead hooking into handle_node_store?

@charliermarsh Thanks for the comment. By `hooking into handle_node_store`, you meant something like below? 

```rust
    fn handle_node_store(&mut self, expr: &Expr, parent: Option<&Stmt>) {
        if let ExprKind::Name { id, .. } = &expr.node {
            ...

            if self.settings.select.contains(&CheckCode::E741)
                && checks::is_ambiguous_name(id)
            {
                self.checks.push(Check::new(
                    CheckKind::AmbiguousVariableName(id.to_string()),
                    expr.location,
                ));
            }
```

---

_Comment by @harupy on 2022-09-11 13:59_

```
> cargo run --select=E741, resources/test/fixtures/E741.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/ruff resources/test/fixtures/E741.py`
resources/test/fixtures/E741.py:3:1: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:4:1: E741 ambiguous variable name 'I'
resources/test/fixtures/E741.py:5:1: E741 ambiguous variable name 'O'
resources/test/fixtures/E741.py:6:1: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:8:4: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:9:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:10:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:11:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:16:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:20:8: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:25:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:26:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:30:5: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:33:9: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:34:9: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:40:8: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:40:14: E741 ambiguous variable name 'I'
resources/test/fixtures/E741.py:44:8: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:44:16: E741 ambiguous variable name 'I'
resources/test/fixtures/E741.py:48:9: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:48:14: E741 ambiguous variable name 'I'
resources/test/fixtures/E741.py:57:16: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:66:20: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:71:1: E741 ambiguous variable name 'l'  <---
resources/test/fixtures/E741.py:71:1: E741 ambiguous variable name 'l'  <---
```

It looks like `handle_node_store` is called twice on the same except handler alias:

```python
# resources/test/fixtures/E741.py

...

try:
    pass
except ValueError as l:
    #                ^ this
    pass
```

---

_@charliermarsh reviewed on 2022-09-11 14:19_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:408 on 2022-09-11 14:19_

Yeah that's roughly what I was suggesting!

---

_Comment by @charliermarsh on 2022-09-11 14:22_

Hmm yeah, it is... if you look at `visit_excepthandler`, we do call it twice. That was based on the PyFlakes logic [here](https://github.com/PyCQA/pyflakes/blob/master/pyflakes/checker.py#L2227). Perhaps we can just get rid of the second `handle_node_store`. The current version, at least, is buggy in that `let definition = scope.values.get(name).cloned();` should be removing the name from the scope.


---

_Comment by @charliermarsh on 2022-09-11 14:22_

What if we instead put the lint rule logic in the `ExprContext::Store` block here?

```
ExprKind::Name { ctx, .. } => match ctx {
    ExprContext::Load => self.handle_node_load(expr),
    ExprContext::Store => {
        let parent =
            self.parents[*(self.parent_stack.last().expect("No parent found."))];
        self.handle_node_store(expr, Some(parent));
    }
    ExprContext::Del => self.handle_node_delete(expr),
},
```

...instead of in the `handle_node_store` function.


---

_@charliermarsh approved on 2022-09-11 16:30_

Great, thank you!

---

_Merged by @charliermarsh on 2022-09-11 16:30_

---

_Closed by @charliermarsh on 2022-09-11 16:30_

---
