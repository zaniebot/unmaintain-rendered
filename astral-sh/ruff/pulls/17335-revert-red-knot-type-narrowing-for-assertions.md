```yaml
number: 17335
title: "Revert \"[red-knot] Type narrowing for assertions (#17149)\""
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/revert-assert-narrowing
created_at: 2025-04-10T15:00:16Z
updated_at: 2025-04-10T15:06:27Z
url: https://github.com/astral-sh/ruff/pull/17335
synced_at: 2026-01-10T19:40:37Z
```

# Revert "[red-knot] Type narrowing for assertions (#17149)"

---

_Pull request opened by @carljm on 2025-04-10 15:00_

I merged #17149 without checking the ecosystem results, and it still caused a cycle panic in pybind11. Reverting for now until I fix that, so we don't lose the ecosystem signal on other PRs.

---

_Label `red-knot` added by @carljm on 2025-04-10 15:00_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-10 15:00_

---

_Review requested from @sharkdp by @carljm on 2025-04-10 15:00_

---

_Review requested from @dcreager by @carljm on 2025-04-10 15:00_

---

_Comment by @github-actions[bot] on 2025-04-10 15:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:97:22: Attribute `rsplit` on type `str | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:97:22: Attribute `rsplit` on type `str | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:97:22: Attribute `rsplit` on type `str | None` is possibly unbound
- Found 345 diagnostics
+ Found 348 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/sansio/test_multipart.py:127:16: Type `Event` has no attribute `more_data`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_http.py:728:12: Attribute `to_header` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:208:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["int("]` with `@Todo(generics) & Unknown & str | @Todo(generics) & Any & str | @Todo(generics) & str | @Todo(generics) & Unknown & None | @Todo(generics) & Any & None | @Todo(generics) & None`
- Found 768 diagnostics
+ Found 769 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- 
- thread '<unnamed>' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/function/fetch.rs:167:25:
- dependency graph cycle querying try_metaclass_(Id(dc04)); set cycle_fn/cycle_initial to fixpoint iterate
- note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
- 
- thread '<unnamed>' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/function/fetch.rs:167:25:
- dependency graph cycle querying member_lookup_with_policy_(Id(e86c)); set cycle_fn/cycle_initial to fixpoint iterate
- 
- thread '<unnamed>' panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/function/fetch.rs:167:25:
- dependency graph cycle querying try_metaclass_(Id(27021)); set cycle_fn/cycle_initial to fixpoint iterate
- Rayon: detected unexpected panic; aborting
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/pybind11/tests/test_eval_call.py:5:5: Name `call_test2` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/pybind11/tests/test_eval_call.py:5:16: Name `y` used when not defined
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_unique_ptr.py:3:8: Cannot resolve import `pybind11_tests.class_sh_trampoline_unique_ptr`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_embed/test_trampoline.py:3:8: Cannot resolve import `trampoline_module`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_async.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_type_caster_std_function_specializations.py:3:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_custom_type_setup.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_custom_type_setup.py:8:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_custom_type_setup.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_opaque_types.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_opaque_types.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_opaque_types.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_opaque_types.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_opaque_types.py:7:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_thread.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_thread.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pickling.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pickling.py:8:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pickling.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pickling.py:80:10: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_basic.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_basic.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_const_name.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_const_name.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_disowning_mi.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_disowning_mi.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_disowning_mi.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_disowning_mi.py:72:1: Cannot create a consistent method resolution order (MRO) for class `MI1` with bases list `[Unknown, Unknown]`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_disowning_mi.py:83:1: Cannot create a consistent method resolution order (MRO) for class `MI2` with bases list `[<class 'B1'>, Unknown, Unknown]`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_disowning_mi.py:102:1: Cannot create a consistent method resolution order (MRO) for class `MI5` with bases list `[Unknown, <class 'B1'>, Unknown]`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_cpp_conduit.py:5:8: Cannot resolve import `exo_planet_c_api`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_cpp_conduit.py:6:8: Cannot resolve import `exo_planet_pybind11`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_cpp_conduit.py:7:8: Cannot resolve import `home_planet_very_lonely_traveler`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_cpp_conduit.py:8:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_cpp_conduit.py:10:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_cpp_conduit.py:11:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_chrono.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_chrono.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_chrono.py:8:6: Cannot resolve import `pybind11_tests`
+ warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pybind11/pybind11/__main__.py:23:22: Name `UNSAFE` used when possibly not defined
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_tagbased_polymorphic.py:3:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_enum.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_enum.py:8:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_enum.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_callbacks.py:7:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_callbacks.py:9:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_callbacks.py:10:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_callbacks.py:11:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_unique_ptr_custom_deleter.py:3:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_union.py:3:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_unnamed_namespace_b.py:3:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_gil_scoped.py:8:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_gil_scoped.py:10:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_gil_scoped.py:11:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_virtual_py_cpp_mix.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_virtual_py_cpp_mix.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_sequences_and_iterators.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_sequences_and_iterators.py:6:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_sequences_and_iterators.py:8:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_sequences_and_iterators.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_sequences_and_iterators.py:10:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:7:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:168:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:168:10: Object of type `Literal[redirect_stdout]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:176:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:176:10: Object of type `Literal[redirect_stdout]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:187:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:187:10: Object of type `Literal[redirect_stderr]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:195:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:195:10: Object of type `Literal[redirect_stderr]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:205:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:205:10: Object of type `Literal[redirect_stdout]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:225:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:225:10: Object of type `Literal[redirect_stdout]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:232:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:232:10: Object of type `Literal[redirect_stdout]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:239:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:239:10: Object of type `Literal[redirect_stdout]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:251:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:251:10: Object of type `Literal[redirect_stderr]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:266:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:266:10: Object of type `Literal[redirect_stdout]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:266:35: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_iostream.py:266:35: Object of type `Literal[redirect_stderr]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_virtual_functions.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_virtual_functions.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_virtual_functions.py:10:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_property_non_owning.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_property_non_owning.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl_binders.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl_binders.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl_binders.py:130:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_release_gil_before_calling_cpp_dtor.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_release_gil_before_calling_cpp_dtor.py:7:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_modules.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_modules.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_modules.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_modules.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_modules.py:10:6: Cannot resolve import `pybind11_tests.modules`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_modules.py:14:12: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_modules.py:69:10: Cannot resolve import `pybind11_tests.modules`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_modules.py:78:12: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_operator_overloading.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_operator_overloading.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_operator_overloading.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_operator_overloading.py:7:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_kwargs_and_defaults.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_kwargs_and_defaults.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_kwargs_and_defaults.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_docstring_options.py:3:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_factory_constructors.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_factory_constructors.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_shared_ptr_copy_move.py:3:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_numpy_dtypes.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_numpy_dtypes.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_numpy_dtypes.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/env.py:7:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_smart_ptr.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_smart_ptr.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_smart_ptr.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_property.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_property.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_property.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_methods_and_attributes.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_methods_and_attributes.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_methods_and_attributes.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_methods_and_attributes.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_methods_and_attributes.py:243:10: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_methods_and_attributes.py:375:10: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:7:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:9:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:10:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:11:6: Cannot resolve import `pybind11_tests`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:182:15: Operator `*` is unsupported between objects of type `Literal[c_char]` and `Literal[10]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:183:14: Operator `*` is unsupported between objects of type `Literal[c_int]` and `Literal[15]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:184:15: Operator `*` is unsupported between objects of type `Literal[c_long]` and `Literal[7]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:197:16: Operator `*` is unsupported between objects of type `Literal[c_char]` and `Literal[10]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:198:15: Operator `*` is unsupported between objects of type `Literal[c_int]` and `Literal[15]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:199:16: Operator `*` is unsupported between objects of type `Literal[c_long]` and `Literal[7]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:217:23: Operator `*` is unsupported between objects of type `Literal[c_char]` and `int`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:220:23: Operator `*` is unsupported between objects of type `Literal[c_char]` and `int`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_custom_type_casters.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_custom_type_casters.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_custom_type_casters.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_exceptions.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_exceptions.py:6:6: Cannot resolve import `custom_exceptions`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_exceptions.py:8:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_exceptions.py:9:8: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_exceptions.py:10:8: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_exceptions.py:11:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_vector_unique_ptr_member.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_vector_unique_ptr_member.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_basic.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_basic.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_warnings.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_warnings.py:7:8: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_warnings.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_python_multiple_inheritance.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class.py:8:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class.py:10:6: Cannot resolve import `pybind11_tests`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_class.py:218:5: Cannot create a consistent method resolution order (MRO) for class `RabbitHamster` with bases list `[Unknown, Unknown]`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class.py:529:10: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_mi_thunks.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_mi_thunks.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_shared_from_this.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_shared_from_this.py:8:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_shared_from_this.py:9:8: Cannot resolve import `pybind11_tests.class_sh_trampoline_shared_from_this`
+ warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_shared_from_this.py:213:20: Name `obj0_wr` used when possibly not defined
+ warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_shared_from_this.py:216:20: Name `obj0_wr` used when possibly not defined
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_inheritance.py:3:6: Cannot resolve import `pybind11_tests`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_inheritance.py:54:5: Cannot create a consistent method resolution order (MRO) for class `Drvd2` with bases list `[Unknown, Unknown]`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:10:6: Cannot resolve import `pybind11_tests.factory_constructors`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:230:10: Cannot resolve import `pybind11_tests.factory_constructors`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:338:5: Cannot create a consistent method resolution order (MRO) for class `MITest` with bases list `[Unknown, Unknown]`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_self_life_support.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_self_life_support.py:5:8: Cannot resolve import `pybind11_tests.class_sh_trampoline_self_life_support`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_unnamed_namespace_a.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_unnamed_namespace_a.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_unnamed_namespace_a.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_unique_ptr_member.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_unique_ptr_member.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eigen_matrix.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eigen_matrix.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eigen_matrix.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_numpy_array.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_numpy_array.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_numpy_array.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_copy_move.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_copy_move.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_copy_move.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_embed/test_interpreter.py:5:6: Cannot resolve import `widget_module`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_constants_and_functions.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_disowning.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_disowning.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_native_enum.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_native_enum.py:8:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_native_enum.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_native_enum.py:114:41: Type `IntEnum` has no attribute `mem`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_cmake_build/test.py:5:8: Cannot resolve import `test_cmake_build`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_docs_advanced_cast_custom.py:6:10: Cannot resolve import `conftest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_docs_advanced_cast_custom.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:7:6: Cannot resolve import `pybind11_tests`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:58:5: Cannot create a consistent method resolution order (MRO) for class `MI1` with bases list `[Unknown, Unknown]`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:67:5: Cannot create a consistent method resolution order (MRO) for class `MI2` with bases list `[<class 'B1'>, Unknown, Unknown]`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:83:5: Cannot create a consistent method resolution order (MRO) for class `MI5` with bases list `[Unknown, <class 'B1'>, Unknown]`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:163:5: Cannot create a consistent method resolution order (MRO) for class `MIMany14` with bases list `[Unknown, Unknown, Unknown, Unknown]`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:170:5: Cannot create a consistent method resolution order (MRO) for class `MIMany58` with bases list `[Unknown, Unknown, Unknown, Unknown]`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_multiple_inheritance.py:177:5: Cannot create a consistent method resolution order (MRO) for class `MIMany916` with bases list `[Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown]`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_type_caster_pyobject_ptr.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_type_caster_pyobject_ptr.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:10:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:28:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:53:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:66:12: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:76:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:110:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:136:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:178:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:186:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_local_bindings.py:208:12: Cannot resolve import `pybind11_cross_module_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_builtin_casters.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_builtin_casters.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_builtin_casters.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_builtin_casters.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_builtin_casters.py:9:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/conftest.py:20:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/conftest.py:24:12: Cannot resolve import `pybind11_tests`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/conftest.py:222:14: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/conftest.py:222:14: Object of type `Literal[suppress]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eigen_tensor.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eigen_tensor.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eigen_tensor.py:13:12: Cannot resolve import `eigen_tensor_avoid_stl_array`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pytypes.py:7:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pytypes.py:9:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pytypes.py:10:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pytypes.py:11:6: Cannot resolve import `pybind11_tests`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/test_pytypes.py:777:14: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/test_pytypes.py:777:14: Object of type `Literal[nullcontext]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_pytypes.py:1175:25: Module `inspect` has no member `get_annotations`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_numpy_vectorize.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_numpy_vectorize.py:5:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_shared_ptr_cpp_arg.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_shared_ptr_cpp_arg.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_class_sh_trampoline_shared_ptr_cpp_arg.py:6:8: Cannot resolve import `pybind11_tests.class_sh_trampoline_shared_ptr_cpp_arg`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tools/make_changelog.py:11:8: Cannot resolve import `ghapi.all`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_call_policies.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_call_policies.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_call_policies.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_call_policies.py:7:6: Cannot resolve import `pybind11_tests`
+ error[lint:inconsistent-mro] /tmp/mypy_primer/projects/pybind11/tests/test_call_policies.py:168:5: Cannot create a consistent method resolution order (MRO) for class `Derived` with bases list `[Unknown, Unknown]`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eval.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eval.py:7:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_eval.py:8:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/extra_setuptools/test_setuphelper.py:8:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl.py:5:8: Cannot resolve import `env`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl.py:6:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl.py:7:6: Cannot resolve import `pybind11_tests`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/tests/test_stl.py:391:12: Cannot resolve import `pybind11_cross_module_tests`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:69:48: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:72:62: Unused blanket `type: ignore` directive
+ error[lint:invalid-base] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:89:25: Invalid class base with type `Literal[Extension, Extension]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-base] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:271:17: Invalid class base with type `Literal[build_ext, build_ext]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:471:31: Name `multiprocessing` used when possibly not defined
+ warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:477:22: Name `ThreadPool` used when possibly not defined
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:492:66: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:500:66: Unused blanket `type: ignore` directive
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pybind11/tests/extra_python_package/test_files.py:158:10: No argument provided for required parameter `namespace` of bound method `__new__`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pybind11/tests/extra_python_package/test_files.py:158:10: Object of type `Literal[closing]` cannot be assigned to parameter 2 (`name`) of bound method `__new__`; expected type `str`
+ warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pybind11/setup.py:47:48: Name `patch` used when possibly not defined
+ warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pybind11/setup.py:47:59: Name `level` used when possibly not defined
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pybind11/setup.py:109:36: Unused blanket `type: ignore` directive
+ Found 287 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline_main.py:52:12: Type `None` has no attribute `time`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline_main.py:76:12: Type `None` has no attribute `processor_options`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline_main.py:100:12: Type `None` has no attribute `processor_options`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/context_manager.py:76:17: Attribute `f_code` on type `FrameType | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/context_manager.py:76:50: Attribute `f_lineno` on type `FrameType | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:51:12: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:52:12: Attribute `await_time` on type `Frame | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:54:47: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:74:16: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:75:16: Attribute `await_time` on type `Frame | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:77:51: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:158:18: Attribute `root_frame` on type `None | @Todo(Support for `typing.TypeVar` instances in type expressions)` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:166:44: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:199:12: Attribute `time` on type `Frame | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:201:44: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:231:12: Attribute `time` on type `Frame | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:233:44: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:238:32: Object of type `Frame | None` cannot be assigned to parameter 1 (`frame`) of function `walk_frames`; expected type `Frame`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:69:5: Attribute `self_check` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:71:12: Attribute `total_self_time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:72:12: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:73:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:74:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:75:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:76:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:77:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:79:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:80:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:81:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:123:5: Attribute `self_check` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:125:12: Attribute `total_self_time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:126:12: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:127:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:128:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:129:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:130:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:131:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:159:5: Attribute `self_check` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:161:12: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:162:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:163:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:164:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:165:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:166:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:167:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:168:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:207:5: Attribute `self_check` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:208:12: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:210:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:211:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:212:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:213:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:214:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:215:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:216:12: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:252:5: Attribute `self_check` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:253:12: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:255:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:256:60: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:288:5: Attribute `self_check` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:289:12: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:290:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:292:28: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:347:5: Attribute `self_check` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:348:12: Attribute `time` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:349:18: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_threading.py:20:48: Attribute `frame_records` on type `Unknown | ActiveProfilerSession | None` is possibly unbound
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_threading.py:44:12: Type `None` has no attribute `args`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_pstats_renderer.py:125:43: Object of type `Session | None` cannot be assigned to parameter 2 (`session`) of bound method `render`; expected type `Session`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/stack_sampler.py:249:53: Object of type `int | float | None` cannot be assigned to parameter 1 (`value`) of function `format_float_with_sig_figs`; expected type `int | float`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:90:12: Attribute `function` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:91:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:112:12: Attribute `function` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:113:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
- Found 215 diagnostics
+ Found 292 diagnostics

