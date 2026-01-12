```yaml
number: 17588
title: "[red-knot] Trust all symbols in stub files"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/trust-undeclared-symbols-in-stubs-pt2
created_at: 2025-04-23T17:32:48Z
updated_at: 2025-04-23T18:07:31Z
url: https://github.com/astral-sh/ruff/pull/17588
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Trust all symbols in stub files

---

_@sharkdp_

## Summary

Follow-up on the discussion [here](https://github.com/astral-sh/ruff/pull/17577#discussion_r2055945909). *Generally* trust undeclared symbols in stubs, not just at the module level.

## Test Plan

New Markdown test.

---

_Label `red-knot` added by @sharkdp on 2025-04-23 17:32_

---

_Renamed from "[red-knot] Trust *all* symbols in stub files" to "[red-knot] Trust all symbols in stub files" by @sharkdp on 2025-04-23 17:35_

---

_Comment by @github-actions[bot] on 2025-04-23 17:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-23 17:45_

Oh, I suppose a lot of class-level assignments in typeshed are in the context of enum classes (e.g. https://github.com/python/typeshed/blob/ae57d69c9354147bcf7086d6db47af8f8c82bf7b/stdlib/re.pyi#L201-L221 ), and we infer all attributes on enums as `Todo` anyway...

---

_Comment by @sharkdp on 2025-04-23 17:49_

I found one example that we do infer differently on this branch! I'm sure it's *widely* used:
```py
from pdb import Pdb

reveal_type(Pdb.do_b)  # Previously: Unknown | def do_break(self, arg: str, temporary: bool = EllipsisType) -> bool | None
                       # Now:        def do_break(…) -> …
```

---

_Comment by @AlexWaygood on 2025-04-23 17:51_

This does still feel more consistent to me, so I do support it still!

---

_Marked ready for review by @sharkdp on 2025-04-23 17:57_

---

_Review requested from @carljm by @sharkdp on 2025-04-23 17:57_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-23 17:57_

---

_Review requested from @dcreager by @sharkdp on 2025-04-23 17:57_

---

_@carljm approved on 2025-04-23 18:02_

I think this makes sense as well. Sorry I didn't get through my notifications fast enough to comment on the previous PR sooner!

The core "gradual guarantee" idea that we should not emit false positives on working untyped code, when the user hasn't declared their intentions through type annotations, just doesn't apply to stub files in the same way, since they are inherently somebody explicitly providing type information.

---

_@AlexWaygood approved on 2025-04-23 18:04_

---

_Merged by @sharkdp on 2025-04-23 18:07_

---

_Closed by @sharkdp on 2025-04-23 18:07_

---

_Branch deleted on 2025-04-23 18:07_

---
