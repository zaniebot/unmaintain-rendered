```yaml
number: 18286
title: "[ty] Add `tests` to `src.root` if it exists and is not a package"
type: pull_request
state: merged
author: j178
labels:
  - ty
assignees: []
merged: true
base: main
head: src-root-tests
created_at: 2025-05-24T13:55:50Z
updated_at: 2025-05-26T08:08:57Z
url: https://github.com/astral-sh/ruff/pull/18286
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Add `tests` to `src.root` if it exists and is not a package

---

_Pull request opened by @j178 on 2025-05-24 13:55_

## Summary

Closes https://github.com/astral-sh/ty/issues/414

## Test Plan

Test added.

---

_Review requested from @carljm by @j178 on 2025-05-24 13:55_

---

_Review requested from @MichaReiser by @j178 on 2025-05-24 13:55_

---

_Review requested from @AlexWaygood by @j178 on 2025-05-24 13:55_

---

_Review requested from @sharkdp by @j178 on 2025-05-24 13:55_

---

_Review requested from @dcreager by @j178 on 2025-05-24 13:55_

---

_Label `ty` added by @AlexWaygood on 2025-05-24 15:01_

---

_Comment by @github-actions[bot] on 2025-05-24 15:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
ppb-vector (https://github.com/ppb/ppb-vector)
- error[unresolved-import] tests/benchmark.py:5:6: Cannot resolve imported module `utils`
+ warning[unused-ignore-comment] tests/benchmark.py:13:58: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] tests/benchmark.py:16:45: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] tests/benchmark.py:19:27: Unused blanket `type: ignore` directive
- error[unresolved-import] tests/test_addition.py:5:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_angle.py:5:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_convert.py:5:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_ctor.py:6:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_decompose.py:8:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_dot.py:6:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_equality.py:4:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_isclose.py:8:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_length.py:7:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_member_access.py:4:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_memory.py:8:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_negation.py:4:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_normalize.py:3:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_project.py:6:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_reflect.py:7:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_rotate.py:9:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_scalar_multiplication.py:5:6: Cannot resolve imported module `utils`
+ warning[unused-ignore-comment] tests/test_typing.py:8:76: Unused blanket `type: ignore` directive
- error[unresolved-import] tests/test_scale.py:5:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_truncate.py:6:6: Cannot resolve imported module `utils`
- error[unresolved-import] tests/test_typing.py:5:6: Cannot resolve imported module `utils`
- error[unresolved-reference] tests/test_typing.py:9:10: Name `vectors` used when not defined
- error[unresolved-reference] tests/test_typing.py:9:23: Name `units` used when not defined
- error[unresolved-reference] tests/test_typing.py:14:19: Name `vector_likes` used when not defined
- error[unresolved-import] tests/test_update.py:4:6: Cannot resolve imported module `utils`
- Found 42 diagnostics
+ Found 21 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[unresolved-import] tests/test_buffers.py:10:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_builtin_casters.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_call_policies.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_callbacks.py:9:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_chrono.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_class.py:8:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_class_sh_disowning_mi.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_class_sh_property.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_class_sh_trampoline_shared_from_this.py:8:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_class_sh_trampoline_shared_ptr_cpp_arg.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_cmake_build/test.py:5:8: Cannot resolve imported module `test_cmake_build`
+ error[unresolved-attribute] tests/test_cmake_build/test.py:9:8: Type `<module 'test_cmake_build'>` has no attribute `add`
- error[unresolved-import] tests/test_copy_move.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_cpp_conduit.py:10:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_custom_type_casters.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_custom_type_setup.py:8:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_docs_advanced_cast_custom.py:6:10: Cannot resolve imported module `conftest`
- error[unresolved-import] tests/test_eigen_matrix.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_eigen_tensor.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_enum.py:8:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_eval.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_exceptions.py:6:6: Cannot resolve imported module `custom_exceptions`
- error[unresolved-import] tests/test_exceptions.py:8:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_factory_constructors.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_gil_scoped.py:10:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_kwargs_and_defaults.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_methods_and_attributes.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_modules.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_multiple_inheritance.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_native_enum.py:8:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_numpy_array.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_numpy_dtypes.py:7:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_opaque_types.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_operator_overloading.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_pickling.py:9:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_potentially_slicing_weak_ptr.py:39:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_pytypes.py:9:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_sequences_and_iterators.py:8:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_smart_ptr.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_stl.py:5:8: Cannot resolve imported module `env`
- error[unresolved-import] tests/test_virtual_functions.py:7:8: Cannot resolve imported module `env`
- Found 267 diagnostics
+ Found 228 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[unresolved-import] tests/test_serving.py:33:10: Cannot resolve imported module `conftest`
- error[unresolved-import] tests/test_serving.py:34:10: Cannot resolve imported module `conftest`
- Found 454 diagnostics
+ Found 452 diagnostics

```
</details>


---

_@MichaReiser approved on 2025-05-26 08:08_

Thank you

---

_Merged by @MichaReiser on 2025-05-26 08:08_

---

_Closed by @MichaReiser on 2025-05-26 08:08_

---
