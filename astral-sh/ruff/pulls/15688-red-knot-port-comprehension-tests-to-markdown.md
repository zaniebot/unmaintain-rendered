```yaml
number: 15688
title: "[red-knot] Port comprehension tests to Markdown"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/port-comprehension-tests
created_at: 2025-01-23T12:21:10Z
updated_at: 2025-01-23T12:49:31Z
url: https://github.com/astral-sh/ruff/pull/15688
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Port comprehension tests to Markdown

---

_@sharkdp_

## Summary

Port comprehension tests from Rust to Markdown

I don' think the remaining tests in `infer.rs` should be ported to Markdown, maybe except for the incremental-checking tests when (if ever) we have support for that in the MD tests. So I think it's okay to close #13696 when this is merged.

---

_Label `red-knot` added by @sharkdp on 2025-01-23 12:21_

---

_Review requested from @carljm by @sharkdp on 2025-01-23 12:21_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-23 12:21_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-23 12:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:112 on 2025-01-23 12:25_

```suggestion
# error: [not-iterable] "Object of type `NotIterable` is not iterable"
[*NotIterable()]

# fine
[*Iterable()]
```

---

_@AlexWaygood approved on 2025-01-23 12:36_

So much better!

---

_Merged by @sharkdp on 2025-01-23 12:49_

---

_Closed by @sharkdp on 2025-01-23 12:49_

---

_Branch deleted on 2025-01-23 12:49_

---
