```yaml
number: 13012
title: Red knot panics checking some named expressions
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2024-08-20T15:17:13Z
updated_at: 2024-08-22T14:59:14Z
url: https://github.com/astral-sh/ruff/issues/13012
synced_at: 2026-01-10T11:09:55Z
```

# Red knot panics checking some named expressions

---

_Issue opened by @MichaReiser on 2024-08-20 15:17_

```python
match x:
    case [1, 0] if x := x[:0]:
        y = 1
```

```
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/infer.rs:143:25:
no entry found for key
stack backtrace:
   0: rust_begin_unwind
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:652:5
   1: core::panicking::panic_fmt
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/panicking.rs:72:14
   2: core::panicking::panic_display
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/panicking.rs:262:5
   3: core::option::expect_failed
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/option.rs:1995:5
   4: core::option::Option<T>::expect
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/option.rs:898:21
   5: <std::collections::hash::map::HashMap<K,V,S> as core::ops::index::Index<&Q>>::index
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/collections/hash/map.rs:1343:23
   6: red_knot_python_semantic::types::infer::TypeInference::expression_ty
             at ./crates/red_knot_python_semantic/src/types/infer.rs:143:25
   7: <ruff_python_ast::expression::ExpressionRef as red_knot_python_semantic::semantic_model::HasTy>::ty
             at ./crates/red_knot_python_semantic/src/semantic_model.rs:62:9
   8: <ruff_python_ast::nodes::ExprName as red_knot_python_semantic::semantic_model::HasTy>::ty
             at ./crates/red_knot_python_semantic/src/semantic_model.rs:72:17
   9: red_knot_workspace::lint::lint_maybe_undefined
             at ./crates/red_knot_workspace/src/lint.rs:130:11
  10: <red_knot_workspace::lint::SemanticVisitor as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./crates/red_knot_workspace/src/lint.rs:257:17
  11: ruff_python_ast::visitor::walk_expr
             at ./crates/ruff_python_ast/src/visitor.rs:511:13
  12: <red_knot_workspace::lint::SemanticVisitor as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./crates/red_knot_workspace/src/lint.rs:262:9
  13: ruff_python_ast::visitor::walk_expr
             at ./crates/ruff_python_ast/src/visitor.rs:351:13
  14: <red_knot_workspace::lint::SemanticVisitor as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./crates/red_knot_workspace/src/lint.rs:262:9
  15: ruff_python_ast::visitor::walk_match_case
             at ./crates/ruff_python_ast/src/visitor.rs:680:9
  16: ruff_python_ast::visitor::Visitor::visit_match_case
             at ./crates/ruff_python_ast/src/visitor.rs:82:9
  17: ruff_python_ast::visitor::walk_stmt
             at ./crates/ruff_python_ast/src/visitor.rs:269:17
  18: <red_knot_workspace::lint::SemanticVisitor as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./crates/red_knot_workspace/src/lint.rs:251:9
  19: ruff_python_ast::visitor::walk_body
             at ./crates/ruff_python_ast/src/visitor.rs:115:9
  20: ruff_python_ast::visitor::Visitor::visit_body
             at ./crates/ruff_python_ast/src/visitor.rs:94:9
  21: <red_knot_workspace::lint::lint_semantic::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_workspace/src/lint.rs:106:5
  22: <red_knot_workspace::lint::lint_semantic::Configuration_ as salsa::function::Configuration>::execute
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/components/salsa-macro-rules/src/setup_tracked_fn.rs:180:21
  23: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/execute.rs:46:43
  24: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/panic/unwind_safe.rs:272:9
  25: std::panicking::try::do_call
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:559:40
  26: __rust_try
  27: std::panicking::try
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:523:19
  28: std::panic::catch_unwind
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panic.rs:149:14
  29: salsa::cycle::Cycle::catch
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/cycle.rs:42:15
  30: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/execute.rs:46:27
  31: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/fetch.rs:104:14
  32: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::compute_value::{{closure}}
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/fetch.rs:46:29
  33: core::option::Option<T>::or_else
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/option.rs:1515:21
  34: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::compute_value
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/fetch.rs:44:34
  35: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/fetch.rs:21:13
  36: red_knot_workspace::lint::lint_semantic::{{closure}}
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/components/salsa-macro-rules/src/setup_tracked_fn.rs:277:25
  37: salsa::attach::Attached::attach
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/attach.rs:71:9
  38: salsa::attach::attach::{{closure}}
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/attach.rs:91:23
  39: std::thread::local::LocalKey<T>::try_with
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/thread/local.rs:283:12
  40: std::thread::local::LocalKey<T>::with
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/thread/local.rs:260:9
  41: salsa::attach::attach
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/attach.rs:91:5
  42: red_knot_workspace::lint::lint_semantic
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/components/salsa-macro-rules/src/setup_tracked_fn.rs:269:13
  43: <red_knot_workspace::workspace::check_file::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_workspace/src/workspace.rs:407:35
  44: <red_knot_workspace::workspace::check_file::Configuration_ as salsa::function::Configuration>::execute
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/components/salsa-macro-rules/src/setup_tracked_fn.rs:180:21
  45: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/execute.rs:46:43
  46: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/panic/unwind_safe.rs:272:9
  47: std::panicking::try::do_call
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:559:40
  48: __rust_try
  49: std::panicking::try
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:523:19
  50: std::panic::catch_unwind
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panic.rs:149:14
  51: salsa::cycle::Cycle::catch
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/cycle.rs:42:15
  52: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/execute.rs:46:27
  53: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/fetch.rs:104:14
  54: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::compute_value::{{closure}}
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/fetch.rs:46:29
  55: core::option::Option<T>::or_else
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/option.rs:1515:21
  56: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::compute_value
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/fetch.rs:44:34
  57: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/function/fetch.rs:21:13
  58: red_knot_workspace::workspace::check_file::{{closure}}
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/components/salsa-macro-rules/src/setup_tracked_fn.rs:277:25
  59: salsa::attach::Attached::attach
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/attach.rs:71:9
  60: salsa::attach::attach::{{closure}}
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/attach.rs:91:23
  61: std::thread::local::LocalKey<T>::try_with
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/thread/local.rs:283:12
  62: std::thread::local::LocalKey<T>::with
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/thread/local.rs:260:9
  63: salsa::attach::attach
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/attach.rs:91:5
  64: red_knot_workspace::workspace::check_file
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/components/salsa-macro-rules/src/setup_tracked_fn.rs:269:13
  65: red_knot_workspace::workspace::Package::check
             at ./crates/red_knot_workspace/src/workspace.rs:315:31
  66: red_knot_workspace::workspace::Workspace::check
             at ./crates/red_knot_workspace/src/workspace.rs:191:31
  67: red_knot_workspace::db::RootDatabase::check::{{closure}}
             at ./crates/red_knot_workspace/src/db.rs:60:27
  68: red_knot_workspace::db::RootDatabase::with_db::{{closure}}
             at ./crates/red_knot_workspace/src/db.rs:82:29
  69: std::panicking::try::do_call
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:559:40
  70: __rust_try
  71: std::panicking::try
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:523:19
  72: std::panic::catch_unwind
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panic.rs:149:14
  73: salsa::cancelled::Cancelled::catch
             at /home/micha/.cargo/git/checkouts/salsa-fa0738f776139e4c/ece083e/src/cancelled.rs:36:15
  74: red_knot_workspace::db::RootDatabase::with_db
             at ./crates/red_knot_workspace/src/db.rs:82:9
  75: red_knot_workspace::db::RootDatabase::check
             at ./crates/red_knot_workspace/src/db.rs:60:9
  76: red_knot::MainLoop::main_loop::{{closure}}
             at ./crates/red_knot/src/main.rs:296:45
  77: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/core/src/panic/unwind_safe.rs:272:9
  78: std::panicking::try::do_call
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:559:40
  79: __rust_try
  80: std::panicking::try
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panicking.rs:523:19
  81: std::panic::catch_unwind
             at /rustc/051478957371ee0084a7c0913941d2a8c4757bb9/library/std/src/panic.rs:149:14
  82: rayon_core::unwind::halt_unwinding
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/unwind.rs:17:5
  83: rayon_core::registry::Registry::catch_unwind
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:367:27
  84: rayon_core::spawn::spawn_job::{{closure}}
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/spawn/mod.rs:97:13
  85: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/job.rs:169:9
  86: rayon_core::job::JobRef::execute
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/job.rs:64:9
  87: rayon_core::registry::WorkerThread::execute
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:860:9
  88: rayon_core::registry::WorkerThread::wait_until_cold
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:794:21
  89: rayon_core::registry::WorkerThread::wait_until
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:769:13
  90: rayon_core::registry::WorkerThread::wait_until_out_of_work
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:818:9
  91: rayon_core::registry::main_loop
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:923:5
  92: rayon_core::registry::ThreadBuilder::run
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:53:18
  93: <rayon_core::registry::DefaultSpawn as rayon_core::registry::ThreadSpawn>::spawn::{{closure}}
             at /home/micha/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:98:20
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
Rayon: detected unexpected panic; aborting
fish: Job 1, 'RUST_BACKTRACE=1 cargo run --bi…' terminated by signal SIGABRT (Abort)
```

