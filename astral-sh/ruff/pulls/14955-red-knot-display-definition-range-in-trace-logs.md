```yaml
number: 14955
title: "[red-knot] Display definition range in trace logs"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/definition-range
created_at: 2024-12-13T10:23:43Z
updated_at: 2024-12-13T14:34:02Z
url: https://github.com/astral-sh/ruff/pull/14955
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Display definition range in trace logs

---

_Pull request opened by @dhruvmanila on 2024-12-13 10:23_

I've mainly opened this PR to get some opinions. I've found having some additional information in the tracing logs to be useful to determine what we are currently inferring. For the `Definition` ingredient, the range seems to be much useful. I thought of using the identifier name but we would have to deconstruct the `Expr` to find out the identifier which seems a lot for just trace logs. Additionally, multiple identifiers _could_ have the same name where range would be useful.

The ranges are isolated to the names that have been defined by the definition except for the `except` block where the entire range is being used because the name is optional.

***Before:***

```
3      ├─   0.074671s  54ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: infer_definition_types(Id(1402)) } }
3      └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(1402), file=/Users/dhruv/playground/ruff/type_inference/isolated3/play.py}
3      ┌─┘
3      ├─   0.074768s  54ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: inner_fn_name_(Id(2800)) } }
3      ├─   0.074807s  54ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: infer_deferred_types(Id(1735)) } }
3      └─┐red_knot_python_semantic::types::infer::infer_deferred_types{definition=Id(1735), file=vendored://stdlib/typing.pyi}
3        ├─   0.074842s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: infer_definition_types(Id(14f3)) } }
3        └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(14f3), file=vendored://stdlib/typing.pyi}
3          ├─   0.074871s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: infer_expression_types(Id(1820)) } }
3          └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(1820), file=vendored://stdlib/typing.pyi}
3            ├─   0.074924s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: infer_definition_types(Id(1429)) } }
3            └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(1429), file=vendored://stdlib/typing.pyi}
3              ├─   0.074958s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: infer_definition_types(Id(1428)) } }
3              └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(1428), file=vendored://stdlib/typing.pyi}
3              ┌─┘
```

***After:***

```
12      ├─   0.074609s  55ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: infer_definition_types(Id(1402)) } }
12      └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(1402), range=36..37, file=/Users/dhruv/playground/ruff/type_inference/isolated3/play.py}
12      ┌─┘
12      ├─   0.074705s  55ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: inner_fn_name_(Id(2800)) } }
12      ├─   0.074742s  55ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: infer_deferred_types(Id(1735)) } }
12      └─┐red_knot_python_semantic::types::infer::infer_deferred_types{definition=Id(1735), range=30225..30236, file=vendored://stdlib/typing.pyi}
12        ├─   0.074775s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: infer_definition_types(Id(14f3)) } }
12        └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(14f3), range=9472..9474, file=vendored://stdlib/typing.pyi}
12          ├─   0.074803s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: infer_expression_types(Id(1820)) } }
12          └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(1820), range=9477..9490, file=vendored://stdlib/typing.pyi}
12            ├─   0.074855s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: infer_definition_types(Id(1429)) } }
12            └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(1429), range=3139..3146, file=vendored://stdlib/typing.pyi}
12              ├─   0.074892s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: infer_definition_types(Id(1428)) } }
12              └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(1428), range=3102..3107, file=vendored://stdlib/typing.pyi}
12              ┌─┘
```

---

_Label `red-knot` added by @dhruvmanila on 2024-12-13 10:23_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-13 10:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-13 10:23_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-13 10:23_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-13 10:23_

---

_Comment by @github-actions[bot] on 2024-12-13 10:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:470 on 2024-12-13 13:20_

would it be better to implement `Ranged` for `DefinitionKind`? Then we'd also get the `start()` and `stop()` methods that the trait implements

---

_@AlexWaygood approved on 2024-12-13 13:21_

Nice, this is a big improvement! Even better would be the line number/column number, but we could consider that separately

---

_Comment by @MichaReiser on 2024-12-13 13:24_

> Nice, this is a big improvement! Even better would be the line number/column number, but we could consider that separately

Getting the line and column number is too expensive because it a) requires building the line index and b) a binary search to get the line number. 

---

_@dhruvmanila reviewed on 2024-12-13 13:46_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:470 on 2024-12-13 13:46_

I thought about that but that would make it publicly available which I wasn't sure about. I could possibly rename this function to `debug_range` and provide a brief docs on what this represents.

I guess it's fine to implement it, will do so.

---

_Renamed from "Display definition range in trace logs" to "[red-knot] Display definition range in trace logs" by @dhruvmanila on 2024-12-13 14:25_

---

_Merged by @dhruvmanila on 2024-12-13 14:29_

---

_Closed by @dhruvmanila on 2024-12-13 14:29_

---

_Branch deleted on 2024-12-13 14:29_

---
