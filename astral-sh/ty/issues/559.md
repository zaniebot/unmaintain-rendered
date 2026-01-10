```yaml
number: 559
title: "False positive with closures where variable is not reassigned (or del'd)"
type: issue
state: closed
author: hauntsaninja
labels:
  - control flow
assignees: []
created_at: 2025-06-01T02:24:54Z
updated_at: 2025-09-08T21:08:36Z
url: https://github.com/astral-sh/ty/issues/559
synced_at: 2026-01-10T02:06:24Z
```

# False positive with closures where variable is not reassigned (or del'd)

---

_Issue opened by @hauntsaninja on 2025-06-01 02:24_

```
def foo(x: str | None):
    if x is None:
        x = "asdf"

    def inner():
        return x + "foo"
```

pyright looks for reassignments following the closure definition in the flow graph. mypy does something similar (but its implementation is a little sketchier). This heuristic seems to work fairly well in practice. Was a very common cause of user confusion and duplicate reports before mypy added support for this.

---

_Comment by @hauntsaninja on 2025-06-01 02:27_

Hmm actually the unresolved reference one repros even when it's a parameter, so maybe this is a separate bug:
```
def foo(n_activations=0):
    def inner():
        nonlocal n_activations
        n_activations += 1
    inner()
    print(n_activations)
```

---

_Comment by @AlexWaygood on 2025-06-01 14:25_

Thanks! I think this may be a duplicate of (or at least overlap with) https://github.com/astral-sh/ty/issues/210?

---

_Label `control flow` added by @AlexWaygood on 2025-06-01 15:41_

---

_Comment by @carljm on 2025-06-02 15:52_

The second example is https://github.com/astral-sh/ty/issues/220

I think we should keep this open for the first example, though. It's related to #210 but I think the first version of #210 probably won't address this

---

_Added to milestone `GA` by @carljm on 2025-07-23 23:09_

---

_Comment by @carljm on 2025-07-23 23:09_

https://github.com/astral-sh/ruff/pull/19321 improves this for some cases, but still not for the case in the OP here, because it doesn't use a flow-sensitive approach related to where in the outer scope the nested scope is defined (cc @mtshiba)

---

_Closed by @carljm on 2025-09-08 21:08_

---
