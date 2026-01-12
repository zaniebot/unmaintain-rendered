```yaml
number: 13136
title: "[red-knot] Remove very noisy tracing call when resolving `ImportFrom` statements"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-cleanup-import-logs
created_at: 2024-08-28T10:00:32Z
updated_at: 2024-08-28T10:14:37Z
url: https://github.com/astral-sh/ruff/pull/13136
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Remove very noisy tracing call when resolving `ImportFrom` statements

---

_@AlexWaygood_

## Summary

This tracing call generates a huge amount of output that makes the logs hard to read:

```
2      ├─   0.062121s  12ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: symbol_table(Id(13c2)) } }
2      └─┐red_knot_python_semantic::semantic_index::symbol_table{scope=Id(13c2), file=vendored://stdlib/typing.pyi}
2      ┌─┘
2      └─┐red_knot_python_semantic::types::symbol_ty{symbol=ScopedSymbolId(32)}
2        ├─   0.062192s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: use_def_map(Id(13c2)) } }
2        └─┐red_knot_python_semantic::semantic_index::use_def_map{scope=Id(13c2), file=vendored://stdlib/typing.pyi}
2        ┌─┘
2        ├─   0.062239s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(2faf)) } }
2        └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(2faf), file=vendored://stdlib/typing.pyi}
2          └─┐red_knot_python_semantic::types::symbol_ty{symbol=ScopedSymbolId(78)}
2            ├─   0.062310s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(1850)) } }
2            └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(1850), file=vendored://stdlib/builtins.pyi}
2              ├─   0.062348s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_expression_types(Id(1402)) } }
2              └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(1402), file=vendored://stdlib/builtins.pyi}
2                ├─   0.062410s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(1841)) } }
2                └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(1841), file=vendored://stdlib/builtins.pyi}
2                  ├─   0.062451s   0ms TRACE red_knot_python_semantic::types::infer Resolving imported object Alias { range: 1355..1362, name: Identifier { id: Name("TypeVar"), range: 1355..1362 }, asname: None } from statement StmtImportFrom { range: 1067..1411, module: Some(Identifier { id: Name("typing"), range: 1072..1078 }), names: [Alias { range: 1106..1108, name: Identifier { id: Name("IO"), range: 1106..1108 }, asname: None }, Alias { range: 1114..1117, name: Identifier { id: Name("Any"), range: 1114..1117 }, asname: None }, Alias { range: 1123..1131, name: Identifier { id: Name("BinaryIO"), range: 1123..1131 }, asname: None }, Alias { range: 1137..1145, name: Identifier { id: Name("ClassVar"), range: 1137..1145 }, asname: None }, Alias { range: 1151..1158, name: Identifier { id: Name("Generic"), range: 1151..1158 }, asname: None }, Alias { range: 1164..1171, name: Identifier { id: Name("Mapping"), range: 1164..1171 }, asname: None }, Alias { range: 1177..1191, name: Identifier { id: Name("MutableMapping"), range: 1177..1191 }, asname: None }, Alias { range: 1197..1212, name: Identifier { id: Name("MutableSequence"), range: 1197..1212 }, asname: None }, Alias { range: 1218..1226, name: Identifier { id: Name("NoReturn"), range: 1218..1226 }, asname: None }, Alias { range: 1232..1240, name: Identifier { id: Name("Protocol"), range: 1232..1240 }, asname: None }, Alias { range: 1246..1254, name: Identifier { id: Name("Sequence"), range: 1246..1254 }, asname: None }, Alias { range: 1260..1271, name: Identifier { id: Name("SupportsAbs"), range: 1260..1271 }, asname: None }, Alias { range: 1277..1290, name: Identifier { id: Name("SupportsBytes"), range: 1277..1290 }, asname: None }, Alias { range: 1296..1311, name: Identifier { id: Name("SupportsComplex"), range: 1296..1311 }, asname: None }, Alias { range: 1317..1330, name: Identifier { id: Name("SupportsFloat"), range: 1317..1330 }, asname: None }, Alias { range: 1336..1349, name: Identifier { id: Name("SupportsIndex"), range: 1336..1349 }, asname: None }, Alias { range: 1355..1362, name: Identifier { id: Name("TypeVar"), range: 1355..1362 }, asname: None }, Alias { range: 1368..1373, name: Identifier { id: Name("final"), range: 1368..1373 }, asname: None }, Alias { range: 1379..1387, name: Identifier { id: Name("overload"), range: 1379..1387 }, asname: None }, Alias { range: 1393..1408, name: Identifier { id: Name("type_check_only"), range: 1393..1408 }, asname: None }], level: 0 }
2                  ├─   0.062504s   0ms TRACE red_knot_python_semantic::types::infer Resolving imported object 'TypeVar' from module 'typing'
2                  └─┐red_knot_python_semantic::types::symbol_ty{symbol=ScopedSymbolId(33)}
2                    ├─   0.062550s   0ms TRACE red_knot_workspace::db Salsa event: Event { thread_id: ThreadId(2), kind: WillExecute { database_key: infer_definition_types(Id(2fb0)) } }
2                    └─┐red_knot_python_semantic::types::infer::infer_definition_types{definition=Id(2fb0), file=vendored://stdlib/typing.pyi}
2                    ┌─┘
2                  ┌─┘
2                ┌─┘
2              ┌─┘
2            ┌─┘
2          ┌─┘
2        ┌─┘
2      ┌─┘
2    ┌─┘
```

And it's just not really needed when the next tracing call immediately afterwards tells you which import is being resolved, much more concisely:

```
2                  ├─   0.062504s   0ms TRACE red_knot_python_semantic::types::infer Resolving imported object 'TypeVar' from module 'typing'
```

## Test Plan

<!-- How was it tested? -->


---

_Label `red-knot` added by @AlexWaygood on 2024-08-28 10:00_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-28 10:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-28 10:00_

---

_Merged by @AlexWaygood on 2024-08-28 10:05_

---

_Closed by @AlexWaygood on 2024-08-28 10:05_

---

_Branch deleted on 2024-08-28 10:05_

---

_Comment by @github-actions[bot] on 2024-08-28 10:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
