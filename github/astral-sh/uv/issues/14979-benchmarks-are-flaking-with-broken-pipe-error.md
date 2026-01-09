---
number: 14979
title: Benchmarks are flaking with broken pipe error
type: issue
state: open
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-07-30T15:42:57Z
updated_at: 2025-07-30T15:43:02Z
url: https://github.com/astral-sh/uv/issues/14979
synced_at: 2026-01-07T13:12:19-06:00
---

# Benchmarks are flaking with broken pipe error

---

_Issue opened by @zanieb on 2025-07-30 15:42_

```
Measured: crates/uv-bench/benches/uv.rs::uv::resolve_warm_jupyter_universal::resolve_warm_jupyter_universal
  
  thread 'main' panicked at crates/uv-bench/benches/uv.rs:77:14:
  called `Result::unwrap()` on an `Err` value: Failed to download and build `pyspark==3.5.2`
  
  Caused by:
      0: Failed to extract archive: pyspark==3.5.2
      1: I/O operation failed during extraction
      2: failed to unpack `/home/runner/work/uv/uv/.cache/sdists-v9/.tmpQqnd3c/pyspark-3.5.2/deps/jars/mesos-1.4.3-shaded-protobuf.jar`
      3: failed to unpack `pyspark-3.5.2/deps/jars/mesos-1.4.3-shaded-protobuf.jar` into `/home/runner/work/uv/uv/.cache/sdists-v9/.tmpQqnd3c/pyspark-3.5.2/deps/jars/mesos-1.4.3-shaded-protobuf.jar`
      4: error decoding response body
      5: request or response body error
      6: error reading a body from connection
      7: stream closed because of a broken pipe
  
  Stack backtrace:
     0: anyhow::error::<impl core::convert::From<E> for anyhow::Error>::from
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/anyhow-1.0.98/src/backtrace.rs:27:14
     1: <core::result::Result<T,F> as core::ops::try_trait::FromResidual<core::result::Result<core::convert::Infallible,E>>>::from_residual
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/result.rs:2050:27
     2: uv::resolver::resolve::{{closure}}
               at ./benches/uv.rs:226:12
     3: <core::pin::Pin<P> as core::future::future::Future>::poll
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/future/future.rs:124:9
     4: tokio::runtime::scheduler::current_thread::CoreGuard::block_on::{{closure}}::{{closure}}::{{closure}}
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/scheduler/current_thread/mod.rs:742:54
     5: tokio::task::coop::with_budget
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/task/coop/mod.rs:167:5
     6: tokio::task::coop::budget
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/task/coop/mod.rs:133:5
     7: tokio::runtime::scheduler::current_thread::CoreGuard::block_on::{{closure}}::{{closure}}
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/scheduler/current_thread/mod.rs:742:25
     8: tokio::runtime::scheduler::current_thread::Context::enter
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/scheduler/current_thread/mod.rs:432:19
     9: tokio::runtime::scheduler::current_thread::CoreGuard::block_on::{{closure}}
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/scheduler/current_thread/mod.rs:741:36
    10: tokio::runtime::scheduler::current_thread::CoreGuard::enter::{{closure}}
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/scheduler/current_thread/mod.rs:829:68
    11: tokio::runtime::context::scoped::Scoped<T>::set
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/context/scoped.rs:40:9
    12: tokio::runtime::context::set_scheduler::{{closure}}
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/context.rs:176:26
    13: std::thread::local::LocalKey<T>::try_with
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/thread/local.rs:315:12
    14: std::thread::local::LocalKey<T>::with
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/thread/local.rs:279:15
    15: tokio::runtime::context::set_scheduler
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/context.rs:176:9
    16: tokio::runtime::scheduler::current_thread::CoreGuard::enter
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/scheduler/current_thread/mod.rs:829:27
    17: tokio::runtime::scheduler::current_thread::CoreGuard::block_on
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.47.0/src/runtime/scheduler/current_thread/mod.rs:729:19
    18: tokio::runtime::scheduler::current_thread::CurrentThread::block_on::{{closure}}
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:552:19
    44: std::panic::catch_unwind
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panic.rs:359:14
    45: std::rt::lang_start_internal
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/rt.rs:164:5
    46: main
    47: __libc_start_call_main
               at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
    48: __libc_start_main_impl
               at ./csu/../csu/libc-start.c:360:3
    49: _start
  stack backtrace:
     0: __rustc::rust_begin_unwind
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:6[97](https://github.com/astral-sh/uv/actions/runs/16626787091/job/47045342940?pr=14978#step:8:100):5
     1: core::panicking::panic_fmt
               at /rustc/6b00bc38801[98](https://github.com/astral-sh/uv/actions/runs/16626787091/job/47045342940?pr=14978#step:8:101)600130e1cf62b8f8a93494488cc/library/core/src/panicking.rs:75:14
     2: core::result::unwrap_failed
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/result.rs:1732:5
     3: core::result::Result<T,E>::unwrap
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/result.rs:1137:23
     4: uv::setup::{{closure}}
               at ./benches/uv.rs:69:9
     5: uv::resolve_warm_jupyter::{{closure}}::{{closure}}
               at ./benches/uv.rs:15:60
     6: codspeed_criterion_compat::compat_criterion::compat::bencher::Bencher::iter
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/codspeed-criterion-compat-3.0.4/src/./compat/bencher.rs:38:27
     7: uv::resolve_warm_airflow::{{closure}}
               at ./benches/uv.rs:32:50
     8: codspeed_criterion_compat::compat_criterion::compat::group::BenchmarkGroup<M>::bench_function::{{closure}}
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/codspeed-criterion-compat-3.0.4/src/./compat/group.rs:40:60
     9: codspeed_criterion_compat::compat_criterion::compat::group::BenchmarkGroup<M>::run_bench
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/codspeed-criterion-compat-3.0.4/src/./compat/group.rs:77:9
    10: codspeed_criterion_compat::compat_criterion::compat::group::BenchmarkGroup<M>::bench_function
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/codspeed-criterion-compat-3.0.4/src/./compat/group.rs:40:9
    11: codspeed_criterion_compat::compat_criterion::compat::criterion::Criterion<M>::bench_function
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/codspeed-criterion-compat-3.0.4/src/./compat/criterion.rs:59:9
    12: uv::resolve_warm_airflow
               at ./benches/uv.rs:32:5
    13: uv::uv
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/codspeed-criterion-compat-3.0.4/src/./compat/macros.rs:9:17
    14: uv::main
               at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/codspeed-criterion-compat-3.0.4/src/./compat/macros.rs:28:17
    15: core::ops::function::FnOnce::call_once
               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/ops/function.rs:250:5
  note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
  failed to execute the benchmark process, exit code: [101](https://github.com/astral-sh/uv/actions/runs/16626787091/job/47045342940?pr=14978#step:8:104)
```

---

_Label `ci-flake` added by @zanieb on 2025-07-30 15:43_

---
