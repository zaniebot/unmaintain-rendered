```yaml
number: 17797
title: "[red-knot] Avoid including `__all__` in the exported names"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: dhruv/avoid-exporting-dunder-all
created_at: 2025-05-02T19:53:21Z
updated_at: 2025-05-07T15:18:07Z
url: https://github.com/astral-sh/ruff/pull/17797
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] Avoid including `__all__` in the exported names

---

_Pull request opened by @dhruvmanila on 2025-05-02 19:53_

## Summary

This PR fixes a bug where the `__all__` was being included in the exported names which would lead to a hang on the following example.

The `__all__` variable is not exported from a `*`-import.

<details><summary>Example that hangs on <code>main</code></summary>
<p>

`play.py`:
```py
from submodule import *
```

`submodule/__init__.py`:
```py
from submodule import foo

reveal_type(foo)

__all__ = []
__all__ += foo.__all__
```

`submodule/foo/__init__.py`:
```py
__all__ = ["SubmoduleFoo"]


class SubmoduleFoo: ...
```

</p>
</details> 

## Test Plan

Update one of the existing star import test case to make sure `__all__` creates an unresolved-reference error and the inferred type is `Unknown`.

On `main`, this doesn't result into any error and the inferred type is `Unknown | list`.


---

_Label `bug` added by @dhruvmanila on 2025-05-02 19:53_

---

_Label `red-knot` added by @dhruvmanila on 2025-05-02 19:53_

---

_Review requested from @carljm by @dhruvmanila on 2025-05-02 19:53_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-05-02 19:53_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-05-02 19:53_

---

_Review requested from @dcreager by @dhruvmanila on 2025-05-02 19:53_

---

_Comment by @github-actions[bot] on 2025-05-02 19:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-05-02 20:00_

> The `__all__` variable is not exported from a `*`-import.

it is if `__all__` itself includes `"__all__"`: https://playknot.ruff.rs/20c9ff08-2415-4932-8a13-83b60706350d

---

_Review request for @carljm removed by @carljm on 2025-05-02 20:04_

---

_Comment by @dhruvmanila on 2025-05-02 20:08_

> it is if `__all__` itself includes `"__all__"`: [playknot.ruff.rs/20c9ff08-2415-4932-8a13-83b60706350d](https://playknot.ruff.rs/20c9ff08-2415-4932-8a13-83b60706350d)

Hmm, so this is not the correct fix, we need `__all__` support here

---

_Converted to draft by @dhruvmanila on 2025-05-02 20:09_

---

_Comment by @dhruvmanila on 2025-05-07 03:21_

Closing in favor of astral-sh/ty#106

---

_Closed by @dhruvmanila on 2025-05-07 03:21_

---