---

_Label `bug` added by @MichaReiser on 2024-08-20 15:17_

---

_Label `help wanted` added by @MichaReiser on 2024-08-20 15:17_

---

_Label `red-knot` added by @MichaReiser on 2024-08-20 15:17_

---

_Comment by @dylwil3 on 2024-08-21 19:09_

I think the issue is the walrus operator, not the match guard. Adding just this to the test corpus causes a panic (from a missing key in the `expressions` hashmap for the `TypeInference` struct):

```python
x = 0
if x := x+1:
    pass
```

If it's helpful it seems like the missing scoped expression id is exactly one larger than the the largest scoped expression ids appearing as keys.


---

_Comment by @MichaReiser on 2024-08-21 19:16_

Oh interesting. I suspect we miss to visit the name or value. Are interested in taking this one? 

---

_Comment by @dylwil3 on 2024-08-21 19:16_

Maybe this is the culprit? Should something be added to `expressions` as well as `definitions`?

https://github.com/astral-sh/ruff/blob/dce87c21fdf73a58f3821cae5e71b9da234e29ce/crates/red_knot_python_semantic/src/types/infer.rs#L1448-L1465

---

_Comment by @dylwil3 on 2024-08-21 19:18_

Sure I can try but I imagine I'll be slower to fix it than you - red knot feels more like the [Gordian knot](https://en.wikipedia.org/wiki/Gordian_Knot) to me at the moment 😄 

