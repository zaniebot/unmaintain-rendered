```yaml
number: 205
title: "Support implicit class attributes (only defined in a `@classmethod`)"
type: issue
state: closed
author: sharkdp
labels:
  - attribute access
assignees: []
created_at: 2025-02-05T12:40:16Z
updated_at: 2025-06-24T09:42:12Z
url: https://github.com/astral-sh/ty/issues/205
synced_at: 2026-01-10T02:08:20Z
```

# Support implicit class attributes (only defined in a `@classmethod`)

---

_Issue opened by @sharkdp on 2025-02-05 12:40_

Similar to how we support implicit *instance* attributes, we should also support implicitly defined *class* attributes. More concretely, we want the `TODO` comment here to be resolved, i.e. there should be no `unresolved-attribute` diagnostic:

https://github.com/astral-sh/ruff/blob/eb08345fd5434ed3db243233df6dd91757a6af3b/crates/red_knot_python_semantic/resources/mdtest/attributes.md?plain=1#L503-L514

The implementation for this can probably re-use most of the machinery introduced in [this PR](https://github.com/astral-sh/ruff/pull/15811). In particular, the [`Type::implicit_instance_attribute` method](https://github.com/astral-sh/ruff/blob/16f2a93fca20082179df082bc16a2c5095933772/crates/red_knot_python_semantic/src/types.rs#L4181-L4248).

---

_Renamed from "Support implicit class attributes (only defined in a `@classmethod`)" to "[red-knot] Support implicit class attributes (only defined in a `@classmethod`)" by @sharkdp on 2025-02-05 12:40_

---

_Label `help wanted` added by @sharkdp on 2025-02-05 13:51_

---

_Label `help wanted` removed by @sharkdp on 2025-02-05 16:27_

---

_Renamed from "[red-knot] Support implicit class attributes (only defined in a `@classmethod`)" to "Support implicit class attributes (only defined in a `@classmethod`)" by @MichaReiser on 2025-05-07 15:26_

---

_Label `attribute access` added by @AlexWaygood on 2025-05-11 08:05_

---

_Closed by @sharkdp on 2025-06-24 09:42_

---
