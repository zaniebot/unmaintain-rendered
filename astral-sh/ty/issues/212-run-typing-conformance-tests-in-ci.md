```yaml
number: 212
title: Run typing conformance tests in CI
type: issue
state: closed
author: karthiknadig
labels:
  - ci
assignees: []
created_at: 2025-01-24T04:22:15Z
updated_at: 2025-07-28T06:03:24Z
url: https://github.com/astral-sh/ty/issues/212
synced_at: 2026-01-12T15:54:22Z
```

# Run typing conformance tests in CI

---

_@karthiknadig_

### Description

using build from main https://github.com/astral-sh/ruff/commit/2b3550c85fcc55d93216c4d4ef64c5f0a5766053

You probably already know this but just incase:
```
(.venv) C:\GIT\misc\typing>C:\GIT\misc\ruff\target\release\red_knot.exe --venv-path C:\GIT\misc\typing\.venv --project .\conformance\tests --verbose
INFO Python version: Python 3.9, platform: all
INFO Found 141 files in project `tests`
thread '<unnamed>' panicked at C:\Users\kanadig\.cargo\git\checkouts\salsa-61760caba2b17ca5\88a1d77\src\runtime.rs:335:13:
Box<dyn Any>
stack backtrace:
   0:     0x7ff73e99218a - std::backtrace_rs::backtrace::dbghelp64::trace
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\..\..\backtrace\src\backtrace\dbghelp64.rs:91
   1:     0x7ff73e99218a - std::backtrace_rs::backtrace::trace_unsynchronized
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\..\..\backtrace\src\backtrace\mod.rs:66
   2:     0x7ff73e99218a - std::sys::backtrace::_print_fmt
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\sys\backtrace.rs:66
   3:     0x7ff73e99218a - std::sys::backtrace::impl$0::print::impl$0::fmt
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\sys\backtrace.rs:39
   4:     0x7ff73e33fe9a - core::fmt::rt::Argument::fmt
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/core\src\fmt\rt.rs:177
   5:     0x7ff73e33fe9a - core::fmt::write
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/core\src\fmt\mod.rs:1189
   6:     0x7ff73e98d1f8 - std::io::Write::write_fmt<std::sys::pal::windows::stdio::Stderr>
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\io\mod.rs:1884
   7:     0x7ff73e991fb5 - std::sys::backtrace::BacktraceLock::print
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\sys\backtrace.rs:42
   8:     0x7ff73e993e81 - std::panicking::default_hook::closure$1
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\panicking.rs:268
   9:     0x7ff73e993c72 - std::panicking::default_hook
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\panicking.rs:295
  10:     0x7ff73e994533 - std::panicking::rust_panic_with_hook
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\panicking.rs:801
  11:     0x7ff73e974551 - std::sys::backtrace::__rust_end_short_backtrace::hb43df133bd3cc9db
  12:     0x7ff73e974519 - std::sys::backtrace::__rust_end_short_backtrace::hb43df133bd3cc9db
  13:     0x7ff73ea691db - std::panicking::begin_panic::h01ca2cdf480fd399
  14:     0x7ff73e975b66 - salsa::table::sync::SyncTable::claim::h0dc1ae628aade642
  15:     0x7ff73e4d898b - core::option::Option<T>::or_else::h6959188434ceeabe
  16:     0x7ff73e4f5e50 - salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch::hb9a68b9799a78f15
  17:     0x7ff73e5a8931 - red_knot_python_semantic::types::definition_expression_type::h63f12074fc732089
  18:     0x7ff73e5be681 - <red_knot_python_semantic::types::TypeAliasType::value_type::inner_fn_name_::Configuration_ as salsa::function::Configuration>::execute::h0859ae33fa79eb8d
  19:     0x7ff73e5c8e32 - salsa::cycle::Cycle::catch::h23b735ea3051191d
  20:     0x7ff73e51fdc8 - salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_hot::hfe49e8c51b0b4ead
  21:     0x7ff73e4dae5e - core::option::Option<T>::or_else::ha8b1686a9084550d
  22:     0x7ff73e4f6190 - salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch::hc6d0565888ad7ef3
  23:     0x7ff73e5be311 - red_knot_python_semantic::types::TypeAliasType::value_type::h2fe8c72b3c5b9a81
  24:     0x7ff73e5af73e - red_knot_python_semantic::types::Type::in_type_expression::hb5ed3c76e6787691
  25:     0x7ff73e57ba15 - red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_binary_type_comparison::h98d60383c7c71277
  26:     0x7ff73e578e18 - red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_binary_type_comparison::h98d60383c7c71277
  27:     0x7ff73e580669 - <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute::heb7b8f704ae20a77
  28:     0x7ff73e5ca47b - salsa::cycle::Cycle::catch::hc9fdc5d1b27d804c
  29:     0x7ff73e523c1e - salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_hot::hfe49e8c51b0b4ead
  30:     0x7ff73e4d8bdc - core::option::Option<T>::or_else::h6959188434ceeabe
  31:     0x7ff73e4f5e50 - salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch::hb9a68b9799a78f15
  32:     0x7ff73e5b5e76 - <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::h7afcbd17d4eafbb3
  33:     0x7ff73e5c8f78 - salsa::cycle::Cycle::catch::h38f6bfb6a6f680d4
  34:     0x7ff73e4ff0cc - salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_hot::hfe49e8c51b0b4ead
  35:     0x7ff73e4d805c - core::option::Option<T>::or_else::h53cadaa22641676b
  36:     0x7ff73e4f4ac0 - salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch::h48159ec57ae7ed8f
  37:     0x7ff73e5b543a - red_knot_python_semantic::types::check_types::hfe82806a2749bdd1
  38:     0x7ff73e473fae - red_knot_project::check_file::h53bb0b922ceb10eb
  39:     0x7ff73e4a1afc - rayon_core::scope::ScopeBase::execute_job_closure::h9edff98ddad2ccf0
  40:     0x7ff73e4aa3c9 - <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute::h492d7eb417d59a3c
  41:     0x7ff73ea41d00 - rayon_core::registry::WorkerThread::wait_until_cold::hf60be8f5244da166
  42:     0x7ff73e407db1 - rayon_core::registry::ThreadBuilder::run::h1a46a9ee282143a6
  43:     0x7ff73e40e932 - std::sys::backtrace::__rust_begin_short_backtrace::hb11de3c775aa8746
  44:     0x7ff73e40b50c - std::thread::Builder::spawn_unchecked::hab5de077d341b550
  45:     0x7ff73e99810d - alloc::boxed::impl$28::call_once
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/alloc\src\boxed.rs:1972
  46:     0x7ff73e99810d - alloc::boxed::impl$28::call_once
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/alloc\src\boxed.rs:1972
  47:     0x7ff73e99810d - std::sys::pal::windows::thread::impl$0::new::thread_start
                               at /rustc/9fc6b43126469e3858e2fe86cafb4f0fd5068869\library/std\src\sys\pal\windows\thread.rs:55
  48:     0x7ff9cf96e8d7 - BaseThreadInitThunk
  49:     0x7ff9d0c5fbcc - RtlUserThreadStart
Rayon: detected unexpected panic; aborting
```