black (https://github.com/psf/black)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/driver.py:291:13: Object of type `bytes | None` cannot be assigned to parameter 2 (`pkl`) of bound method `loads`; expected type `bytes`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/pgen.py:187:16: Object of type `tuple[dict, None | Unknown]` is not assignable to return type `tuple[@Todo(generics), str]`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:564:17: Attribute `update` on type `@Todo(Support for `typing.GenericAlias` instances in type expressions) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:203:29: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:210:30: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:267:41: Attribute `parent` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:267:54: Attribute `parent` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:277:8: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:282:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:287:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:304:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:311:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:323:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:336:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:340:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:348:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:355:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:358:20: Attribute `parent` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:359:16: Attribute `parent` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:370:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:375:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:380:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:402:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:414:10: Attribute `type` on type `Node | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:417:10: Attribute `type` on type `Node | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:672:17: Object of type `@Todo(return type of overloaded function) | None` cannot be assigned to parameter `root` of function `get_sources`; expected type `Path`
- Found 232 diagnostics
+ Found 258 diagnostics

rich (https://github.com/Textualize/rich)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/traceback.py:760:34: Object of type `None | range` cannot be assigned to parameter 1 (`obj`) of function `len`; expected type `Sized`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_logging.py:122:25: Type `None` has no attribute `msg`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_pr...*[Comment body truncated]*

---

_@AlexWaygood approved on 2025-04-10 15:03_

---

_Merged by @carljm on 2025-04-10 15:06_

---

_Closed by @carljm on 2025-04-10 15:06_

---

_Branch deleted on 2025-04-10 15:06_

---
