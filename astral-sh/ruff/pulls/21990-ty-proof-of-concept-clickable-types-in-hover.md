```yaml
number: 21990
title: "[ty] proof of concept: clickable types in hover"
type: pull_request
state: open
author: Gankra
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: gankra/clickdoc
created_at: 2025-12-15T19:22:15Z
updated_at: 2025-12-15T19:26:05Z
url: https://github.com/astral-sh/ruff/pull/21990
synced_at: 2026-01-10T16:42:11Z
```

# [ty] proof of concept: clickable types in hover

---

_Pull request opened by @Gankra on 2025-12-15 19:22_

## Summary

This is a proof of concept for rendering types as a series of hyperlinks in hovers:

<img width="485" height="72" alt="Screenshot 2025-12-15 at 2 19 20 PM" src="https://github.com/user-attachments/assets/91cfd72e-0351-4cdc-9a37-574c60c993c9" />

<img width="204" height="75" alt="Screenshot 2025-12-15 at 2 19 09 PM" src="https://github.com/user-attachments/assets/71b1bb45-901e-481b-a8df-ca40396ca24c" />

* A silly solution to https://github.com/astral-sh/ty/issues/1575

## Test Plan

Messing around in IDE.


---

_Label `server` added by @Gankra on 2025-12-15 19:22_

---

_Label `ty` added by @Gankra on 2025-12-15 19:22_

---

_Comment by @astral-sh-bot[bot] on 2025-12-15 19:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @Gankra on 2025-12-15 19:24_

The key insight is `[MyType](file://x/y.py#L1C3-L10C13)` works in markdown rendering -- at least in vscode.

The rest is just copy-pasting the inlay hint goto-def machinery.

---

_Comment by @astral-sh-bot[bot] on 2025-12-15 19:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---
