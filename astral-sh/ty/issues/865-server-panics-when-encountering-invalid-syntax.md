```yaml
number: 865
title: "Server panics when encountering invalid syntax related to `Callable` annotations"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - server
  - fatal
assignees: []
created_at: 2025-07-21T18:14:32Z
updated_at: 2025-07-25T02:05:34Z
url: https://github.com/astral-sh/ty/issues/865
synced_at: 2026-01-10T02:06:24Z
```

# Server panics when encountering invalid syntax related to `Callable` annotations

---

_Issue opened by @AlexWaygood on 2025-07-21 18:14_

### Summary

It's quite easy to get the server to panic when encountering invalid syntax involving `Callable` if it appears as a type annotation. E.g. adding this as a corpus test panics:

```py
from typing import Callable

def _(x: Callable[..,
```

So does this:

```py
from typing import Callable

def _(x: Callable[.., None]
```

and this:

```py
from typing import Callable

def _(x: Callable[.., None])
```

and this:

```py
from typing import Callable

def _(x: Callable[.., None]):
```

and this:

```py
from typing import Callable

def _(x: Callable[.., None]): ...
```

I came across this by accident while doing something like this in the playground:

https://github.com/user-attachments/assets/0e906a48-6b27-44cf-ba2e-d2ee0f3fa852

---

_Label `bug` added by @AlexWaygood on 2025-07-21 18:14_

---

_Label `server` added by @AlexWaygood on 2025-07-21 18:14_

---

_Label `fatal` added by @AlexWaygood on 2025-07-21 18:14_

---

_Renamed from "Server panics when encountering invalid syntax" to "Server panics when encountering invalid syntax related to `Callable` annotations" by @AlexWaygood on 2025-07-21 18:14_

---

_Comment by @AlexWaygood on 2025-07-21 18:15_

This is similar to https://github.com/astral-sh/ty/issues/738 but it might also be related to https://github.com/astral-sh/ty/issues/815

---

_Comment by @dhruvmanila on 2025-07-24 04:52_

In an editor context, it only occurs when `python.ty.disableLanguageServices` is `false` i.e., we need to make sure the server goes through the path where it requires more semantic information which leads me to the following log which says that it's happening when the server resolve the semantic tokens:

