```yaml
number: 19174
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-07-07T09:25:31Z
updated_at: 2025-07-07T10:00:10Z
url: https://github.com/astral-sh/ruff/pull/19174
synced_at: 2026-01-12T15:56:33Z
```

# [ty] Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Label `ty` added by @github-actions[bot] on 2025-07-07 09:25_

---

_Review requested from @carljm by @github-actions[bot] on 2025-07-07 09:25_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-07-07 09:25_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-07-07 09:25_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-07-07 09:25_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-07-07 09:25_

---

_@sharkdp reviewed on 2025-07-07 09:26_

---

_Review comment by @sharkdp on `crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi`:710 on 2025-07-07 09:26_

I ran the typeshed sync workflow manually to get [this change](https://github.com/python/typeshed/pull/14372) in, to prevent new false positives in https://github.com/astral-sh/ruff/pull/19142

---

_Closed by @sharkdp on 2025-07-07 09:30_

---

_Reopened by @sharkdp on 2025-07-07 09:30_

---

_Comment by @github-actions[bot] on 2025-07-07 09:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
- TOTAL MEMORY USAGE: ~28MB
+ TOTAL MEMORY USAGE: ~30MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~49MB
+     memo fields = ~54MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

porcupine (https://github.com/Akuli/porcupine)
-     memo fields = ~97MB
+     memo fields = ~106MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[invalid-argument-type] pymongo/client_options.py:56:41: Argument to function `_build_credentials_tuple` is incorrect: Expected `str`, found `Any | Literal["DEFAULT"] | None`
- error[invalid-return-type] pymongo/client_options.py:145:16: Return type does not match returned value: expected `tuple[SSLContext | None, bool]`, found `tuple[SSLContext | SSLContext | Unknown, Any | Literal[False]]`
+ error[invalid-return-type] pymongo/client_options.py:145:16: Return type does not match returned value: expected `tuple[SSLContext | None, bool]`, found `tuple[SSLContext | SSLContext | Unknown, Any]`
- Found 430 diagnostics
+ Found 429 diagnostics

colour (https://github.com/colour-science/colour)
- error[invalid-assignment] colour/geometry/primitives.py:142:5: Object of type `Unknown | LiteralString` is not assignable to `Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"]`
- Found 488 diagnostics
+ Found 487 diagnostics

flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~60MB
+     memo fields = ~66MB

ignite (https://github.com/pytorch/ignite)
+ warning[redundant-cast] ignite/handlers/visdom_logger.py:182:22: Value is already of type `str`
- Found 2137 diagnostics
+ Found 2138 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-return-type] pwndbg/lib/cache.py:160:28: Return type does not match returned value: expected `T`, found `object`
- Found 2275 diagnostics
+ Found 2274 diagnostics

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

altair (https://github.com/vega/altair)
-     memo fields = ~251MB
+     memo fields = ~228MB

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-return-type] src/_pytest/skipping.py:158:12: Return type does not match returned value: expected `tuple[bool, str]`, found `tuple[Any | bool, Any | None | str]`
- Found 528 diagnostics
+ Found 527 diagnostics
- TOTAL MEMORY USAGE: ~251MB
+ TOTAL MEMORY USAGE: ~276MB

static-frame (https://github.com/static-frame/static-frame)
- TOTAL MEMORY USAGE: ~405MB
+ TOTAL MEMORY USAGE: ~445MB

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-argument-type] tests/appsec/iast/fixtures/integration/main_configure_wrong.py:27:22: Argument to bound method `configure` is incorrect: Expected `bool | None`, found `str | Unknown`
+ error[invalid-argument-type] tests/appsec/iast/fixtures/integration/main_configure_wrong.py:27:22: Argument to bound method `configure` is incorrect: Expected `bool | None`, found `str`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
-     memo fields = ~539MB
+     memo fields = ~593MB

```
</details>


---

_Merged by @sharkdp on 2025-07-07 10:00_

---

_Closed by @sharkdp on 2025-07-07 10:00_

---

_Branch deleted on 2025-07-07 10:00_

---
