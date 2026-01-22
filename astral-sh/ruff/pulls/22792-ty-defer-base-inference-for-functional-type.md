```yaml
number: 22792
title: "[ty] Defer base inference for functional `type(...)` classes"
type: pull_request
state: open
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
base: main
head: charlie/defer-bases
created_at: 2026-01-22T01:26:17Z
updated_at: 2026-01-22T01:29:12Z
url: https://github.com/astral-sh/ruff/pull/22792
synced_at: 2026-01-22T02:09:04Z
```

# [ty] Defer base inference for functional `type(...)` classes

---

_@charliermarsh_


## Summary

Closes https://github.com/astral-sh/ty/issues/2564.


---

_Label `bug` added by @charliermarsh on 2026-01-22 01:26_

---

_Marked ready for review by @charliermarsh on 2026-01-22 01:26_

---

_Review requested from @carljm by @charliermarsh on 2026-01-22 01:26_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-22 01:26_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-22 01:26_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-22 01:26_

---

_Label `ty` added by @charliermarsh on 2026-01-22 01:26_

---

_Comment by @astral-sh-bot[bot] on 2026-01-22 01:27_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-22 01:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | Coroutine[Any, Any, Unknown | None] | None` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2050 diagnostics
+ Found 2049 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, int | _R@ignore_variance | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14465 diagnostics
+ Found 14464 diagnostics


```

</details>


No memory usage changes detected ✅



---