---

_Comment by @MichaReiser on 2024-08-21 19:20_

infer_expression should add an entry into expressions. Not quite sure what's off

---

_Comment by @MichaReiser on 2024-08-21 19:25_

I don't think it matters but the semantic index builder visits target and value in the opposite order 

https://github.com/astral-sh/ruff/blob/dce87c21fdf73a58f3821cae5e71b9da234e29ce/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L640



---

_Comment by @MichaReiser on 2024-08-21 19:26_

> Sure I can try but I imagine I'll be slower to fix it than you - red knot feels more like the [Gordian knot](https://en.wikipedia.org/wiki/Gordian_Knot) to me at the moment 😄 

Haha 😆. Yeah debugging missing expressions is a bit of a pain. Don't feel pressured into doing it but if you're interested go for it. We're happy to help

---

_Comment by @carljm on 2024-08-22 01:49_

It has something to do with the self-referential named expression; `x := 1` doesn't trigger it, but `x := x + 1` (or Micha's original `x := x[0]`) does.

---

_Renamed from "Red knot panics checking a match case with an `if` guard" to "Red knot panics checking some named expressions" by @carljm on 2024-08-22 01:51_

---

_Comment by @dylwil3 on 2024-08-22 02:42_

As far as I can tell, while trying to infer types for the right hand side of `x := x+1`, type inference enters this function (specifically, while accounting for the `x` on the right hand side):

