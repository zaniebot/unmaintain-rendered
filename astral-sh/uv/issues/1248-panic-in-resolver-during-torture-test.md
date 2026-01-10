```yaml
number: 1248
title: panic in resolver during torture test
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2024-02-05T01:01:26Z
updated_at: 2024-03-12T12:56:43Z
url: https://github.com/astral-sh/uv/issues/1248
synced_at: 2026-01-10T05:40:31Z
```

# panic in resolver during torture test

---

_Issue opened by @BurntSushi on 2024-02-05 01:01_

I've been hunting around for "torture tests," so I took the top 1k pypi packages and tried to run `puffin pip compile` on them. (I had to trim down the list though because a lot of packages don't build.) In so doing, I found a panic:

```
$ RUST_BACKTRACE=1 puffin-main pip compile pypi_top_1k_flat.txt -o /dev/null
⠇ nvidia-cuda-runtime-cu12==12.1.105                                                                                                                                                                                             thread 'main' panicked at crates/puffin-resolver/src/resolution.rs:175:50:
no entry found for key
stack backtrace:
⠋ azure-mgmt-documentdb==0.1.3                                                                                                                                                                                                      0: rust_begin_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:645:5
   1: core::panicking::panic_fmt
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:72:14
   2: core::panicking::panic_display
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:178:5
   3: core::panicking::panic_str
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:152:5
   4: core::option::expect_failed
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/option.rs:1985:5
   5: puffin_resolver::resolution::ResolutionGraph::from_state
   6: puffin_resolver::resolver::Resolver<Provider>::solve::{{closure}}::{{closure}}
             at ./crates/puffin-resolver/src/resolver/mod.rs:280:24
   7: puffin_resolver::resolver::Resolver<Provider>::solve::{{closure}}
             at ./crates/puffin-resolver/src/resolver/mod.rs:242:5
   8: <futures_util::future::future::fuse::Fuse<Fut> as core::future::future::Future>::poll
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/futures-util-0.3.30/src/future/future/fuse.rs:84:26
   9: puffin_resolver::resolver::Resolver<Provider>::resolve::{{closure}}::{{closure}}
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/macros/select.rs:524:49
  10: <tokio::future::poll_fn::PollFn<F> as core::future::future::Future>::poll
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/future/poll_fn.rs:58:9
  11: puffin_resolver::resolver::Resolver<Provider>::resolve::{{closure}}
             at ./crates/puffin-resolver/src/resolver/mod.rs:214:26
  12: puffin::commands::pip_compile::pip_compile::{{closure}}
             at ./crates/puffin/src/commands/pip_compile.rs:299:47
  13: puffin::run::{{closure}}::{{closure}}
             at ./crates/puffin/src/main.rs:731:14
  14: puffin::run::{{closure}}
             at ./crates/puffin/src/main.rs:618:1
  15: tokio::runtime::park::CachedParkThread::block_on::{{closure}}
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/park.rs:282:63
  16: tokio::runtime::coop::with_budget
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/coop.rs:107:5
  17: tokio::runtime::coop::budget
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/coop.rs:73:5
  18: tokio::runtime::park::CachedParkThread::block_on
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/park.rs:282:31
  19: tokio::runtime::context::blocking::BlockingRegionGuard::block_on
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/context/blocking.rs:66:9
  20: tokio::runtime::scheduler::multi_thread::MultiThread::block_on::{{closure}}
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/scheduler/multi_thread/mod.rs:87:13
  21: tokio::runtime::context::runtime::enter_runtime
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/context/runtime.rs:65:16
  22: tokio::runtime::scheduler::multi_thread::MultiThread::block_on
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/scheduler/multi_thread/mod.rs:86:9
  23: tokio::runtime::runtime::Runtime::block_on
             at /home/andrew/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tokio-1.35.1/src/runtime/runtime.rs:350:50
  24: puffin::main
             at ./crates/puffin/src/main.rs:904:9
  25: core::ops::function::FnOnce::call_once
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

I've attached the `~/astral/tmp/reqs/pypi_top_1k_flat.txt` input file.
[pypi_top_1k_flat.txt](https://github.com/astral-sh/puffin/files/14159238/pypi_top_1k_flat.txt)

---

_Label `bug` added by @BurntSushi on 2024-02-05 01:01_

---

_Renamed from "panic in resolver during tortue test" to "panic in resolver during torture test" by @AlexWaygood on 2024-02-05 12:06_

---

_Comment by @konstin on 2024-03-12 12:56_

This should be fixed by #2360.

---

_Closed by @konstin on 2024-03-12 12:56_

---
