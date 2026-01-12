```yaml
number: 1009
title: "Implement `consider-merging-isinstance`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: consider-merging-isinstance
created_at: 2022-12-03T08:49:20Z
updated_at: 2022-12-03T21:51:54Z
url: https://github.com/astral-sh/ruff/pull/1009
synced_at: 2026-01-12T15:55:05Z
```

# Implement `consider-merging-isinstance`

---

_@harupy_

#970

---

_@harupy reviewed on 2022-12-03 08:56_

---

_Review comment by @harupy on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:20 on 2022-12-03 08:56_

<img width="927" alt="image" src="https://user-images.githubusercontent.com/17039389/205432886-b54be501-b7c3-479a-a3d1-774732fb358a.png">

I'm not sure if this is intended, but the start location of a `BoolOp` expression is the start location of `or`.

---

_@harupy reviewed on 2022-12-03 08:57_

---

_Review comment by @harupy on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:20 on 2022-12-03 08:57_

RustPython's code:

https://github.com/RustPython/RustPython/blob/18af44b7c7410c3448df1f3b080bd358e586d265/compiler/parser/python.lalrpop#L758

```
OrTest: ast::Expr = {
    <e1:AndTest> <location:@L> <e2:("or" AndTest)*> <end_location:@R> => {
        if e2.is_empty() {
            e1
        } else {
            let mut values = vec![e1];
            values.extend(e2.into_iter().map(|e| e.1));
            ast::Expr {
                location,
                end_location: Some(end_location),
                custom: (),
                node: ast::ExprKind::BoolOp { op: ast::Boolop::Or, values }
            }
        }
    },
};
```

---

_Review comment by @harupy on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:22 on 2022-12-03 09:01_

In the current implementation, this line doesn't emit an error.

---

_@harupy reviewed on 2022-12-03 09:01_

---

_@harupy reviewed on 2022-12-03 09:23_

---

_Review comment by @harupy on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:20 on 2022-12-03 09:23_

Moving `<location:@L>` before `<e1:AndTest>` seems to work?

https://github.com/harupy/RustPython/blob/3835575a831dafe48fa4ddbae895960a377f1227/compiler/parser/python.lalrpop#L759

```
OrTest: ast::Expr = {
    <location:@L> <e1:AndTest> <e2:("or" AndTest)*> <end_location:@R> => {
    ^^^^^^^^^^^^^
        if e2.is_empty() {
            e1
        } else {
            let mut values = vec![e1];
            values.extend(e2.into_iter().map(|e| e.1));
            ast::Expr {
                location,
                end_location: Some(end_location),
                custom: (),
                node: ast::ExprKind::BoolOp { op: ast::Boolop::Or, values }
            }
        }
    },
};
```


<img width="927" alt="image" src="https://user-images.githubusercontent.com/17039389/205433820-5679cb35-918d-4217-842b-d4bda29e03c1.png">


---

_@harupy reviewed on 2022-12-03 09:39_

---

_Review comment by @harupy on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:20 on 2022-12-03 09:39_

```python
import ast

# Visitor to print out node locations
class V(ast.NodeVisitor):
    def visit_BoolOp(self, node: ast.BoolOp) -> None:
        print(
            node.__class__.__name__,
            (node.lineno, node.col_offset),
            (node.end_lineno, node.end_col_offset),
        )
        self.generic_visit(node)

V().visit(ast.parse("a or b or c"))
```

prints out:

```
BoolOp (1, 0) (1, 11)
```

---

_Review comment by @charliermarsh on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:20 on 2022-12-03 15:00_

Oh cool. Do you want to submit an upstream PR for this? Or I can.

---

_@charliermarsh reviewed on 2022-12-03 15:00_

---

_@charliermarsh reviewed on 2022-12-03 15:35_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:22 on 2022-12-03 15:35_

Is that because the `and` is at the end? (Could we find subsequences of `or`s?)

---

_@harupy reviewed on 2022-12-03 20:46_

---

_Review comment by @harupy on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:22 on 2022-12-03 20:46_

It's because `isinstance` is assigned to `infered_isinstance`. Can ruff handle this?

---

_@harupy reviewed on 2022-12-03 20:47_

---

_Review comment by @harupy on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:20 on 2022-12-03 20:47_

Filed https://github.com/RustPython/RustPython/pull/4306.

---

_Review comment by @charliermarsh on `resources/test/fixtures/pylint/consider_merging_isinstance.py`:22 on 2022-12-03 21:27_

Oh duh! No, not right now.

---

_@charliermarsh reviewed on 2022-12-03 21:27_

---

_Merged by @charliermarsh on 2022-12-03 21:51_

---

_Closed by @charliermarsh on 2022-12-03 21:51_

---
