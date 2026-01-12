```yaml
number: 21263
title: "[ty] Allow values of type `None` in type expressions"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/allow-none-in-type-expressions
created_at: 2025-11-03T20:55:39Z
updated_at: 2025-11-04T15:29:56Z
url: https://github.com/astral-sh/ruff/pull/21263
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Allow values of type `None` in type expressions

---

_@sharkdp_

## Summary

Allow values of type `None` in type expressions. The [typing spec](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions) could be more explicit on whether this is actually allowed or not, but it seems relatively harmless and does help in some use cases like:

```py
try:
    from module import MyClass
except ImportError:
    MyClass = None  # ty: ignore


def f(m: MyClass):
    pass
``` 

## Test Plan

Updated tests, ecosystem check.


---

_Label `ty` added by @sharkdp on 2025-11-03 20:55_

---

_Comment by @github-actions[bot] on 2025-11-03 20:57_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-03 20:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kopf (https://github.com/nolar/kopf)
- kopf/_cogs/helpers/thirdparty.py:41:36: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- kopf/_cogs/helpers/thirdparty.py:45:36: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- Found 47 diagnostics
+ Found 45 diagnostics

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/tty.pyi:25:43: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- mypy/typeshed/stdlib/tty.pyi:26:46: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- Found 1839 diagnostics
+ Found 1837 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dbt/prefect_dbt/cli/credentials.py:166:42: warning[possibly-missing-attribute] Attribute `get_configs` may be missing on object of type `Unknown | None`
- src/integrations/prefect-dbt/prefect_dbt/cli/credentials.py:133:23: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- src/integrations/prefect-dbt/prefect_dbt/cli/credentials.py:134:23: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- src/integrations/prefect-dbt/prefect_dbt/cli/credentials.py:135:23: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- Found 3308 diagnostics
+ Found 3306 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/client_exceptions.py:161:28: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- Found 161 diagnostics
+ Found 160 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/internal/ci_visibility/coverage.py:105:12: warning[possibly-missing-attribute] Attribute `_collector` may be missing on object of type `Unknown | None`
+ ddtrace/internal/ci_visibility/coverage.py:105:44: warning[possibly-missing-attribute] Attribute `_collector` may be missing on object of type `Unknown | None`
+ ddtrace/internal/ci_visibility/coverage.py:127:9: warning[possibly-missing-attribute] Attribute `switch_context` may be missing on object of type `Unknown | None`
- ddtrace/internal/ci_visibility/coverage.py:104:45: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- ddtrace/internal/ci_visibility/coverage.py:113:20: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- ddtrace/internal/ci_visibility/coverage.py:125:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ddtrace/internal/ci_visibility/coverage.py:134:20: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
- ddtrace/internal/ci_visibility/coverage.py:163:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ddtrace/internal/ci_visibility/coverage.py:178:22: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
+ ddtrace/internal/ci_visibility/coverage.py:179:12: warning[possibly-missing-attribute] Attribute `_collector` may be missing on object of type `Unknown | None`
+ ddtrace/internal/ci_visibility/coverage.py:179:39: warning[possibly-missing-attribute] Attribute `_collector` may be missing on object of type `Unknown | None`
- ddtrace/internal/ci_visibility/coverage.py:188:29: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
+ ddtrace/internal/ci_visibility/coverage.py:184:26: warning[possibly-missing-attribute] Attribute `_collector` may be missing on object of type `Unknown | None`
- Found 8156 diagnostics
+ Found 8155 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] Allow values of type None in type expressions" to "[ty] Allow values of type `None` in type expressions" by @sharkdp on 2025-11-03 21:18_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-03 21:19_

---

_Comment by @github-actions[bot] on 2025-11-03 21:25_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-type-form` | 0 | 13 | 0 |
| `possibly-missing-attribute` | 7 | 0 | 0 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **7** | **15** | **0** |

**[Full report with detailed diff](https://david-allow-none-in-type-exp.ecosystem-663.pages.dev/diff)** ([timing results](https://david-allow-none-in-type-exp.ecosystem-663.pages.dev/timing))


---

_Marked ready for review by @sharkdp on 2025-11-04 13:50_

---

_Review requested from @carljm by @sharkdp on 2025-11-04 13:50_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-04 13:50_

---

_Review requested from @dcreager by @sharkdp on 2025-11-04 13:50_

---

_@AlexWaygood approved on 2025-11-04 14:39_

---

_Merged by @sharkdp on 2025-11-04 15:29_

---

_Closed by @sharkdp on 2025-11-04 15:29_

---

_Branch deleted on 2025-11-04 15:29_

---
