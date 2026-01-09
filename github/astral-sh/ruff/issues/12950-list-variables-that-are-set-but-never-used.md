---
number: 12950
title: List variables that are set but never used
type: issue
state: closed
author: Beliavsky
labels: []
assignees: []
created_at: 2024-08-17T13:08:09Z
updated_at: 2024-08-19T10:27:54Z
url: https://github.com/astral-sh/ruff/issues/12950
synced_at: 2026-01-07T13:12:15-06:00
---

# List variables that are set but never used

---

_Issue opened by @Beliavsky on 2024-08-17 13:08_

For the code

```
a = 10
b = 20
c = 30
print(b)
```
ruff is silent. Here is a function using the `ast` module that warns about variables that are set but not used later.

```
import ast

class AssignmentVisitor(ast.NodeVisitor):
    def __init__(self):
        self.assigned = set()
        self.used = set()

    def visit_Assign(self, node):
        # Record all the variables assigned to
        for target in node.targets:
            if isinstance(target, ast.Name):
                self.assigned.add(target.id)
            elif isinstance(target, ast.Tuple):
                for element in target.elts:
                    if isinstance(element, ast.Name):
                        self.assigned.add(element.id)
        self.generic_visit(node)

    def visit_Name(self, node):
        # Record all the variables that are used
        if isinstance(node.ctx, ast.Load):
            self.used.add(node.id)
        self.generic_visit(node)

def find_unused_variables(code):
    tree = ast.parse(code)
    visitor = AssignmentVisitor()
    visitor.visit(tree)
    return visitor.assigned - visitor.used

# Example usage
code = """
a = 10
b = 20
c = 30
print(b)
"""
    
unused_vars = find_unused_variables(code)
if unused_vars:
    print("Unused variables:", unused_vars)
else:
    print("No unused variables")
```
output:

`Unused variables: {'a', 'c'}`

Could such functionality be added to ruff? Thanks for the useful tool.

---

_Comment by @tjkuson on 2024-08-17 13:51_

Ruff will detect that via [unused-variable (F841)](https://docs.astral.sh/ruff/rules/unused-variable/) if the unused names are in a local scope; for example,

```python
def fn() -> None:
    a = 10
    b = 20
    c = 30
    print(b)
```

If the names are globally scoped, Ruff can't determine if a name is used in a different file (see #7447).

---

_Comment by @charliermarsh on 2024-08-18 22:31_

Yeah, I think this is by design at the moment.

---

_Closed by @charliermarsh on 2024-08-18 22:31_

---
