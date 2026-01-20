```yaml
number: 22732
title: "Replace `NEXT_RUFF_VERSION` with rooster in releases"
type: pull_request
state: open
author: ntBre
labels:
  - release
assignees: []
base: main
head: brent/rooster-rule-versions
created_at: 2026-01-19T19:43:33Z
updated_at: 2026-01-20T18:25:08Z
url: https://github.com/astral-sh/ruff/pull/22732
synced_at: 2026-01-20T18:40:22Z
```

# Replace `NEXT_RUFF_VERSION` with rooster in releases

---

_@ntBre_

Summary
--

This PR updates our rooster config to replace `NEXT_RUFF_VERSION` with the new
version when doing releases. This will be helpful for the `preview_since`
entries (and even others like `stable_since`) in the `violation_metadata` macro,
which we currently update manually.

This is the variable name that we discussed briefly on Discord, but something shorter might be nice too? ~~I guess we may also want a check somewhere that there's not a typo preventing the version from ever being updated?~~

I went ahead and added this validation to the macro.

Test Plan
--

The first commit is the entirety of the rooster config change. The second commit
modifies an existing rule to use the `NEXT_RUFF_VERSION` field, and then the
third commit shows the result of a `./scripts/release.sh` run that successfully
updates the rule metadata. The fourth commit reverts the test release.


---

_Label `release` added by @ntBre on 2026-01-19 19:43_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5411 diagnostics
+ Found 5406 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:22_


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

_Marked ready for review by @ntBre on 2026-01-19 20:28_

---

_@amyreese approved on 2026-01-19 20:45_

---

_Review requested from @MichaReiser by @ntBre on 2026-01-20 00:29_

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/violation_metadata.rs`:136 on 2026-01-20 07:15_

What's the reason that we allow the `v` prefix? Can't we standardize around one format? 



---

_@MichaReiser approved on 2026-01-20 07:15_

---

_@ntBre reviewed on 2026-01-20 13:35_

---

_Review comment by @ntBre on `crates/ruff_macros/src/violation_metadata.rs`:136 on 2026-01-20 13:35_

All of our versions before 0.5.0 have a `v` prefix, so this was just to allow those. I narrowed this a bit more to allow the `v` only for `v0.[0-4].\d+`.

---
