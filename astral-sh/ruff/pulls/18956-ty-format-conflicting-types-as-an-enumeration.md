```yaml
number: 18956
title: "[ty] Format conflicting types as an enumeration"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/human-readable-enumeration
created_at: 2025-06-26T12:07:08Z
updated_at: 2025-06-26T12:29:35Z
url: https://github.com/astral-sh/ruff/pull/18956
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Format conflicting types as an enumeration

---

_@sharkdp_

## Summary

Format conflicting declared types as
```
`str`, `int` and `bytes`
```

Thanks to @AlexWaygood for the initial draft.

@dcreager, looking forward to your one-character follow-up PR.

---

_Review requested from @carljm by @sharkdp on 2025-06-26 12:07_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-26 12:07_

---

_Review requested from @dcreager by @sharkdp on 2025-06-26 12:07_

---

_Label `internal` added by @sharkdp on 2025-06-26 12:07_

---

_Label `ty` added by @sharkdp on 2025-06-26 12:07_

---

_Label `diagnostics` added by @sharkdp on 2025-06-26 12:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:2019 on 2025-06-26 12:09_

Possibly https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/util/diagnostics.rs might be a better home for this helper, since it doesn't have much to do with _types_ specifically?

---

_@AlexWaygood approved on 2025-06-26 12:10_

Nice! There might be some pre-existing diagnostics that could also make use of this

---

_Comment by @github-actions[bot] on 2025-06-26 12:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- error[conflicting-declarations] pydantic/v1/schema.py:1143:17: Conflicting declared types for `constraint_func`: ((...) -> type) | None, def constraint_func(**kw: Any) -> type[Any], def constraint_func(**kw: Any) -> type[Any], def constraint_func(**kw: Any) -> type[Any]
+ error[conflicting-declarations] pydantic/v1/schema.py:1143:17: Conflicting declared types for `constraint_func`: `((...) -> type) | None`, `def constraint_func(**kw: Any) -> type[Any]`, `def constraint_func(**kw: Any) -> type[Any]` and `def constraint_func(**kw: Any) -> type[Any]`

cloud-init (https://github.com/canonical/cloud-init)
- error[conflicting-declarations] cloudinit/handlers/jinja_template.py:28:5: Conflicting declared types for `JUndefinedError`: type[Exception], <class 'UndefinedError'>
+ error[conflicting-declarations] cloudinit/handlers/jinja_template.py:28:5: Conflicting declared types for `JUndefinedError`: `type[Exception]` and `<class 'UndefinedError'>`

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[conflicting-declarations] src/typeshed_stats/_cli.py:83:17: Conflicting declared types for `pprint`: (object, /) -> None, Unknown
+ error[conflicting-declarations] src/typeshed_stats/_cli.py:83:17: Conflicting declared types for `pprint`: `(object, /) -> None` and `Unknown`

vision (https://github.com/pytorch/vision)
- error[conflicting-declarations] torchvision/datasets/folder.py:80:5: Conflicting declared types for `is_valid_file`: ((str, /) -> bool) | None, def is_valid_file(x: str) -> bool
+ error[conflicting-declarations] torchvision/datasets/folder.py:80:5: Conflicting declared types for `is_valid_file`: `((str, /) -> bool) | None` and `def is_valid_file(x: str) -> bool`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[conflicting-declarations] benchmarks/appsec_iast_aspects/scenario.py:25:13: Conflicting declared types for `set_iast_request_enabled`: def set_iast_request_enabled(request_enabled) -> None, Unknown
+ error[conflicting-declarations] benchmarks/appsec_iast_aspects/scenario.py:25:13: Conflicting declared types for `set_iast_request_enabled`: `def set_iast_request_enabled(request_enabled) -> None` and `Unknown`
- error[conflicting-declarations] benchmarks/appsec_iast_aspects_ospath/scenario.py:23:13: Conflicting declared types for `set_iast_request_enabled`: def set_iast_request_enabled(request_enabled) -> None, Unknown
+ error[conflicting-declarations] benchmarks/appsec_iast_aspects_ospath/scenario.py:23:13: Conflicting declared types for `set_iast_request_enabled`: `def set_iast_request_enabled(request_enabled) -> None` and `Unknown`
- error[conflicting-declarations] benchmarks/appsec_iast_aspects_re_module/scenario.py:23:13: Conflicting declared types for `set_iast_request_enabled`: def set_iast_request_enabled(request_enabled) -> None, Unknown
+ error[conflicting-declarations] benchmarks/appsec_iast_aspects_re_module/scenario.py:23:13: Conflicting declared types for `set_iast_request_enabled`: `def set_iast_request_enabled(request_enabled) -> None` and `Unknown`
- error[conflicting-declarations] benchmarks/appsec_iast_aspects_split/scenario.py:23:13: Conflicting declared types for `set_iast_request_enabled`: def set_iast_request_enabled(request_enabled) -> None, Unknown
+ error[conflicting-declarations] benchmarks/appsec_iast_aspects_split/scenario.py:23:13: Conflicting declared types for `set_iast_request_enabled`: `def set_iast_request_enabled(request_enabled) -> None` and `Unknown`
- error[conflicting-declarations] benchmarks/appsec_iast_propagation/scenario.py:20:9: Conflicting declared types for `set_iast_request_enabled`: def set_iast_request_enabled(request_enabled) -> None, Unknown
+ error[conflicting-declarations] benchmarks/appsec_iast_propagation/scenario.py:20:9: Conflicting declared types for `set_iast_request_enabled`: `def set_iast_request_enabled(request_enabled) -> None` and `Unknown`

```
</details>


---

_Review requested from @MichaReiser by @sharkdp on 2025-06-26 12:23_

---

_Comment by @sharkdp on 2025-06-26 12:24_

> Nice! There might be some pre-existing diagnostics that could also make use of this

Nothing immediately came up while doing a superficial scan.

---

_Merged by @sharkdp on 2025-06-26 12:29_

---

_Closed by @sharkdp on 2025-06-26 12:29_

---

_Branch deleted on 2025-06-26 12:29_

---
