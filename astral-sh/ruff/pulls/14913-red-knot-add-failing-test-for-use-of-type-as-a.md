```yaml
number: 14913
title: "[red-knot] Add failing test for use of `type[]` as a base class"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/failing-type-t-test
created_at: 2024-12-11T13:13:32Z
updated_at: 2024-12-11T17:08:02Z
url: https://github.com/astral-sh/ruff/pull/14913
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Add failing test for use of `type[]` as a base class

---

_Pull request opened by @AlexWaygood on 2024-12-11 13:13_

We support using `typing.Type[]` as a base class (and we have tests for it), but not yet `builtins.type[]`. At some point we should fix that, but I don't think it';s worth spending much time on now (and it might be easier once we've implemented generics?). This PR just adds a failing test with a TODO.

---

_Label `red-knot` added by @AlexWaygood on 2024-12-11 13:13_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-11 13:13_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-11 13:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-11 13:13_

---

_Comment by @github-actions[bot] on 2024-12-11 13:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2024-12-11 14:25_

There are also no inheritance tests for `tuple` just yet. #14916 should add one for `Tuple` though.

---

_Comment by @AlexWaygood on 2024-12-11 14:28_

> There are also no inheritance tests for `tuple` just yet. #14916 should add one for `Tuple` though.

If you could add one for `tuple` at the same time as you add them for `Tuple`, that would be great. It doesn't need to pass the way we'd "like it to"; it's fine for it to record the "wrong" result and have a big TODO comment next to it for now.

---

_@carljm approved on 2024-12-11 17:06_

---

_Merged by @AlexWaygood on 2024-12-11 17:08_

---

_Closed by @AlexWaygood on 2024-12-11 17:08_

---

_Branch deleted on 2024-12-11 17:08_

---
