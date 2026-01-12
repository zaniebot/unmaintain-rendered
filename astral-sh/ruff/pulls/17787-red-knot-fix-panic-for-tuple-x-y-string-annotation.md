```yaml
number: 17787
title: "[red-knot] Fix panic for `tuple[x[y]]` string annotation"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-17775
created_at: 2025-05-02T07:22:08Z
updated_at: 2025-05-02T11:28:43Z
url: https://github.com/astral-sh/ruff/pull/17787
synced_at: 2026-01-12T15:56:05Z
```

# [red-knot] Fix panic for `tuple[x[y]]` string annotation

---

_@sharkdp_

## Summary

closes #17775

## Test Plan

Added corpus regression test


---

_Label `red-knot` added by @sharkdp on 2025-05-02 07:22_

---

_Comment by @MichaReiser on 2025-05-02 07:23_

Nice! I've been waiting for this panic to "go away" :)

---

_Comment by @sharkdp on 2025-05-02 07:28_

So this is a bit unfortunate… the `mypy_primer` run fails because I'm adding projects that are still panicking in the old state on `main`. Ideally, we'd only fail that run if we're introducing new panics.

I can run `mypy_primer` locally with the new projects `--project-selector '/alerta|aiohttp|meson|sphinx|trio$'`

---

_Marked ready for review by @sharkdp on 2025-05-02 07:44_

---

_Review requested from @carljm by @sharkdp on 2025-05-02 07:44_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-02 07:44_

---

_Review requested from @dcreager by @sharkdp on 2025-05-02 07:44_

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-02 07:44_

---

_Comment by @sharkdp on 2025-05-02 10:11_

Going to merge this, as it seems relatively uncontroversial — the whole fix happens in a function that will eventually go away.

---

_Merged by @sharkdp on 2025-05-02 10:11_

---

_Closed by @sharkdp on 2025-05-02 10:11_

---

_Branch deleted on 2025-05-02 10:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:7633 on 2025-05-02 10:47_

oof, that seems a bit unfortunate. Does that mean we should just get rid of the `expression_type()` API altogether? Or have `expression_type()` itself check whether we're in a deferred string annotation (and fallback to `infer_expression()` if so)?

It feels like a bit of a footgun if we have to always make sure we're not in a deferred string annotation before calling `expression_type()`

---

_@AlexWaygood reviewed on 2025-05-02 10:47_

---

_@sharkdp reviewed on 2025-05-02 11:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:7633 on 2025-05-02 11:28_

Good questions. It's certainly possible that we have other hidden problems with calling `expression_type` for something that might be inside a string annotation. But note that it's not common to re-enter a sub-AST and match on inner elements (and then use `expression_type`) after the type has already been inferred. Usually, in functions like `infer_expression`, we return types "back up" to the caller, i.e. to outer elements. Admittedly, I did not think too much about this exact callsite because this whole function is basically a large "To do" and will disappear eventually.

---
