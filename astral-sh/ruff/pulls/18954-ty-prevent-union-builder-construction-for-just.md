```yaml
number: 18954
title: "[ty] Prevent union builder construction for just one declaration"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - performance
  - ty
assignees: []
merged: true
base: main
head: david/prevent-builder-construction
created_at: 2025-06-26T10:30:11Z
updated_at: 2025-06-26T11:58:18Z
url: https://github.com/astral-sh/ruff/pull/18954
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Prevent union builder construction for just one declaration

---

_Pull request opened by @sharkdp on 2025-06-26 10:30_

## Summary

Avoid the construction of the `DeclaredTypeBuilder` if there is just one declared type.

---

_Review requested from @carljm by @sharkdp on 2025-06-26 10:30_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-26 10:30_

---

_Label `performance` added by @sharkdp on 2025-06-26 10:30_

---

_Review requested from @dcreager by @sharkdp on 2025-06-26 10:30_

---

_Label `ty` added by @sharkdp on 2025-06-26 10:30_

---

_Label `internal` added by @sharkdp on 2025-06-26 10:30_

---

_@AlexWaygood approved on 2025-06-26 10:33_

---

_Comment by @github-actions[bot] on 2025-06-26 10:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-06-26 10:45_

some nice speedups of 1-2% on our benchmarks there: https://codspeed.io/astral-sh/ruff/branches/david%2Fprevent-builder-construction?runnerMode=Instrumentation

---

_Comment by @sharkdp on 2025-06-26 10:59_

> some nice speedups of 1-2% on our benchmarks there: https://codspeed.io/astral-sh/ruff/branches/david%2Fprevent-builder-construction?runnerMode=Instrumentation

Nice! Good call, @carljm.

---

_Merged by @sharkdp on 2025-06-26 11:00_

---

_Closed by @sharkdp on 2025-06-26 11:00_

---

_Branch deleted on 2025-06-26 11:00_

---

_Comment by @MichaReiser on 2025-06-26 11:00_

are you saying I should see if my `UnionBuilder::from_elements` optimization shows a perf win now

---

_Comment by @sharkdp on 2025-06-26 11:14_

> are you saying I should see if my `UnionBuilder::from_elements` optimization shows a perf win now

Notice that the builder here is not a pure `UnionBuilder`. Not sure what your optimization was about, but it's unlikely to translate to `DeclaredTypeBuilder`, I think. I used "union builder" in the title because this is a wrapper around `UnionBuilder`.

---

_Comment by @MichaReiser on 2025-06-26 11:58_

It was this https://github.com/astral-sh/ruff/pull/16218

---
