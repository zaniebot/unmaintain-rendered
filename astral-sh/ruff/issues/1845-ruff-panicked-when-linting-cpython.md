```yaml
number: 1845
title: ruff panicked when linting cpython 
type: issue
state: closed
author: messense
labels:
  - bug
assignees: []
created_at: 2023-01-13T05:51:24Z
updated_at: 2023-01-14T13:03:43Z
url: https://github.com/astral-sh/ruff/issues/1845
synced_at: 2026-01-12T15:54:41Z
```

# ruff panicked when linting cpython 

---

_@messense_

```bash
RUST_BACKTRACE=1 cargo run -- --no-cache ../cpython
   Compiling ruff v0.0.220 (/Users/messense/Projects/ruff)
    Finished dev [unoptimized + debuginfo] target(s) in 5.35s
     Running `target/debug/ruff --no-cache ../cpython`
thread '<unnamed>' panicked at 'byte index 1 is not a char boundary; it is inside 'Я' (bytes 0..2) of `ЯЧ}`', /Users/messense/.cargo/git/checkouts/rustpython-f8ef4d934ac33cd8/d532160/common/src/format.rs:902:31
stack backtrace:
   0: rust_begin_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panicking.rs:142:14
   2: core::str::slice_error_fail_rt
   3: core::str::slice_error_fail
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/str/mod.rs:86:9
   4: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::RangeFrom<usize>>::index
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/str/traits.rs:370:21
   5: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/str/traits.rs:65:9
   6: rustpython_common::format::FormatString::parse_spec
             at /Users/messense/.cargo/git/checkouts/rustpython-f8ef4d934ac33cd8/d532160/common/src/format.rs:902:31
   7: <rustpython_common::format::FormatString as rustpython_common::format::FromTemplate>::from_str::{{closure}}
             at /Users/messense/.cargo/git/checkouts/rustpython-f8ef4d934ac33cd8/d532160/common/src/format.rs:924:30
   8: core::result::Result<T,E>::or_else
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/result.rs:1440:23
   9: <rustpython_common::format::FormatString as rustpython_common::format::FromTemplate>::from_str
             at /Users/messense/.cargo/git/checkouts/rustpython-f8ef4d934ac33cd8/d532160/common/src/format.rs:923:24
  10: <ruff::pyflakes::format::FormatSummary as core::convert::TryFrom<&str>>::try_from
             at ./src/pyflakes/format.rs:35:29
  11: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_expr
             at ./src/checkers/ast.rs:1856:39
  12: ruff::ast::visitor::walk_stmt
             at ./src/ast/visitor.rs:253:37
  13: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_stmt
             at ./src/checkers/ast.rs:1585:18
  14: ruff::ast::visitor::walk_body
             at ./src/ast/visitor.rs:72:9
  15: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_body
             at ./src/checkers/ast.rs:3187:9
  16: ruff::ast::visitor::walk_stmt
             at ./src/ast/visitor.rs:199:13
  17: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_stmt
             at ./src/checkers/ast.rs:1585:18
  18: ruff::ast::visitor::walk_body
             at ./src/ast/visitor.rs:72:9
  19: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_body
             at ./src/checkers/ast.rs:3187:9
  20: ruff::checkers::ast::Checker::check_deferred_functions
             at ./src/checkers/ast.rs:3761:21
  21: ruff::checkers::ast::check_ast
             at ./src/checkers/ast.rs:4355:5
  22: ruff::linter::check_path
             at ./src/linter.rs:78:40
  23: ruff::linter::lint_only
             at ./src/linter.rs:238:23
  24: ruff::diagnostics::lint_path
             at ./src/diagnostics.rs:87:24
  25: ruff::commands::run::{{closure}}
             at ./src/commands.rs:99:21
  26: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &F>::call_mut
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/ops/function.rs:270:13
  27: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/iter/adapters/map.rs:84:28
  28: core::iter::traits::iterator::Iterator::fold
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/iter/traits/iterator.rs:2414:21
  29: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/iter/adapters/map.rs:124:9
  30: <rayon::iter::reduce::ReduceFolder<R,T> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/reduce.rs:105:19
  31: <rayon::iter::map::MapFolder<C,F> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/map.rs:248:21
  32: rayon::iter::plumbing::Producer::fold_with
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:110:9
  33: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:438:13
  34: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:427:21
  35: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:129:25
  36: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:96:9
  37: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:158:36
  38: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  39: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  40: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  41: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:418:21
  42: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:124:17
  43: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panic/unwind_safe.rs:271:9
  44: std::panicking::try::do_call
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:492:40
  45: ___rust_try
  46: std::panicking::try
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:456:19
  47: std::panic::catch_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panic.rs:137:14
  48: rayon_core::unwind::halt_unwinding
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/unwind.rs:17:5
  49: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:141:24
  50: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  51: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  52: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  53: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:418:21
  54: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:124:17
  55: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panic/unwind_safe.rs:271:9
  56: std::panicking::try::do_call
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:492:40
  57: ___rust_try
  58: std::panicking::try
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:456:19
  59: std::panic::catch_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panic.rs:137:14
  60: rayon_core::unwind::halt_unwinding
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/unwind.rs:17:5
  61: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:141:24
  62: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  63: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  64: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  65: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:427:21
  66: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:129:25
  67: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:96:9
  68: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:158:36
  69: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  70: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  71: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  72: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:418:21
  73: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:124:17
  74: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panic/unwind_safe.rs:271:9
  75: std::panicking::try::do_call
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:492:40
  76: ___rust_try
  77: std::panicking::try
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:456:19
  78: std::panic::catch_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panic.rs:137:14
  79: rayon_core::unwind::halt_unwinding
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/unwind.rs:17:5
  80: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:141:24
  81: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  82: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  83: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  84: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:427:21
  85: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:129:25
  86: rayon_core::job::JobResult<T>::call::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:212:41
  87: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panic/unwind_safe.rs:271:9
  88: std::panicking::try::do_call
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:492:40
  89: ___rust_try
  90: std::panicking::try
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:456:19
  91: std::panic::catch_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panic.rs:137:14
  92: rayon_core::unwind::halt_unwinding
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/unwind.rs:17:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
thread '<unnamed>' panicked at 'called `Result::unwrap()` on an `Err` value: UnmatchedBracket', src/pyflakes/format.rs:35:61
stack backtrace:
   0: rust_begin_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panicking.rs:142:14
   2: core::result::unwrap_failed
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/result.rs:1785:5
   3: core::result::Result<T,E>::unwrap
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/result.rs:1107:23
   4: <ruff::pyflakes::format::FormatSummary as core::convert::TryFrom<&str>>::try_from
             at ./src/pyflakes/format.rs:35:29
   5: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_expr
             at ./src/checkers/ast.rs:1856:39
   6: ruff::ast::visitor::walk_expr
             at ./src/ast/visitor.rs:359:17
   7: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_expr
             at ./src/checkers/ast.rs:2878:21
   8: ruff::ast::visitor::walk_stmt
             at ./src/ast/visitor.rs:253:37
   9: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_stmt
             at ./src/checkers/ast.rs:1585:18
  10: ruff::ast::visitor::walk_body
             at ./src/ast/visitor.rs:72:9
  11: <ruff::checkers::ast::Checker as ruff::ast::visitor::Visitor>::visit_body
             at ./src/checkers/ast.rs:3187:9
  12: ruff::checkers::ast::Checker::check_deferred_functions
             at ./src/checkers/ast.rs:3761:21
  13: ruff::checkers::ast::check_ast
             at ./src/checkers/ast.rs:4355:5
  14: ruff::linter::check_path
             at ./src/linter.rs:78:40
  15: ruff::linter::lint_only
             at ./src/linter.rs:238:23
  16: ruff::diagnostics::lint_path
             at ./src/diagnostics.rs:87:24
  17: ruff::commands::run::{{closure}}
             at ./src/commands.rs:99:21
  18: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &F>::call_mut
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/ops/function.rs:270:13
  19: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/iter/adapters/map.rs:84:28
  20: core::iter::traits::iterator::Iterator::fold
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/iter/traits/iterator.rs:2414:21
  21: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/iter/adapters/map.rs:124:9
  22: <rayon::iter::reduce::ReduceFolder<R,T> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/reduce.rs:105:19
  23: <rayon::iter::map::MapFolder<C,F> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/map.rs:248:21
  24: rayon::iter::plumbing::Producer::fold_with
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:110:9
  25: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:438:13
  26: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:427:21
  27: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:129:25
  28: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:96:9
  29: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:158:36
  30: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  31: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  32: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  33: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:418:21
  34: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:124:17
  35: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panic/unwind_safe.rs:271:9
  36: std::panicking::try::do_call
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:492:40
  37: ___rust_try
  38: std::panicking::try
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:456:19
  39: std::panic::catch_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panic.rs:137:14
  40: rayon_core::unwind::halt_unwinding
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/unwind.rs:17:5
  41: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:141:24
  42: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  43: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  44: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  45: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:418:21
  46: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:124:17
  47: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panic/unwind_safe.rs:271:9
  48: std::panicking::try::do_call
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:492:40
  49: ___rust_try
  50: std::panicking::try
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:456:19
  51: std::panic::catch_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panic.rs:137:14
  52: rayon_core::unwind::halt_unwinding
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/unwind.rs:17:5
  53: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:141:24
  54: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  55: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  56: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  57: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:427:21
  58: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:129:25
  59: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:96:9
  60: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:158:36
  61: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  62: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  63: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  64: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:418:21
  65: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:124:17
  66: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panic/unwind_safe.rs:271:9
  67: std::panicking::try::do_call
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:492:40
  68: ___rust_try
  69: std::panicking::try
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:456:19
  70: std::panic::catch_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panic.rs:137:14
  71: rayon_core::unwind::halt_unwinding
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/unwind.rs:17:5
  72: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:141:24
  73: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
  74: rayon_core::join::join_context
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:132:5
  75: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:416:47
  76: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.6.1/src/iter/plumbing/mod.rs:427:21
  77: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:129:25
  78: rayon_core::job::JobResult<T>::call::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:212:41
  79: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panic/unwind_safe.rs:271:9
  80: std::panicking::try::do_call
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:492:40
  81: ___rust_try
  82: std::panicking::try
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:456:19
  83: std::panic::catch_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panic.rs:137:14
  84: rayon_core::unwind::halt_unwinding
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/unwind.rs:17:5
  85: rayon_core::job::JobResult<T>::call
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:212:15
  86: <rayon_core::job::StackJob<L,F,R> as rayon_core::job::Job>::execute
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:114:32
  87: rayon_core::job::JobRef::execute
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/job.rs:58:9
  88: rayon_core::registry::WorkerThread::execute
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:804:9
  89: rayon_core::registry::WorkerThread::wait_until_cold
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:781:17
  90: rayon_core::registry::WorkerThread::wait_until
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:755:13
  91: rayon_core::join::join_context::{{closure}}
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/join/mod.rs:166:17
  92: rayon_core::registry::in_worker
             at /Users/messense/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.10.1/src/registry.rs:925:13
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

ruff version: main & 0.0.220
cpython commit: https://github.com/python/cpython/commit/94fc7706b7bc3d57cdd6d15bf8e8c4499ae53a69

---

_Comment by @messense on 2023-01-13 06:18_

Can be reproduced with a simple code

```python
"{a:%ЫйЯЧ}".format(a='a')
```

---

_Label `bug` added by @charliermarsh on 2023-01-14 04:20_

---

_Comment by @charliermarsh on 2023-01-14 13:03_

Hopefully closed by #1836.

---

_Closed by @charliermarsh on 2023-01-14 13:03_

---
