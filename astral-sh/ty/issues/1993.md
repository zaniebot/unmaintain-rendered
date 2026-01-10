```yaml
number: 1993
title: "ty gets stuck while checking `parver` package"
type: issue
state: closed
author: RazerM
labels:
  - bug
  - performance
assignees: []
created_at: 2025-12-17T09:45:14Z
updated_at: 2025-12-17T19:45:06Z
url: https://github.com/astral-sh/ty/issues/1993
synced_at: 2026-01-10T01:53:59Z
```

# ty gets stuck while checking `parver` package

---

_Issue opened by @RazerM on 2025-12-17 09:45_

### Summary

I tried the ty beta on a project and had to kill it at 56 GB memory usage. I narrowed the issue down to a script that uses `parver` to bump the version number.

To reproduce, create the following script as `issue1993.py` and run `uvx --with-requirements issue1993.py ty check issue1993.py`

```python
# /// script
# requires-python = ">=3.14"
# dependencies = [
#     "parver>=0.5",
# ]
# ///
from parver import Version

Version(release=(1,2))
```

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Label `memory` added by @AlexWaygood on 2025-12-17 09:57_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 09:57_

---

_Label `bug` added by @AlexWaygood on 2025-12-17 09:57_

---

_Comment by @sharkdp on 2025-12-17 14:34_

Thank you for reporting this!

Here is a backtrace from the thread that's stuck / iterating

<details><summary>Details</summary>
<p>