---

_Comment by @dhruvmanila on 2025-01-24 04:30_

Thanks. Yeah, I don't think we're ready to run the conformance tests as red knot is missing a lot of features. But, this specific issue is most likely about fixpoint iteration which @carljm is working on currently (https://github.com/astral-sh/ty/issues/232).

---

_Label `bug` added by @AlexWaygood on 2025-01-24 08:57_

---

_Comment by @carljm on 2025-01-24 16:48_

It certainly looks like a cycle anyway, for which the right fix might be fixpoint iteration, or might be fixing a case where we're trying to resolve something too eagerly (or in a too coarse-grained way).

---

_Renamed from "red_knot crashes on the typing conformance tests" to "[red-knot] crashes on the typing conformance tests" by @carljm on 2025-03-27 19:00_

---

_Renamed from "[red-knot] crashes on the typing conformance tests" to "crashes on the typing conformance tests" by @MichaReiser on 2025-05-07 15:27_

---

_Added to milestone `Beta` by @carljm on 2025-05-07 18:39_

---

_Label `fatal` added by @AlexWaygood on 2025-05-10 21:40_

---

_Renamed from "crashes on the typing conformance tests" to "run vs typing conformance tests" by @carljm on 2025-05-28 23:47_

---

_Renamed from "run vs typing conformance tests" to "run typing conformance tests in CI" by @carljm on 2025-05-28 23:47_

---

_Renamed from "run typing conformance tests in CI" to "Run typing conformance tests in CI" by @dhruvmanila on 2025-07-18 14:14_

---

_Label `ci` added by @dhruvmanila on 2025-07-18 14:14_

---

_Label `bug` removed by @dhruvmanila on 2025-07-18 14:15_

---

_Label `fatal` removed by @dhruvmanila on 2025-07-18 14:15_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-07-19 03:34_

---

_Closed by @dhruvmanila on 2025-07-28 06:03_

---
