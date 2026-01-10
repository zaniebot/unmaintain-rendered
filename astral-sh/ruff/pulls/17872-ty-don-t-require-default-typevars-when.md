```yaml
number: 17872
title: "[ty] Don't require default typevars when specializing"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/specialize-default
created_at: 2025-05-05T21:57:40Z
updated_at: 2025-05-05T22:29:33Z
url: https://github.com/astral-sh/ruff/pull/17872
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Don't require default typevars when specializing

---

_Pull request opened by @dcreager on 2025-05-05 21:57_

If a typevar is declared as having a default, we shouldn't require a type to be specified for that typevar when explicitly specializing a generic class:

```py
class WithDefault[T, U = int]: ...

reveal_type(WithDefault[str]())  # revealed: WithDefault[str, int]
```

---

_Review requested from @carljm by @dcreager on 2025-05-05 21:57_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-05 21:57_

---

_Review requested from @sharkdp by @dcreager on 2025-05-05 21:57_

---

_Label `ty` added by @dcreager on 2025-05-05 21:57_

---

_Comment by @github-actions[bot] on 2025-05-05 22:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy (https://github.com/python/mypy)
- error[lint:missing-argument] mypy/typeshed/stdlib/email/message.pyi:152:89: No argument provided for required parameter `_HeaderRegistryParamT` of class `MIMEPart`
- error[lint:missing-argument] mypy/typeshed/stdlib/email/message.pyi:158:38: No argument provided for required parameter `_HeaderRegistryParamT` of class `MIMEPart`
- Found 3277 diagnostics
+ Found 3275 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- error[lint:missing-argument] django-stubs/contrib/admin/filters.pyi:38:62: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/options.pyi:130:53: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/options.pyi:148:66: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/options.pyi:214:61: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/options.pyi:230:16: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/options.pyi:233:81: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/options.pyi:287:41: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/options.pyi:328:53: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/sites.pyi:81:56: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/utils.pyi:40:36: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/admin/utils.pyi:62:10: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/postgres/expressions.pyi:12:42: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/sitemaps/__init__.pyi:37:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/sitemaps/__init__.pyi:42:44: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/contrib/sitemaps/__init__.pyi:47:24: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:15:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:21:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:27:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:33:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:39:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:45:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:61:21: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:65:24: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:67:52: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:79:78: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/deletion.pyi:97:10: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:30:45: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:35:49: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:36:16: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:38:55: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:39:16: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:65:45: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:70:49: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:71:16: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:73:55: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:74:16: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:115:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:122:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:179:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/fields/related_descriptors.pyi:186:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:34:31: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:35:22: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:115:23: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:116:52: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:117:53: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:118:50: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:121:57: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:122:47: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:123:45: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:126:10: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:127:47: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:128:50: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:129:54: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:130:51: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:131:46: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:132:46: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:142:10: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:143:26: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:144:38: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:145:37: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/manager.pyi:146:43: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/query.pyi:22:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/db/models/query.pyi:25:34: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/forms/models.pyi:115:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/forms/models.pyi:123:19: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/forms/models.pyi:129:31: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/forms/models.pyi:186:19: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/forms/models.pyi:264:15: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/forms/models.pyi:269:33: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/forms/models.pyi:302:48: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/forms/models.pyi:304:36: No argument provided for required parameter `_Row` of class `QuerySet`
- error[lint:missing-argument] django-stubs/views/generic/dates.pyi:109:36: No argument provided for required parameter `_Row` of class `QuerySet`
- Found 1465 diagnostics
+ Found 1393 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[lint:missing-argument] static_frame/core/quilt.py:1248:22: No arguments provided for required parameters `TVIndex`, `TVColumns` of class `InterGetItemLocCompoundReduces`
- error[lint:missing-argument] static_frame/core/util.py:305:9: No argument provided for required parameter `_InexactItemT_co` of class `inexact`
- error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:304:38: Argument to this function is incorrect: Expected `NBitBase`, found `integer[Unknown]`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:304:55: Argument to this function is incorrect: Expected `NBitBase`, found `number[_32Bit, int | float | complex]`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:304:66: No argument provided for required parameter `_NumberItemT_co` of class `number`
- error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:307:38: Argument to this function is incorrect: Expected `NBitBase`, found `integer[Unknown]`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:307:55: Argument to this function is incorrect: Expected `NBitBase`, found `number[_64Bit, int | float | complex]`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:307:66: No argument provided for required parameter `_NumberItemT_co` of class `number`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:314:38: No argument provided for required parameter `_InexactItemT_co` of class `inexact`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:326:50: Argument to this function is incorrect: Expected `NBitBase`, found `number[_64Bit, int | float | complex]`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:329:50: Argument to this function is incorrect: Expected `NBitBase`, found `number[_32Bit, int | float | complex]`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:326:50: No argument provided for required parameter `_InexactItemT_co` of class `inexact`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:326:61: No argument provided for required parameter `_NumberItemT_co` of class `number`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:329:50: No argument provided for required parameter `_InexactItemT_co` of class `inexact`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:329:61: No argument provided for required parameter `_NumberItemT_co` of class `number`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:362:38: No argument provided for required parameter `_InexactItemT_co` of class `inexact`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2305:16: Argument to this function is incorrect: Expected `generic[Any]`, found `number[Any, int | float | complex]`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:2305:39: No argument provided for required parameter `_NumberItemT_co` of class `number`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2306:16: Argument to this function is incorrect: Expected `generic[Any]`, found `number[Any, int | float | complex]`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:2306:39: No argument provided for required parameter `_NumberItemT_co` of class `number`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2307:18: Argument to this function is incorrect: Expected `generic[Any]`, found `number[Any, int | float | complex]`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:2307:41: No argument provided for required parameter `_NumberItemT_co` of class `number`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:2384:33: No argument provided for required parameter `_NumberItemT_co` of class `number`
- error[lint:missing-argument] static_frame/test/unit/test_type_clinic.py:2385:33: No argument provided for required parameter `_NumberItemT_co` of class `number`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2389:16: Argument to this function is incorrect: Expected `generic[Any]`, found `T2`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2389:26: Argument to this function is incorrect: Expected `generic[Any]`, found `T1`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2390:16: Argument to this function is incorrect: Expected `generic[Any]`, found `T2`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2390:26: Argument to this function is incorrect: Expected `generic[Any]`, found `T1`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2391:18: Argument to this function is incorrect: Expected `generic[Any]`, found `T2`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2391:28: Argument to this function is incorrect: Expected `generic[Any]`, found `T1`
- Found 5650 diagnostics
+ Found 5646 diagnostics

scipy (https://github.com/scipy/scipy)
- error[lint:missing-argument] scipy/stats/_sensitivity_analysis.py:25:52: No argument provided for required parameter `_InexactItemT_co` of class `inexact`
- Found 37094 diagnostics
+ Found 37093 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-05-05 22:05_

could you possibly rebase this on `main`? It would make it easier for me to rebase #17832 on top of it (I need https://github.com/astral-sh/ruff/commit/bb6c7cad0731905dd9a11dd1e22b1a85089ab2d8 to reduce false positive diagnostics on #17832)

EDIT: or you could just land it 😄

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/classes.md`:142 on 2025-05-05 22:11_