```
#0  0x00005555563cc0f1 in ty_python_semantic::types::builder::IntersectionBuilder::build ()
#1  0x000055555677c449 in ty_python_semantic::types::generics::SpecializationBuilder::add_type_mappings_from_constraint_set ()
#2  0x000055555677a556 in ty_python_semantic::types::generics::SpecializationBuilder::infer_map_impl ()
#3  0x0000555556778d91 in ty_python_semantic::types::generics::SpecializationBuilder::infer_map_impl ()
#4  0x00005555563a95ef in ty_python_semantic::types::call::bind::Binding::check_types ()
#5  0x00005555563a58b0 in ty_python_semantic::types::call::bind::Bindings::check_types_impl ()
#6  0x0000555556684f41 in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_call_expression_impl ()
#7  0x000055555665b24b in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_call_expression ()
#8  0x000055555664b083 in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_expression_impl ()
#9  0x00005555566ab85b in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_definition ()
#10 0x00005555566f958f in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::finish_definition ()
#11 0x0000555556160537 in salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute ()
#12 0x0000555556480fb1 in std::thread::local::LocalKey<T>::with ()
#13 0x00005555567215eb in core::iter::traits::iterator::Iterator::find_map::check::{{closure}} ()
#14 0x0000555556720d38 in ty_python_semantic::place::place_from_bindings_impl ()
#15 0x0000555556662148 in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_place_load ()
#16 0x000055555665e63a in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_name_load ()
#17 0x000055555664b3be in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_expression_impl ()
#18 0x000055555668ac29 in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_all_argument_types ()
#19 0x0000555556689e8c in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_and_check_argument_types::{{closure}} ()
#20 0x0000555556684e7c in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_call_expression_impl ()
#21 0x000055555665b24b in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_call_expression ()
#22 0x000055555664b083 in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_expression_impl ()
#23 0x00005555566ab85b in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_definition ()
#24 0x00005555566f958f in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::finish_definition ()
#25 0x0000555556160537 in salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute ()
#26 0x0000555556480fb1 in std::thread::local::LocalKey<T>::with ()
#27 0x00005555567222a8 in <core::iter::adapters::peekable::Peekable<I> as core::iter::traits::iterator::Iterator>::try_fold ()
#28 0x000055555671f604 in ty_python_semantic::place::place_from_declarations_impl ()
#29 0x000055555672ebe9 in ty_python_semantic::types::class::ClassLiteral::own_fields ()
#30 0x00005555561bd5e8 in salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute ()
#31 0x0000555556485ce3 in std::thread::local::LocalKey<T>::with ()
#32 0x000055555675b5d5 in ty_python_semantic::types::class::ClassLiteral::own_synthesized_member::{{closure}} ()
#33 0x000055555675a357 in ty_python_semantic::types::class::ClassLiteral::own_synthesized_member ()
#34 0x0000555556757cb0 in ty_python_semantic::types::class::ClassLiteral::own_class_member ()
#35 0x00005555567552f7 in ty_python_semantic::types::class::ClassType::own_class_member ()
#36 0x0000555556752847 in ty_python_semantic::types::class::ClassLiteral::class_member_inner ()
#37 0x0000555556752689 in ty_python_semantic::types::class::ClassLiteral::class_member ()
#38 0x000055555658884e in ty_python_semantic::types::Type::find_name_in_mro_with_policy ()
#39 0x0000555556588ec7 in ty_python_semantic::types::Type::find_name_in_mro_with_policy ()
#40 0x0000555556146ecb in salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute ()
#41 0x000055555649aa02 in std::thread::local::LocalKey<T>::with ()
#42 0x000055555658da10 in ty_python_semantic::types::Type::invoke_descriptor_protocol ()
#43 0x000055555658b857 in <ty_python_semantic::types::Type as ty_python_semantic::types::Type::member_lookup_with_policy::InnerTrait_>::member_lookup_with_policy_ ()
#44 0x00005555561db4b2 in salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute ()
#45 0x00005555564a0d62 in std::thread::local::LocalKey<T>::with ()
#46 0x00005555565c924f in ty_python_semantic::types::Type::try_call_constructor ()
#47 0x00005555566891cd in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_call_expression_impl ()
#48 0x000055555665b24b in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_call_expression ()
#49 0x000055555664b083 in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_expression_impl ()
#50 0x00005555566c9f76 in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_body ()
#51 0x00005555566c1bf5 in ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_scope ()
#52 0x0000555556242b59 in salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute ()
#53 0x00005555565335ae in ty_python_semantic::types::infer::infer_scope_types ()
#54 0x000055555659a8a7 in ty_python_semantic::types::check_types ()
#55 0x000055555611549a in <ty_project::check_file_impl::check_file_impl_Configuration_ as salsa::function::Configuration>::execute ()
#56 0x000055555608863f in salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute ()
#57 0x0000555556104bdf in std::thread::local::LocalKey<T>::with ()
#58 0x00005555561142e7 in <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute ()
#59 0x0000555555d0a26a in rayon_core::registry::WorkerThread::wait_until_cold ()
#60 0x00005555561138f4 in rayon_core::scope::scope::{{closure}} ()
#61 0x00005555560b88cd in ty_project::db::ProjectDatabase::check_with_reporter ()
#62 0x00005555560012ed in <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute ()
#63 0x0000555555d0ad03 in rayon_core::registry::WorkerThread::wait_until_cold ()
#64 0x0000555555d10765 in std::sys::backtrace::__rust_begin_short_backtrace ()
#65 0x0000555555d0ecb1 in core::ops::function::FnOnce::call_once{{vtable.shim}} ()
#66 0x0000555555f4529f in std::sys::thread::unix::Thread::new::thread_start ()
#67 0x00007ffff7c9698b in start_thread (arg=<optimized out>) at pthread_create.c:448
#68 0x00007ffff7d1a9cc in __GI___clone3 () at ../sysdeps/unix/sysv/linux/x86_64/clone3.S:78
```

</p>
</details> 

---

_Label `memory` removed by @sharkdp on 2025-12-17 14:35_

---

_Label `performance` added by @sharkdp on 2025-12-17 14:35_

---

_Comment by @sharkdp on 2025-12-17 14:58_

Saving an intermediate state while trying to minimize this (now only dependent on `attrs`):

```py
from typing_extensions import Literal, TypeAlias
from attr.validators import  in_, optional

PreTag: TypeAlias = Literal["c", "rc", "alpha", "a", "beta", "b", "preview", "pre"]

def _():
    PRE_TAGS: set[PreTag] = {"c", "rc", "alpha", "a", "beta", "b", "preview", "pre"}

    validate_pre_tag = optional(in_(PRE_TAGS))
```

`optional` and `in_` are defined [here](https://github.com/python-attrs/attrs/blob/460f019d105c34cbac8e737ce2eb8466448f0506/src/attr/validators.pyi#L53-L60) and seem to involve unions of generic `Callable` types. 

FYI @dcreager 


---

_Comment by @dcreager on 2025-12-17 19:43_

This was the same problem as https://github.com/astral-sh/ty/issues/1968, and is fixed by https://github.com/astral-sh/ruff/pull/22030

---

_Comment by @dcreager on 2025-12-17 19:45_

(Confirmed locally with both @RazerM's example and @sharkdp's reduction)

---

_Closed by @dcreager on 2025-12-17 19:45_

---
