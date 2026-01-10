```yaml
number: 19459
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: closed
author: github-actions
labels:
  - ty
assignees: []
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-07-21T12:35:59Z
updated_at: 2025-07-21T12:42:59Z
url: https://github.com/astral-sh/ruff/pull/19459
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2025-07-21 12:35_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2025-07-21 12:36_

---

_Label `ty` added by @github-actions[bot] on 2025-07-21 12:36_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-07-21 12:36_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-07-21 12:36_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-07-21 12:36_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-07-21 12:36_

---

_Comment by @AlexWaygood on 2025-07-21 12:40_

oops I screwed this up with a bad force-push. I'll close this and redo it.

---

_Closed by @AlexWaygood on 2025-07-21 12:40_

---

_Branch deleted on 2025-07-21 12:40_

---

_Comment by @github-actions[bot] on 2025-07-21 12:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
streamlit (https://github.com/streamlit/streamlit)
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:991:9: Unresolved attribute `return_value` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:991:9: Unresolved attribute `return_value` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:993:9: Unresolved attribute `side_effect` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:993:9: Unresolved attribute `side_effect` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1017:9: Unresolved attribute `return_value` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1017:9: Unresolved attribute `return_value` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1019:9: Unresolved attribute `side_effect` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1019:9: Unresolved attribute `side_effect` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1055:9: Unresolved attribute `return_value` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1055:9: Unresolved attribute `return_value` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1057:9: Unresolved attribute `side_effect` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1057:9: Unresolved attribute `side_effect` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1103:9: Unresolved attribute `return_value` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1103:9: Unresolved attribute `return_value` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1105:9: Unresolved attribute `side_effect` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1105:9: Unresolved attribute `side_effect` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1129:9: Unresolved attribute `return_value` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1129:9: Unresolved attribute `return_value` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1131:9: Unresolved attribute `side_effect` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1131:9: Unresolved attribute `side_effect` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1165:9: Unresolved attribute `return_value` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1165:9: Unresolved attribute `return_value` on type `_patch_pass_arg[MagicMock | AsyncMock]`.
- error[unresolved-attribute] lib/tests/streamlit/config_test.py:1167:9: Unresolved attribute `side_effect` on type `_patch_default_new`.
+ error[unresolved-attribute] lib/tests/streamlit/config_test.py:1167:9: Unresolved attribute `side_effect` on type `_patch_pass_arg[MagicMock | AsyncMock]`.

```
</details>
No memory usage changes detected ✅


---
