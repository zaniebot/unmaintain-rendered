```yaml
number: 21866
title: "[ty] Use default settings in completion tests"
type: pull_request
state: merged
author: BurntSushi
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: ag/switch-default-in-completion-tests
created_at: 2025-12-09T15:01:08Z
updated_at: 2025-12-09T15:42:47Z
url: https://github.com/astral-sh/ruff/pull/21866
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Use default settings in completion tests

---

_@BurntSushi_

This makes it so the test and production environments match.

Ref https://github.com/astral-sh/ruff/pull/21851#discussion_r2601579316


---

_Review requested from @carljm by @BurntSushi on 2025-12-09 15:01_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-09 15:01_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-09 15:01_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-09 15:01_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-09 15:01_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-09 15:01_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-09 15:01_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-09 15:01_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-12-09 15:01_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 15:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `testing` added by @AlexWaygood on 2025-12-09 15:03_

---

_Label `ty` added by @AlexWaygood on 2025-12-09 15:03_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 15:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/testing/internal/test_data.py:148:15: warning[unsupported-base] Unsupported class base with type `<class 'TestItem[Test, Never]'> | <class 'TestItem[Unknown, Unknown]'>`
+ tests/testing/internal/test_telemetry.py:392:9: error[invalid-assignment] Implicit shadowing of function `is_benchmark`


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser approved on 2025-12-09 15:34_

Thank you

---

_Merged by @BurntSushi on 2025-12-09 15:42_

---

_Closed by @BurntSushi on 2025-12-09 15:42_

---

_Branch deleted on 2025-12-09 15:42_

---
