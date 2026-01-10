```yaml
number: 19512
title: "[ty] Fix narrowing and reachability of class patterns with arguments"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-class-patterns
created_at: 2025-07-23T15:15:14Z
updated_at: 2025-07-23T16:45:05Z
url: https://github.com/astral-sh/ruff/pull/19512
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Fix narrowing and reachability of class patterns with arguments

---

_Pull request opened by @sharkdp on 2025-07-23 15:15_

## Summary

I noticed that our type narrowing and reachability analysis was incorrect for class patterns that are not irrefutable. The test cases below compare the old and the new behavior:

```py
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int

class Other: ...

def _(target: Point):
    y = 1

    match target:
        case Point(0, 0):
            y = 2
        case Point(x=0, y=1):
            y = 3
        case Point(x=1, y=0):
            y = 4
    
    reveal_type(y)  # revealed: Literal[1, 2, 3, 4]    (previously: Literal[2])


def _(target: Point | Other):
    match target:
        case Point(0, 0):
            reveal_type(target)  # revealed: Point
        case Point(x=0, y=1):
            reveal_type(target)  # revealed: Point    (previously: Never)
        case Point(x=1, y=0):
            reveal_type(target)  # revealed: Point    (previously: Never)
        case Other():
            reveal_type(target)  # revealed: Other    (previously: Other & ~Point)
```

## Test Plan

New Markdown test


---

_Label `ty` added by @sharkdp on 2025-07-23 15:15_

---

_Comment by @github-actions[bot] on 2025-07-23 15:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `bug` added by @sharkdp on 2025-07-23 15:21_

---

_Marked ready for review by @sharkdp on 2025-07-23 15:42_

---

_Review requested from @carljm by @sharkdp on 2025-07-23 15:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-23 15:42_

---

_Review requested from @dcreager by @sharkdp on 2025-07-23 15:42_

---

_@carljm approved on 2025-07-23 15:47_

Thank you!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-23 16:03_

---

_Merged by @sharkdp on 2025-07-23 16:45_

---

_Closed by @sharkdp on 2025-07-23 16:45_

---

_Branch deleted on 2025-07-23 16:45_

---
