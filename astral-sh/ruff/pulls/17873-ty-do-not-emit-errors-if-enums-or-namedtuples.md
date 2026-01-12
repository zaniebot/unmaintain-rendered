```yaml
number: 17873
title: "[ty] Do not emit errors if enums or NamedTuples constructed using functional syntax are used in type expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/enum-nmtpl-type-expr
created_at: 2025-05-05T22:45:18Z
updated_at: 2025-05-05T23:37:26Z
url: https://github.com/astral-sh/ruff/pull/17873
synced_at: 2026-01-12T15:56:07Z
```

# [ty] Do not emit errors if enums or NamedTuples constructed using functional syntax are used in type expressions

---

_@AlexWaygood_

## Summary

This fixes some false positives that showed up in the primer diff for https://github.com/astral-sh/ruff/pull/17832

## Test Plan

new mdtests added that fail with false-positive diagnostics on `main`


---

_Label `ty` added by @AlexWaygood on 2025-05-05 22:45_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-05 22:45_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-05 22:45_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-05 22:45_

---

_Comment by @github-actions[bot] on 2025-05-05 22:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- error[lint:invalid-type-form] src/flake8/options/manager.py:46:34: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:47:33: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:49:47: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:50:24: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:51:36: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:52:21: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:53:28: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:54:22: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:55:34: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:56:21: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:57:24: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:58:26: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] src/flake8/options/manager.py:145:45: Variable of type `Enum` is not allowed in a type expression
- Found 80 diagnostics
+ Found 67 diagnostics

isort (https://github.com/pycqa/isort)
- error[lint:invalid-type-form] isort/wrap.py:16:33: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] isort/wrap_modes.py:12:33: Variable of type `Enum` is not allowed in a type expression
- Found 50 diagnostics
+ Found 48 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[lint:invalid-type-form] cki_lib/stomp.py:30:31: Variable of type `Enum` is not allowed in a type expression
- Found 191 diagnostics
+ Found 190 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[lint:invalid-type-form] run_project_tests.py:324:16: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] run_project_tests.py:629:20: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] run_project_tests.py:1026:26: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] run_project_tests.py:1065:31: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] run_tests.py:83:67: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] run_tests.py:217:39: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] run_tests.py:241:39: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] run_tests.py:258:35: Variable of type `Enum` is not allowed in a type expression
- Found 1695 diagnostics
+ Found 1687 diagnostics

mypy (https://github.com/python/mypy)
- error[lint:invalid-type-form] mypy/stubgenc.py:317:79: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] mypy/stubgenc.py:337:57: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] mypy/stubgenc.py:352:56: Variable of type `Enum` is not allowed in a type expression
- Found 3275 diagnostics
+ Found 3272 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[lint:invalid-type-form] aiohttp/client.py:187:36: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/client.py:286:24: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/client.py:444:39: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/client.py:822:41: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/client.py:870:41: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/connector.py:244:34: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/connector.py:852:47: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/connector.py:1472:34: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/connector.py:1528:34: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/helpers.py:741:44: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/payload.py:160:40: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:191:28: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:192:34: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:193:38: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:194:28: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:195:26: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:196:28: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:197:37: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:822:28: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:823:34: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:824:38: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:825:28: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:826:26: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:827:28: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] aiohttp/web_request.py:828:37: Variable of type `Enum` is not allowed in a type expression
- Found 237 diagnostics
+ Found 212 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[lint:invalid-type-form] testing/test_compat.py:164:8: Variable of type `Enum` is not allowed in a type expression
- Found 918 diagnostics
+ Found 917 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:invalid-type-form] ddtrace/internal/packages.py:218:69: Variable of type `NamedTuple` is not allowed in a type expression
- error[lint:invalid-type-form] ddtrace/internal/packages.py:231:57: Variable of type `NamedTuple` is not allowed in a type expression
- error[lint:invalid-type-form] tests/appsec/iast/aspects/aspect_utils.py:79:27: Variable of type `NamedTuple` is not allowed in a type expression
- Found 8774 diagnostics
+ Found 8771 diagnostics

jax (https://github.com/google/jax)
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:182:15: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:183:15: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:184:14: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:187:49: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:188:28: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:189:27: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:255:15: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:256:15: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:257:14: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:261:9: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:442:36: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:443:23: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/pretty_printer.py:444:22: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/shard_map.py:329:30: Variable of type `Enum` is not allowed in a type expression
- error[lint:invalid-type-form] jax/_src/shard_map.py:402:17: Variable of type `Enum` is not allowed in a type expression
- Found 5483 diagnostics
+ Found 5468 diagnostics

```
</details>


---

_@carljm approved on 2025-05-05 23:30_

---

_Merged by @AlexWaygood on 2025-05-05 23:37_

---

_Closed by @AlexWaygood on 2025-05-05 23:37_

---

_Branch deleted on 2025-05-05 23:37_

---
