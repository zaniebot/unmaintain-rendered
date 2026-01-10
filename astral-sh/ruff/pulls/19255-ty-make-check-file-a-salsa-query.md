```yaml
number: 19255
title: "[ty] Make `check_file` a salsa query"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/check-file-query
created_at: 2025-07-10T10:49:33Z
updated_at: 2025-07-10T13:16:58Z
url: https://github.com/astral-sh/ruff/pull/19255
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Make `check_file` a salsa query

---

_Pull request opened by @MichaReiser on 2025-07-10 10:49_

## Summary
We noticed that all files get reparsed when workspace diagnostics are enabled. 

I realised that this is because `check_file_impl` access the parsed module but itself isn't a salsa query. 
This pr makes `check_file_impl` a salsa query, so that we only access the `parsed_module` when the file actually changed. I decided to remove the salsa query from `check_types` because most functions it calls are salsa queries itself and having both `check_types` and `check_file` as salsa querise has the downside that we double cache the diagnostics.

## Test Plan

**Before**

```
2025-07-10 12:54:16.620766000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c0c))}: File `/Users/micha/astral/test/yaml/yaml-stubs/__init__.pyi` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.621942000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c13))}: File `/Users/micha/astral/test/ignore2 2/nested-repository/main.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.622107000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c09))}: File `/Users/micha/astral/test/notebook.ipynb` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.622357000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c04))}: File `/Users/micha/astral/test/no-trailing.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.622634000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c02))}: File `/Users/micha/astral/test/simple.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.623056000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c07))}: File `/Users/micha/astral/test/open/more.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.623254000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c11))}: File `/Users/micha/astral/test/ignore-bug/backend/src/subdir/log/some_logging_lib.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.623450000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c0f))}: File `/Users/micha/astral/test/yaml/tomllib/__init__.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.624599000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c05))}: File `/Users/micha/astral/test/create.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.624784000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c00))}: File `/Users/micha/astral/test/lib.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.624911000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c0a))}: File `/Users/micha/astral/test/sub/test.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.625032000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c12))}: File `/Users/micha/astral/test/ignore2/nested-repository/main.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.625101000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c08))}: File `/Users/micha/astral/test/open/test.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.625227000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c03))}: File `/Users/micha/astral/test/pseudocode_with_bom.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.625353000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c0b))}: File `/Users/micha/astral/test/yaml/yaml-stubs/loader.pyi` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.625543000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c01))}: File `/Users/micha/astral/test/test_trailing.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.625616000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c0d))}: File `/Users/micha/astral/test/yaml/tomllib/_re.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.625667000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c06))}: File `/Users/micha/astral/test/yaml/main.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.625779000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c10))}: File `/Users/micha/astral/test/yaml/tomllib/_types.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.627526000  WARN request{id=19 method="workspace/diagnostic"}:Project::check:check_file{file=file(Id(c0e))}: File `/Users/micha/astral/test/yaml/tomllib/_parser.py` was reparsed after being collected in the current Salsa revision
2025-07-10 12:54:16.627959000 DEBUG request{id=19 method="workspace/diagnostic"}:Project::check: Checking all files took 0.007s
```

Now, no more logs regarding reparsing

---

_Review requested from @carljm by @MichaReiser on 2025-07-10 10:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-10 10:49_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-10 10:49_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-10 10:49_

---

_Label `internal` added by @MichaReiser on 2025-07-10 10:49_

---

_Label `ty` added by @MichaReiser on 2025-07-10 10:49_

---

_Comment by @github-actions[bot] on 2025-07-10 10:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dhruvmanila approved on 2025-07-10 13:15_

Thanks!

---

_Merged by @dhruvmanila on 2025-07-10 13:16_

---

_Closed by @dhruvmanila on 2025-07-10 13:16_

---

_Branch deleted on 2025-07-10 13:16_

---
