```yaml
number: 18707
title: "[ty] Fix panic when attempting to provide autocompletions for an instance of a class that assigns attributes to `self[0]`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/fix-autocomplete-subscript-attrs
created_at: 2025-06-16T13:36:31Z
updated_at: 2025-06-17T06:45:39Z
url: https://github.com/astral-sh/ruff/pull/18707
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Fix panic when attempting to provide autocompletions for an instance of a class that assigns attributes to `self[0]`

---

_Pull request opened by @AlexWaygood on 2025-06-16 13:36_

## Summary

Ty currently panics when attempting to provide autocompletions in this scenario:

```py
class Point:
    def orthogonal_direction(self):
        self[0].is_zero


def test_point(p2: Point):
    p2.<CURSOR>
```

The root cause of this issue is that we're incorrectly registering the `[0].is_zero` place as an "instance attribute" `Place`. `Place`s should only be registered as instance attributes if they have the form `self.<NAME>`; `self<SUBSCRIPT>.<NAME>` does not constitute an instance attribute.

This panic was uncovered by https://github.com/astral-sh/ruff/pull/18705.

## Test Plan

I added the above repro as a test to `ty_ide/completions.rs`


---

_Review requested from @carljm by @AlexWaygood on 2025-06-16 13:36_

---

_Label `bug` added by @AlexWaygood on 2025-06-16 13:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-16 13:36_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-16 13:36_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-16 13:36_

---

_Label `ty` added by @AlexWaygood on 2025-06-16 13:36_

---

_Comment by @github-actions[bot] on 2025-06-16 13:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp approved on 2025-06-16 18:51_

Thank you!

I wondered if `matches!(place_expr.sub_segments(), &[PlaceExprSubSegment::Member(_)])` should be a method on `PlaceExpr`, but it already has `PlaceExpr::is_instance_attribute` and `PlaceExpr::is_instance_attribute_named` with subtly different meanings. But maybe that's an argument for doing it (and clarifying the difference in doc comments)?

---

_Comment by @AlexWaygood on 2025-06-16 21:51_

> Thank you!
> 
> I wondered if `matches!(place_expr.sub_segments(), &[PlaceExprSubSegment::Member(_)])` should be a method on `PlaceExpr`, but it already has `PlaceExpr::is_instance_attribute` and `PlaceExpr::is_instance_attribute_named` with subtly different meanings. But maybe that's an argument for doing it (and clarifying the difference in doc comments)?

Good call. I added some higher-level abstractions to `place.rs`, and lots of doc-comments to distinguish semantic-index-internal methods from crate-public API 👍

---

_Merged by @AlexWaygood on 2025-06-16 21:58_

---

_Closed by @AlexWaygood on 2025-06-16 21:58_

---

_Branch deleted on 2025-06-16 21:58_

---

_Comment by @sharkdp on 2025-06-17 06:45_

Thank you for the update, this is great!

---