```
2025-07-24 10:21:28.299975000 ERROR request{id=5 method="textDocument/semanticTokens/full"}: An error occurred with request ID 5: request handler panicked at crates/ty_python_semantic/src/semantic_model.rs:304:44:
Failed to retrieve the inferred type for an `ast::Expr` node passed to `TypeInference::expression_type()`. The `TypeInferenceBuilder` should infer and store types for all `ast::Expr` nodes in any `TypeInference` region it analyzes.
Backtrace:    0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
   2: std::backtrace::Backtrace::create
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/backtrace.rs:331:13
   3: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ruff_db/src/panic.rs:91:34
   4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/alloc/src/boxed.rs:1980:9
   5: std::panicking::rust_panic_with_hook
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:841:13
   6: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:706:13
   7: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/sys/backtrace.rs:168:18
   8: __rustc::rust_begin_unwind
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:697:5
   9: core::panicking::panic_fmt
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/panicking.rs:75:14
  10: core::panicking::panic_display
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/panicking.rs:269:5
  11: core::option::expect_failed
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/option.rs:2049:5
  12: core::option::Option<T>::expect
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:958:21
  13: ty_python_semantic::types::infer::TypeInference::expression_type
             at /Users/dhruv/work/astral/ruff/crates/ty_python_semantic/src/types/infer.rs:462:9
  14: <ruff_python_ast::generated::ExprRef as ty_python_semantic::semantic_model::HasType>::inferred_type
             at /Users/dhruv/work/astral/ruff/crates/ty_python_semantic/src/semantic_model.rs:304:9
  15: <ruff_python_ast::generated::ExprName as ty_python_semantic::semantic_model::HasType>::inferred_type
             at /Users/dhruv/work/astral/ruff/crates/ty_python_semantic/src/semantic_model.rs:314:17
  16: ty_ide::semantic_tokens::SemanticTokenVisitor::classify_name
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:269:18
  17: <ty_ide::semantic_tokens::SemanticTokenVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_expr
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:677:47
  18: <ty_ide::semantic_tokens::SemanticTokenVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_expr
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:683:17
  19: <ty_ide::semantic_tokens::SemanticTokenVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_expr
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:683:17
  20: ruff_python_ast::generated::ExprSlice::visit_source_order
             at /Users/dhruv/work/astral/ruff/crates/ruff_python_ast/src/generated.rs:10566:13
  21: ruff_python_ast::visitor::source_order::walk_expr
             at /Users/dhruv/work/astral/ruff/crates/ruff_python_ast/src/visitor/source_order.rs:296:34
  22: <ty_ide::semantic_tokens::SemanticTokenVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_expr
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:723:17
  23: ruff_python_ast::generated::ExprTuple::visit_source_order
             at /Users/dhruv/work/astral/ruff/crates/ruff_python_ast/src/generated.rs:10543:13
  24: ruff_python_ast::visitor::source_order::walk_expr
             at /Users/dhruv/work/astral/ruff/crates/ruff_python_ast/src/visitor/source_order.rs:295:34
  25: <ty_ide::semantic_tokens::SemanticTokenVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_expr
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:723:17
  26: ruff_python_ast::generated::ExprSubscript::visit_source_order
             at /Users/dhruv/work/astral/ruff/crates/ruff_python_ast/src/generated.rs:10478:9
  27: ruff_python_ast::visitor::source_order::walk_expr
             at /Users/dhruv/work/astral/ruff/crates/ruff_python_ast/src/visitor/source_order.rs:291:38
  28: <ty_ide::semantic_tokens::SemanticTokenVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_expr
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:723:17
  29: ty_ide::semantic_tokens::SemanticTokenVisitor::visit_type_annotation
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:474:9
  30: ty_ide::semantic_tokens::SemanticTokenVisitor::visit_parameters
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:519:17
  31: <ty_ide::semantic_tokens::SemanticTokenVisitor as ruff_python_ast::visitor::source_order::SourceOrderVisitor>::visit_stmt
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:567:17
  32: ruff_python_ast::visitor::source_order::walk_body
             at /Users/dhruv/work/astral/ruff/crates/ruff_python_ast/src/visitor/source_order.rs:208:9
  33: ruff_python_ast::visitor::source_order::SourceOrderVisitor::visit_body
             at /Users/dhruv/work/astral/ruff/crates/ruff_python_ast/src/visitor/source_order.rs:146:9
  34: ty_ide::semantic_tokens::semantic_tokens
             at /Users/dhruv/work/astral/ruff/crates/ty_ide/src/semantic_tokens.rs:189:5
  35: ty_server::server::api::semantic_tokens::generate_semantic_tokens
             at /Users/dhruv/work/astral/ruff/crates/ty_server/src/server/api/semantic_tokens.rs:20:31
  36: <ty_server::server::api::requests::semantic_tokens::SemanticTokensRequestHandler as ty_server::server::api::traits::BackgroundDocumentRequestHandler>::run_with_snapshot
             at /Users/dhruv/work/astral/ruff/crates/ty_server/src/server/api/requests/semantic_tokens.rs:37:26
  37: ty_server::server::api::background_document_request_task::{{closure}}::{{closure}}::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ty_server/src/server/api.rs:268:17
  38: std::panicking::try::do_call
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  39: ___rust_try
  40: std::panicking::try
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  41: std::panic::catch_unwind
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  42: ruff_db::panic::catch_unwind
             at /Users/dhruv/work/astral/ruff/crates/ruff_db/src/panic.rs:122:18
  43: ty_server::server::api::background_document_request_task::{{closure}}::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ty_server/src/server/api.rs:267:26
  44: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  45: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed.rs:1966:9
  46: ty_server::server::schedule::Scheduler::dispatch::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ty_server/src/server/schedule.rs:59:36
  47: ty_server::server::schedule::thread::pool::Pool::spawn::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ty_server/src/server/schedule/thread/pool.rs:124:13
  48: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  49: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed.rs:1966:9
  50: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
  51: std::panicking::try::do_call
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  52: ___rust_try
  53: std::panicking::try
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  54: std::panic::catch_unwind
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  55: ty_server::server::schedule::thread::pool::Pool::new::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ty_server/src/server/schedule/thread/pool.rs:80:49
  56: ty_server::server::schedule::thread::Builder::spawn::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ty_server/src/server/schedule/thread.rs:70:13
  57: std::sys::backtrace::__rust_begin_short_backtrace
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
  58: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559:17
  59: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
  60: std::panicking::try::do_call
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  61: ___rust_try
  62: std::panicking::try
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  63: std::panic::catch_unwind
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  64: std::thread::Builder::spawn_unchecked_::{{closure}}
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557:30
  65: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /Users/dhruv/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  66: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/alloc/src/boxed.rs:1966:9
  67: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/alloc/src/boxed.rs:1966:9
  68: std::sys::pal::unix::thread::Thread::new::thread_start
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/sys/pal/unix/thread.rs:97:17
  69: __pthread_cond_wait
```

And, this is where it's triggered:

https://github.com/astral-sh/ruff/blob/e0149cd9f3712dbe719335216015f09ceab0e70c/crates/ty_ide/src/semantic_tokens.rs#L268-L269

---

_Comment by @dhruvmanila on 2025-07-24 05:06_

This might be resolved by astral-sh/ruff#19517.

---

_Closed by @carljm on 2025-07-25 02:05_

---
