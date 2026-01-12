```yaml
number: 2113
title: "Duplicate `invalid-named-tuple` diagnostics if `_asdict` is declared and bound in separate statements"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
assignees: []
created_at: 2025-12-19T12:37:47Z
updated_at: 2025-12-19T12:37:47Z
url: https://github.com/astral-sh/ty/issues/2113
synced_at: 2026-01-12T15:54:26Z
```

# Duplicate `invalid-named-tuple` diagnostics if `_asdict` is declared and bound in separate statements

---

_@AlexWaygood_

### Summary

Opening an issue so I don't forget about this TODO: https://github.com/astral-sh/ruff/blob/e177cc2a5a5d5e25c06ea6b0bbac9209f37fe756/crates/ty_python_semantic/resources/mdtest/named_tuple.md?plain=1#L623-L631. The bug can probably also be reproduced for some of our other override-related rules such as `invalid-method-override`. The bug is caused due to the way `list_members::all_end_of_scope_members` naively chains together declarations and bindings [here](https://github.com/astral-sh/ruff/blob/e177cc2a5a5d5e25c06ea6b0bbac9209f37fe756/crates/ty_python_semantic/src/types/list_members.rs#L37-L76); it needs to dedupe them; I'm imagining that it would return an `FxHashMap<Name, Member<'db>>` struct, where `Member` holds information about the symbol's declaration and/or its binding.

It's possible that the `list_members::all_reachable_members` iterator that our completions infrastructure uses needs a similar bugfix, too.

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-12-19 12:37_

---
