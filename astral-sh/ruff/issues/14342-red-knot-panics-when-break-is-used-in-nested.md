```yaml
number: 14342
title: "[red-knot] Panics when 'break' is used in nested definitions inside loops"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - ty
assignees: []
created_at: 2024-11-14T17:27:34Z
updated_at: 2024-11-22T12:13:56Z
url: https://github.com/astral-sh/ruff/issues/14342
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] Panics when 'break' is used in nested definitions inside loops

---

_Issue opened by @qarmin on 2024-11-14 17:27_

```
red_knot --current-directory .
```

<details>

```
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/semantic_index/use_def.rs:555:9:
assertion failed: self.symbol_states.len() >= snapshot.symbol_states.len()
stack backtrace:
   0: rust_begin_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:662:5
   1: core::panicking::panic_fmt
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panicking.rs:74:14
   2: core::panicking::panic
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panicking.rs:148:5
   3: red_knot_python_semantic::semantic_index::use_def::UseDefMapBuilder::merge
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/use_def.rs:555:9
   4: red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder::flow_merge
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:179:9
   5: <red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder as ruff_python_ast::visitor::Visitor>::visit_stmt
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:781:21
   6: ruff_python_ast::visitor::walk_body
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_ast/src/visitor.rs:115:9
   7: ruff_python_ast::visitor::Visitor::visit_body
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_ast/src/visitor.rs:94:9
   8: <red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder as ruff_python_ast::visitor::Visitor>::visit_stmt::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:550:33
   9: red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder::with_type_params
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:384:28
  10: <red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder as ruff_python_ast::visitor::Visitor>::visit_stmt
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:534:17
  11: ruff_python_ast::visitor::walk_body
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_ast/src/visitor.rs:115:9
  12: ruff_python_ast::visitor::Visitor::visit_body
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_ast/src/visitor.rs:94:9
  13: <red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder as ruff_python_ast::visitor::Visitor>::visit_stmt::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:586:33
  14: red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder::with_type_params
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:384:28
  15: <red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder as ruff_python_ast::visitor::Visitor>::visit_stmt
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:577:17
  16: ruff_python_ast::visitor::walk_body
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_ast/src/visitor.rs:115:9
  17: ruff_python_ast::visitor::Visitor::visit_body
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_ast/src/visitor.rs:94:9
  18: red_knot_python_semantic::semantic_index::builder::SemanticIndexBuilder::build
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index/builder.rs:472:14
  19: <red_knot_python_semantic::semantic_index::semantic_index::Configuration_ as salsa::function::Configuration>::execute::inner_
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/semantic_index.rs:45:5
  20: <red_knot_python_semantic::semantic_index::semantic_index::Configuration_ as salsa::function::Configuration>::execute
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:179:21
  21: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:43
  22: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panic/unwind_safe.rs:272:9
  23: std::panicking::try::do_call
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:554:40
  24: std::panicking::try
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:518:19
  25: std::panic::catch_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:345:14
  26: salsa::cycle::Cycle::catch
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/cycle.rs:42:15
  27: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:27
  28: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:86:14
  29: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:67
  30: core::option::Option<T>::or_else
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/option.rs:1545:21
  31: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:33
  32: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:13:25
  33: red_knot_python_semantic::semantic_index::semantic_index::{{closure}}
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:278:25
  34: salsa::attach::Attached::attach
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:71:9
  35: salsa::attach::attach::{{closure}}
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:23
  36: std::thread::local::LocalKey<T>::try_with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:283:12
  37: std::thread::local::LocalKey<T>::with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:260:9
  38: salsa::attach::attach
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:14
  39: red_knot_python_semantic::semantic_index::semantic_index
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:270:13
  40: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_python_semantic/src/types.rs:46:17
  41: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:179:21
  42: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:43
  43: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panic/unwind_safe.rs:272:9
  44: std::panicking::try::do_call
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:554:40
  45: std::panicking::try
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:518:19
  46: std::panic::catch_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:345:14
  47: salsa::cycle::Cycle::catch
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/cycle.rs:42:15
  48: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/execute.rs:51:27
  49: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:86:14
  50: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:67
  51: core::option::Option<T>::or_else
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/option.rs:1545:21
  52: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:36:33
  53: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/function/fetch.rs:13:25
  54: red_knot_python_semantic::types::check_types::{{closure}}
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:278:25
  55: salsa::attach::Attached::attach
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:71:9
  56: salsa::attach::attach::{{closure}}
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:23
  57: std::thread::local::LocalKey<T>::try_with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:283:12
  58: std::thread::local::LocalKey<T>::with
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/thread/local.rs:260:9
  59: salsa::attach::attach
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/attach.rs:91:14
  60: red_knot_python_semantic::types::check_types
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/components/salsa-macro-rules/src/setup_tracked_fn.rs:270:13
  61: red_knot_workspace::workspace::check_file
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_workspace/src/workspace.rs:400:24
  62: red_knot_workspace::workspace::Workspace::check::{{closure}}::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_workspace/src/workspace.rs:213:44
  63: rayon_core::scope::Scope::spawn::{{closure}}::{{closure}}
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:526:57
  64: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panic/unwind_safe.rs:272:9
  65: std::panicking::try::do_call
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:554:40
  66: std::panicking::try
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:518:19
  67: std::panic::catch_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:345:14
  68: rayon_core::unwind::halt_unwinding
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/unwind.rs:17:5
  69: rayon_core::scope::ScopeBase::execute_job_closure
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:689:28
  70: rayon_core::scope::ScopeBase::execute_job
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:679:29
  71: rayon_core::scope::Scope::spawn::{{closure}}
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:526:13
  72: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/job.rs:169:9
  73: rayon_core::job::JobRef::execute
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/job.rs:64:9
  74: rayon_core::registry::WorkerThread::execute
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:860:13
  75: rayon_core::registry::WorkerThread::wait_until_cold
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:786:22
  76: rayon_core::scope::ScopeBase::complete
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:668:9
  77: rayon_core::scope::scope::{{closure}}
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:291:9
  78: rayon_core::registry::in_worker
  79: rayon_core::scope::scope
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:289:5
  80: red_knot_workspace::workspace::Workspace::check
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_workspace/src/workspace.rs:203:9
  81: red_knot_workspace::db::RootDatabase::check::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_workspace/src/db.rs:55:27
  82: red_knot_workspace::db::RootDatabase::with_db::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_workspace/src/db.rs:79:29
  83: std::panicking::try::do_call
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:554:40
  84: std::panicking::try
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:518:19
  85: std::panic::catch_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:345:14
  86: salsa::cancelled::Cancelled::catch
             at /home/runner/.cargo/git/checkouts/salsa-61760caba2b17ca5/254c749/src/cancelled.rs:36:15
  87: red_knot_workspace::db::RootDatabase::with_db
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_workspace/src/db.rs:79:9
  88: red_knot_workspace::db::RootDatabase::check
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot_workspace/src/db.rs:55:14
  89: red_knot::MainLoop::main_loop::{{closure}}
             at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/red_knot/src/main.rs:306:45
  90: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/core/src/panic/unwind_safe.rs:272:9
  91: std::panicking::try::do_call
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:554:40
  92: std::panicking::try
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panicking.rs:518:19
  93: std::panic::catch_unwind
             at /rustc/f6e511eec7342f59a25f7c0534f1dbea00d01b14/library/std/src/panic.rs:345:14
  94: rayon_core::unwind::halt_unwinding
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/unwind.rs:17:5
  95: rayon_core::registry::Registry::catch_unwind
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:367:27
  96: rayon_core::spawn::spawn_job::{{closure}}
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/spawn/mod.rs:97:13
  97: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/job.rs:169:9
  98: rayon_core::job::JobRef::execute
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/job.rs:64:9
  99: rayon_core::registry::WorkerThread::execute
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:860:13
 100: rayon_core::registry::WorkerThread::wait_until_cold
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:794:26
 101: rayon_core::registry::WorkerThread::wait_until
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:769:13
 102: rayon_core::registry::WorkerThread::wait_until_out_of_work
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:818:9
 103: rayon_core::registry::main_loop
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:923:5
 104: rayon_core::registry::ThreadBuilder::run
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:53:18
 105: <rayon_core::registry::DefaultSpawn as rayon_core::registry::ThreadSpawn>::spawn::{{closure}}
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:98:20
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
Rayon: detected unexpected panic; aborting
Aborted (core dumped)
```

</details>

[other_11922910986809159176.py.zip](https://github.com/user-attachments/files/17753992/other_11922910986809159176.py.zip)



---

_Label `bug` added by @MichaReiser on 2024-11-14 17:35_

---

_Label `red-knot` added by @MichaReiser on 2024-11-14 17:35_

---

_Comment by @MichaReiser on 2024-11-14 17:35_

Oh no, more distractions for @sharkdp :laughing: 

---

_Renamed from "Red knot panics when checking invalid file" to "[red-knot] Panics when 'break' is used in nested definitions inside loops" by @sharkdp on 2024-11-22 09:44_

---

_Assigned to @sharkdp by @sharkdp on 2024-11-22 09:44_

---

_Comment by @sharkdp on 2024-11-22 09:47_

Ok, this took a while to distill into a minimal example. It happens for invalid code like the following (so this is also part of #13778):
```py
while True:

    def b():
        x: int

        break
```

The problem is that the semantic index builder currently doesn't handle the case of invalid `break` statements. It treats the `break` inside the function definition as being part of the `while` loops.

---

_Closed by @sharkdp on 2024-11-22 12:13_

---

_Closed by @sharkdp on 2024-11-22 12:13_

---
