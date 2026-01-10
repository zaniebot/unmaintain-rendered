```yaml
number: 20175
title: "[ty] Optimize protocol assignability/subtyping"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: alex/proto-optimize
created_at: 2025-08-31T12:08:39Z
updated_at: 2025-08-31T12:22:27Z
url: https://github.com/astral-sh/ruff/pull/20175
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Optimize protocol assignability/subtyping

---

_Pull request opened by @AlexWaygood on 2025-08-31 12:08_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `performance` added by @AlexWaygood on 2025-08-31 12:08_

---

_Label `ty` added by @AlexWaygood on 2025-08-31 12:08_

---

_Comment by @github-actions[bot] on 2025-08-31 12:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-31 12:10:25.307323182 +0000
+++ new-output.txt	2025-08-31 12:10:28.000351700 +0000
@@ -730,10 +730,8 @@
 protocols_class_objects.py:58:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
 protocols_class_objects.py:59:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
 protocols_definition.py:114:1: error[invalid-assignment] Object of type `Concrete2_Bad1` is not assignable to `Template2`
-protocols_definition.py:115:1: error[invalid-assignment] Object of type `Concrete2_Bad2` is not assignable to `Template2`
 protocols_definition.py:116:1: error[invalid-assignment] Object of type `Concrete2_Bad3` is not assignable to `Template2`
 protocols_definition.py:156:1: error[invalid-assignment] Object of type `Concrete3_Bad1` is not assignable to `Template3`
-protocols_definition.py:159:1: error[invalid-assignment] Object of type `Concrete3_Bad4` is not assignable to `Template3`
 protocols_definition.py:160:1: error[invalid-assignment] Object of type `Concrete3_Bad5` is not assignable to `Template3`
 protocols_definition.py:219:1: error[invalid-assignment] Object of type `Concrete4_Bad2` is not assignable to `Template4`
 protocols_merging.py:52:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable1`
@@ -858,5 +856,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 859 diagnostics
+Found 857 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-31 12:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Closed by @AlexWaygood on 2025-08-31 12:22_

---

_Branch deleted on 2025-08-31 12:22_

---
