```yaml
number: 20011
title: "`unnecessary-default-type-args (UP043)` isn't triggering on stubs on Python <3.13"
type: issue
state: closed
author: Avasam
labels:
  - rule
assignees: []
created_at: 2025-08-20T21:35:05Z
updated_at: 2025-09-09T12:57:28Z
url: https://github.com/astral-sh/ruff/issues/20011
synced_at: 2026-01-12T15:54:57Z
```

# `unnecessary-default-type-args (UP043)` isn't triggering on stubs on Python <3.13

---

_@Avasam_

### Summary

`UP043` doesn't trigger at all on typeshed. I guess because it doesn't ignore the Python version in stubs? (relates to https://github.com/astral-sh/ruff/issues/19566) The rule isn't in preview.
```
(typeshed) PS ...\typeshed> uv run ruff check --select=UP043
All checks passed!
```

```
typeshed) PS ...\typeshed> uv run ruff check --select=UP043 --target-version=py313
Found 274 errors (274 fixed, 0 remaining).
```

<img width="450" height="987" alt="Image" src="https://github.com/user-attachments/assets/ba75656c-e34c-486e-943d-b33d80299ad1" />

### Version

ruff 0.12.9

---

_Renamed from "`unnecessary-default-type-args (UP043)` isn't triggering on stubs" to "`unnecessary-default-type-args (UP043)` isn't triggering on stubs on Python <3.13" by @Avasam on 2025-08-20 21:40_

---

_Comment by @ntBre on 2025-08-21 13:04_

Yep, looks like it always requires 3.13+:

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L144-L148

I think the fix would be as easy as adding `|| checker.semantic().in_stub_file()` here, if these are always valid in stub files.

---

_Label `rule` added by @ntBre on 2025-08-21 13:04_

---

_Closed by @dylwil3 on 2025-09-09 12:57_

---
