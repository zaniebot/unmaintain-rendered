```yaml
number: 234
title: "Improve test coverage for edge cases involving `--typeshed`"
type: issue
state: open
author: AlexWaygood
labels:
  - testing
assignees: []
created_at: 2024-10-31T11:34:12Z
updated_at: 2025-11-18T16:10:24Z
url: https://github.com/astral-sh/ty/issues/234
synced_at: 2026-01-12T15:54:22Z
```

# Improve test coverage for edge cases involving `--typeshed`

---

_@AlexWaygood_

red-knot allows users to specify a custom typeshed using `--typeshed`, but we currently have poor test coverage for cases where a custom typeshed means that builtin or stdlib symbols aren't available to us as a result of this. We should improve our coverage here -- for example, what happens to our binary arithmetic logic if we're in a world where there's no `builtins.int` symbol available to us? (I have no idea.)

We have _some_ test cases for this scenario here, but we need more:

https://github.com/astral-sh/ruff/blob/76e4277696e8b34b771d554f8f0d07054d413289/crates/red_knot_python_semantic/src/types/infer.rs#L4570-L4603

It will be easier to add tests for this scenario once we've implemented support for custom typeshed directories in mdtest.

---

_Label `testing` added by @AlexWaygood on 2024-10-31 11:34_

---

_Renamed from "[red-knot] Improve test coverage for edge cases involving `--custom-typeshed-dir`" to "[red-knot] Improve test coverage for edge cases involving `--typeshed`" by @MichaReiser on 2024-12-12 12:39_

---

_Label `testing` added by @MichaReiser on 2025-05-07 15:22_

---

_Renamed from "[red-knot] Improve test coverage for edge cases involving `--typeshed`" to "Improve test coverage for edge cases involving `--typeshed`" by @MichaReiser on 2025-05-07 15:27_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 15:47_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
