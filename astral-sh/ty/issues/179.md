```yaml
number: 179
title: "Consider allowing globs for `src.root` or `environment.extra-paths`"
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2025-03-14T08:56:27Z
updated_at: 2025-06-24T12:52:38Z
url: https://github.com/astral-sh/ty/issues/179
synced_at: 2026-01-10T02:08:20Z
```

# Consider allowing globs for `src.root` or `environment.extra-paths`

---

_Issue opened by @MichaReiser on 2025-03-14 08:56_

Mono repos with multiple packages may need multiple src roots (see https://github.com/astral-sh/ruff/issues/16712) 

We may want to change `src.root` to a list and allow globs and OR support globs in `extra-paths`.

---

_Renamed from "[red-knot] Consider allowing globs for `src.root` or `environment.extra-paths`" to "Consider allowing globs for `src.root` or `environment.extra-paths`" by @MichaReiser on 2025-05-07 15:26_

---

_Label `configuration` added by @AlexWaygood on 2025-05-11 07:27_

---

_Comment by @sarimak on 2025-05-12 12:18_

And adjust the documentation accordingly, please -- currently it claims that `list[str]` is supported but it isn't:
https://github.com/astral-sh/ty/blob/main/docs/configuration.md#root vs.

```
$ ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
ty failed
  Cause: .../pyproject.toml is not a valid `pyproject.toml`: TOML parse error at line 20, column 8
   |
20 | root = ["./bases", "./components"]
   |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^
invalid type: sequence, expected path string

  Cause: TOML parse error at line 20, column 8
   |
20 | root = ["./bases", "./components"]
   |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^
invalid type: sequence, expected path string
```

---

_Comment by @MichaReiser on 2025-05-12 12:26_

Thanks. https://github.com/astral-sh/ruff/pull/18040 fixes the docs. 

---

_Comment by @carljm on 2025-05-16 12:12_

#414 is another use case for this. The popular `pytest` test framework adds the `tests/` directory to the import search path, so if a pytest user wants to check their main source code (in say `src/`) and their tests, they would want multiple `src.root`.

---

_Assigned to @MichaReiser by @MichaReiser on 2025-06-18 14:16_

---

_Closed by @MichaReiser on 2025-06-24 12:52_

---
