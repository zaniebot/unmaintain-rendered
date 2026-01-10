```yaml
number: 18087
title: "[ty] Reduce log level of 'symbol .. (via star import) not found' log message"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/star-import-log
created_at: 2025-05-14T06:58:44Z
updated_at: 2025-05-14T07:20:25Z
url: https://github.com/astral-sh/ruff/pull/18087
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Reduce log level of 'symbol .. (via star import) not found' log message

---

_Pull request opened by @MichaReiser on 2025-05-14 06:58_

## Summary

This seems pretty common and it now floods the console with debug messages, making `-vv` less useful. 





---

_Review requested from @carljm by @MichaReiser on 2025-05-14 06:58_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-14 06:58_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-14 06:58_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-14 06:58_

---

_Label `internal` added by @MichaReiser on 2025-05-14 06:58_

---

_Label `ty` added by @MichaReiser on 2025-05-14 06:58_

---

_Renamed from "Reduce log level of 'symbol .. (via star import) not found' log message" to "[ty]] Reduce log level of 'symbol .. (via star import) not found' log message" by @MichaReiser on 2025-05-14 06:58_

---

_Renamed from "[ty]] Reduce log level of 'symbol .. (via star import) not found' log message" to "[ty] Reduce log level of 'symbol .. (via star import) not found' log message" by @MichaReiser on 2025-05-14 06:59_

---

_Comment by @github-actions[bot] on 2025-05-14 07:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 647 diagnostics
+ Found 649 diagnostics

```
</details>


---

_@sharkdp approved on 2025-05-14 07:13_

---

_Merged by @MichaReiser on 2025-05-14 07:20_

---

_Closed by @MichaReiser on 2025-05-14 07:20_

---

_Branch deleted on 2025-05-14 07:20_

---
