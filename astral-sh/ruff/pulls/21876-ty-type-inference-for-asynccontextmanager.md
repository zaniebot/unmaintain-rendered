```yaml
number: 21876
title: "[ty] Type inference for `@asynccontextmanager`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1804
created_at: 2025-12-09T20:25:46Z
updated_at: 2025-12-09T21:49:03Z
url: https://github.com/astral-sh/ruff/pull/21876
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Type inference for `@asynccontextmanager`

---

_Pull request opened by @sharkdp on 2025-12-09 20:25_

## Summary

This PR adds special handling for `asynccontextmanager` calls as a temporary solution for https://github.com/astral-sh/ty/issues/1804. We will be able to remove this soon once we have support for generic protocols in the solver.

closes https://github.com/astral-sh/ty/issues/1804

## Ecosystem

```diff
+ tests/test_downloadermiddleware.py:305:56: error[invalid-argument-type] Argument to bound method `download` is incorrect: Expected `Spider`, found `Unknown | Spider | None`
+ tests/test_downloadermiddleware.py:305:56: warning[possibly-missing-attribute] Attribute `spider` may be missing on object of type `Crawler | None`
```
These look like true positives

```diff
+ pymongo/asynchronous/database.py:1021:35: error[invalid-assignment] Object of type `(AsyncClientSession & ~AlwaysTruthy & ~AlwaysFalsy) | (_ServerMode & ~AlwaysFalsy) | Unknown | Primary` is not assignable to `_ServerMode | None`
+ pymongo/asynchronous/database.py:1025:17: error[invalid-argument-type] Argument to bound method `_conn_for_reads` is incorrect: Expected `_ServerMode`, found `_ServerMode | None`
```

Known problems or true positives, just caused by the new type for `session`

```diff
- src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:269:16: error[invalid-return-type] Return type does not match returned value: expected `Connection | AsyncConnection`, found `_GeneratorContextManager[Unknown, None, None] | _AsyncGeneratorContextManager[Unknown, None] | Connection | AsyncConnection`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:269:16: error[invalid-return-type] Return type does not match returned value: expected `Connection | AsyncConnection`, found `_GeneratorContextManager[Unknown, None, None] | _AsyncGeneratorContextManager[AsyncConnection, None] | Connection | AsyncConnection`
```

Just a more concrete type

```diff
- src/prefect/flow_engine.py:1277:24: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/server/api/server.py:696:49: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/task_engine.py:1426:24: error[missing-argument] No argument provided for required parameter `cls`
```

Good

## Test Plan

* Adapted and newly added Markdown tests
* Tested on internal codebase


---

_Label `ty` added by @sharkdp on 2025-12-09 20:25_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-09 20:25_

---

_@sharkdp reviewed on 2025-12-09 20:27_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:4665 on 2025-12-09 20:27_

This took me a while to figure out. Looks related to https://github.com/astral-sh/ty/issues/1821 / https://github.com/astral-sh/ty/issues/1821. If we don't do this, we end up with unions of the actual type that we want to have and `Divergent`

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 20:27_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 20:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_downloadermiddleware.py:305:56: error[invalid-argument-type] Argument to bound method `download` is incorrect: Expected `Spider`, found `Unknown | Spider | None`
+ tests/test_downloadermiddleware.py:305:56: warning[possibly-missing-attribute] Attribute `spider` may be missing on object of type `Crawler | None`
- Found 1735 diagnostics
+ Found 1737 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ pymongo/asynchronous/database.py:1021:35: error[invalid-assignment] Object of type `(AsyncClientSession & ~AlwaysTruthy & ~AlwaysFalsy) | (_ServerMode & ~AlwaysFalsy) | Unknown | Primary` is not assignable to `_ServerMode | None`
+ pymongo/asynchronous/database.py:1025:17: error[invalid-argument-type] Argument to bound method `_conn_for_reads` is incorrect: Expected `_ServerMode`, found `_ServerMode | None`
- Found 460 diagnostics
+ Found 462 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_channel.py:610:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@as_safe_channel) -> AbstractAsyncContextManager[ReceiveChannel[T@as_safe_channel], bool | None]`, found `((self, *args: P@as_safe_channel.args, **kwargs: P@as_safe_channel.kwargs) -> _AsyncGeneratorContextManager[Unknown, None]) | Unknown`
- Found 494 diagnostics
+ Found 495 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:269:16: error[invalid-return-type] Return type does not match returned value: expected `Connection | AsyncConnection`, found `_GeneratorContextManager[Unknown, None, None] | _AsyncGeneratorContextManager[Unknown, None] | Connection | AsyncConnection`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:269:16: error[invalid-return-type] Return type does not match returned value: expected `Connection | AsyncConnection`, found `_GeneratorContextManager[Unknown, None, None] | _AsyncGeneratorContextManager[AsyncConnection, None] | Connection | AsyncConnection`
- src/prefect/flow_engine.py:1277:24: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/server/api/server.py:696:49: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/task_engine.py:1426:24: error[missing-argument] No argument provided for required parameter `cls`
- Found 5480 diagnostics
+ Found 5477 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 20:35_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 0 | 0 |
| `invalid-return-type` | 1 | 0 | 1 |
| `invalid-assignment` | 1 | 0 | 0 |
| `possibly-missing-attribute` | 1 | 0 | 0 |
| `unsupported-base` | 0 | 1 | 0 |
| **Total** | **5** | **1** | **1** |

**[Full report with detailed diff](https://david-fix-1804.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1804.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-09 20:40_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-09 20:40_

---

_@sharkdp reviewed on 2025-12-09 20:42_

---

_Review comment by @sharkdp on `scripts/check_ecosystem.py`:50 on 2025-12-09 20:42_

Oh look, we found a bug.

---

_Marked ready for review by @sharkdp on 2025-12-09 20:56_

---

_Review requested from @carljm by @sharkdp on 2025-12-09 20:56_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-09 20:56_

---

_Review requested from @dcreager by @sharkdp on 2025-12-09 20:56_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 20:59_


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

_@carljm approved on 2025-12-09 21:18_

This looks like a reasonable temporary patch for a common case to me!

---

_@dcreager approved on 2025-12-09 21:26_

👍 thank you!

---

_Comment by @sharkdp on 2025-12-09 21:48_

Merging despite the failing benchmark run (timeout). Benchmarks were running on a previous version of this PR and I only made a completely trivial change since then.

---

_Merged by @sharkdp on 2025-12-09 21:49_

---

_Closed by @sharkdp on 2025-12-09 21:49_

---

_Branch deleted on 2025-12-09 21:49_

---