https://github.com/astral-sh/ruff/blob/8144a11f98032a1f109f557fbd5c8379364230b6/crates/red_knot_python_semantic/src/types/infer.rs#L83-L98

and then some failure must occur because it winds up in recovery:

https://github.com/astral-sh/ruff/blob/8144a11f98032a1f109f557fbd5c8379364230b6/crates/red_knot_python_semantic/src/types/infer.rs#L68-L78

which is presumably why the key never gets added?

But to go further than that I think I'll have to take a detour and try to learn about salsa...



---

_Comment by @dhruvmanila on 2024-08-22 03:31_

> But to go further than that I think I'll have to take a detour and try to learn about salsa...

I think it might be also useful to look at the `SemanticIndexBuilder` alongside `TypeInferenceBuilder`. You can refer to the [tracing docs](https://github.com/astral-sh/ruff/blob/main/crates/red_knot/docs/tracing.md) to turn on verbose logging. I'd also look at which expression it's failing to lookup and verify whether the semantic index that's built is correct or not by adding some `println` here and then basically follow the code path:

https://github.com/astral-sh/ruff/blob/1a8f29ea4141468f1772ff4e87da05b234a17ac2/crates/red_knot_python_semantic/src/semantic_model.rs#L55-L64

---

_Comment by @carljm on 2024-08-22 03:50_

The "recovery" here is recovering from a query cycle. The cycle might involve the definition of x and the use of x in the named expr (ie the definition wrongly ends up being visible to the use in the same expression?), or the cycle might involve the type of the if's test expression being resolved as a Constraint. 

---

_Comment by @dylwil3 on 2024-08-22 03:55_

> The "recovery" here is recovering from a query cycle. The cycle might involve the definition of x and the use of x in the named expr (ie the definition wrongly ends up being visible to the use in the same expression?), or the cycle might involve the type of the if's test expression being resolved as a Constraint.

In case it helps narrow it down, a more minimal example is just:
```python
x = 0
(x := x + 1)
```
So I think it's `x` vs `x` in the named expression that's causing a cycle.

---

_Comment by @dylwil3 on 2024-08-22 04:14_

Is this related to the issue Carl and Micha discussed in this thread: https://salsa.zulipchat.com/#narrow/stream/333573-salsa-3.2E0/topic/fixpoint.20cycle.20recovery ? Was a resolution ever found to the examples brought up there?

---

_Comment by @carljm on 2024-08-22 04:33_

We don't have a solution for those examples yet, but it shouldn't matter to this case. There shouldn't be any cycle here; the use of `x` on the RHS is evaluated first and doesn't see the definition it is part of. So there's a bug in the construction of the `UseDefMap` here and once it's fixed there shouldn't be any cycle in this example. 

---

_Comment by @carljm on 2024-08-22 04:40_

On my phone, but pretty sure the bug is in `SemanticIndexBuilder::visit_expr` in the case for `NamedExpr`; we are visiting the target (and thus creating the Definition) first, and then visiting the RHS. This is backwards and wrongly causes the definition to be visible to the RHS. We should instead visit the RHS first, just like at runtime the RHS is evaluated before the assignment occurs.

---

_Comment by @dylwil3 on 2024-08-22 11:56_

You were right! Should've tried that first since @MichaReiser pointed to the same spot way at the beginning 😄 

---

_Closed by @carljm on 2024-08-22 14:59_

---

_Closed by @carljm on 2024-08-22 14:59_

---
