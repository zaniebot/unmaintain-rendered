```yaml
number: 3053
title: Improve error message for uv pip compile when input requirements conflict
type: issue
state: closed
author: hauntsaninja
labels:
  - error messages
assignees: []
created_at: 2024-04-16T07:16:06Z
updated_at: 2024-04-16T07:57:14Z
url: https://github.com/astral-sh/uv/issues/3053
synced_at: 2026-01-10T05:40:32Z
```

# Improve error message for uv pip compile when input requirements conflict

---

_Issue opened by @hauntsaninja on 2024-04-16 07:16_

```
λ cat r.txt
pypyp==1,>=1.2
λ uv pip compile r.txt
  × No solution found when resolving dependencies:
  ╰─▶ you require ∅
```
It would be nice for uv to mention the name of the package.

For instance, in other scenarios, uv will say something like:
```
λ uv pip compile r.txt -c c.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because you require pypyp==1 and you require pypyp>=1.2, we can conclude that the requirements are unsatisfiable.
```

This comes up when the input to `uv pip compile` is dynamically generated, so it's not obvious that you have a conflicting requirement.

---

_Label `error messages` added by @konstin on 2024-04-16 07:49_

---

_Closed by @konstin on 2024-04-16 07:57_

---
