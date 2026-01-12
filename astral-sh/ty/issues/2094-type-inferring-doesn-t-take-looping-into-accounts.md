```yaml
number: 2094
title: "type inferring doesn't take looping into accounts, inaccurately narrows to `Literal`"
type: issue
state: closed
author: paw-lu
labels: []
assignees: []
created_at: 2025-12-18T23:38:53Z
updated_at: 2025-12-18T23:41:33Z
url: https://github.com/astral-sh/ty/issues/2094
synced_at: 2026-01-12T15:54:26Z
```

# type inferring doesn't take looping into accounts, inaccurately narrows to `Literal`

---

_@paw-lu_

### Summary

```python
x: int = 2

for _ in range(20):
    x += 2
    y = x
    reveal_type(y)

z = x
reveal_type(z)
```

```txt
Revealed type: `Literal[4]` (revealed-type) [Ln 6, Col 17]
Revealed type: `Literal[2, 4]` (revealed-type) [Ln 9, Col 13]
```

[Here ty infers `y` as `Literal[2]`, and `z` as `Literal[2, 4]`](https://play.ty.dev/d163fcbd-88ef-410f-8123-bea867f9647f). 

<img width="269" height="204" alt="Image" src="https://github.com/user-attachments/assets/e11e91d3-c31a-44dc-983e-efbeff9ab413" />

Ideally these would just stay as `int` (right?). Don't really expect ty do do the math here and figure out what the final value of x is, but was surprised to see it narrow down the `int` to a `Literal` here.

---

ty has been awesome so far—thanks a lot for the project!


### Version

0.0.3

---

_Comment by @paw-lu on 2025-12-18 23:40_

This issue seems to be a duplicate, my bad here!

- https://github.com/astral-sh/ty/issues/1715
- https://github.com/astral-sh/ty/issues/232

---

_Closed by @paw-lu on 2025-12-18 23:40_

---

_Reopened by @paw-lu on 2025-12-18 23:41_

---

_Closed by @paw-lu on 2025-12-18 23:41_

---