Is it worth also adding an assertion that explicitly shows that you can still specify a type argument for the typevar-with-default, and that we respect it if you do?

````suggestion
If the type variable has a default, it can be omitted:

```py
class WithDefault[T, U = int]: ...

reveal_type(WithDefault[str, str]())  # revealed: WithDefault[str, str]
reveal_type(WithDefault[str]())  # revealed: WithDefault[str, int]
```
````

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:161 on 2025-05-05 22:11_

```suggestion
reveal_type(WithDefault[str, str]())  # revealed: WithDefault[str, str]
reveal_type(WithDefault[str]())  # revealed: WithDefault[str, int]
```

---

_@AlexWaygood approved on 2025-05-05 22:12_

LGTM!

---

_@dcreager reviewed on 2025-05-05 22:19_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/classes.md`:142 on 2025-05-05 22:19_

Good call!

---

_Comment by @dcreager on 2025-05-05 22:20_

> could you possibly rebase this on `main`? It would make it easier for me to rebase #17832 on top of it (I need [bb6c7ca](https://github.com/astral-sh/ruff/commit/bb6c7cad0731905dd9a11dd1e22b1a85089ab2d8) to reduce false positive diagnostics on #17832)

done

---

_@carljm approved on 2025-05-05 22:26_

---

_Merged by @dcreager on 2025-05-05 22:29_

---

_Closed by @dcreager on 2025-05-05 22:29_

---

_Branch deleted on 2025-05-05 22:29_

---
