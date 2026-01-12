```yaml
number: 11425
title: Move string-prefix enumerations to a separate submodule
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: move-prefix-code
created_at: 2024-05-14T14:09:20Z
updated_at: 2024-05-15T11:40:32Z
url: https://github.com/astral-sh/ruff/pull/11425
synced_at: 2026-01-12T15:55:38Z
```

# Move string-prefix enumerations to a separate submodule

---

_@AlexWaygood_

## Summary

This moves the string-prefix enumerations in `ruff_python_ast` to a separate submodule. I think this helps clarify that these prefixes are purely abstract: they only depend on each other, and do not depend on any of the other code in `nodes.rs` in any way. Moreover, while various AST nodes _use_ them, they're not really nodes themselves, so they feel slightly out of place in `nodes.rs`.

I considered moving all of them to `str.rs`, but it felt like enough code that it could be a separate submodule.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-05-14 14:09_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-05-14 14:09_

---

_Comment by @github-actions[bot] on 2024-05-14 14:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila approved on 2024-05-15 09:02_

I'm not sure if I'd want to keep the string nodes and the types which make up the nodes in separate modules. That said, I think it would be ok to keep these definitions in `str.rs` module. I leave it up to you though.

Ideally, I'd make a new `ast` directory which has various sub-modules for a group of similar nodes like statements, expressions, literals, patterns, etc. and possibly keep them in sync with how the parser code is structured. Or, have a single `ast.rs` which has all the ndoes and related types 🤷‍♂️

---

_Label `internal` added by @dhruvmanila on 2024-05-15 09:02_

---

_Merged by @AlexWaygood on 2024-05-15 11:40_

---

_Closed by @AlexWaygood on 2024-05-15 11:40_

---

_Branch deleted on 2024-05-15 11:40_

---
