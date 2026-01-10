```yaml
number: 21537
title: "[ty] Add tests: `types.UnionType` in `isinstance`/`issubclass`"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/isinstance-uniontype
created_at: 2025-11-20T11:07:59Z
updated_at: 2025-11-20T11:59:37Z
url: https://github.com/astral-sh/ruff/pull/21537
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add tests: `types.UnionType` in `isinstance`/`issubclass`

---

_Pull request opened by @sharkdp on 2025-11-20 11:07_

## Summary

Add some tests documenting the fact that we don't support `types.UnionType` in `isinstance`/`issubclass` at the moment.


---

_Label `testing` added by @sharkdp on 2025-11-20 11:08_

---

_Label `ty` added by @sharkdp on 2025-11-20 11:08_

---

_Review requested from @carljm by @sharkdp on 2025-11-20 11:08_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-20 11:08_

---

_Review requested from @dcreager by @sharkdp on 2025-11-20 11:08_

---

_Comment by @sharkdp on 2025-11-20 11:08_

Will fix this in one of my next PRs, or otherwise create an issue for it.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/isinstance.md`:169 on 2025-11-20 11:41_

oof ouch that's a confusing diagnostic -- we're just saying things that are blatantly untrue 😆

<img width="2176" height="534" alt="image" src="https://github.com/user-attachments/assets/453e2c89-8922-40cf-aa5e-08af50013339" />

I totally missed this edge case in https://github.com/astral-sh/ruff/pull/21343 -- thank you!!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/isinstance.md`:152 on 2025-11-20 11:42_

```suggestion
Python 3.10 added the ability to use `Union[int, str]` as the second argument to `isinstance()`:
```

---

_@AlexWaygood approved on 2025-11-20 11:42_

Thank you!

---

_Merged by @sharkdp on 2025-11-20 11:59_

---

_Closed by @sharkdp on 2025-11-20 11:59_

---

_Branch deleted on 2025-11-20 11:59_

---
