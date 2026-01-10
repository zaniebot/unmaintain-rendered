```yaml
number: 1226
title: Stack overflow, but only when running ty with AddressSanitizer
type: issue
state: closed
author: qarmin
labels:
  - testing
  - fuzzer
assignees: []
created_at: 2025-09-21T09:08:41Z
updated_at: 2025-12-05T13:08:47Z
url: https://github.com/astral-sh/ty/issues/1226
synced_at: 2026-01-10T01:56:40Z
```

# Stack overflow, but only when running ty with AddressSanitizer

---

_Issue opened by @qarmin on 2025-09-21 09:08_

When running the file check with ty under AddressSanitizer, I get a stack overflow error.
Running ty normally, however, does not cause this issue.
AddressSanitizer uses more stack space, which is why the error becomes visible in this setup.

I’m not sure whether creating a larger file (e.g., by duplicating parts of it) would make it crash under normal ty as well — I haven’t been able to reproduce it that way. It’s possible that ty already has some form of internal limit in place.

I know this looks like an edge case, but I suspect something similar could happen in regular ty on systems where the default stack size is much smaller than on Linux.

File is quite big(~28KB), but contains such code(objs['...'] part is repeated multiple times)
```
#!/usr/bin/python3
"""An ASCII 5x9 (monospace) pattern."""
from pyxstitch.font import BaseFontFactory


class FiveByNineMono(BaseFontFactory):
    """Font factory definition."""

    def _display(self):
        """Display name."""
        return self._monospace_ascii()

    def _height_width(self):
        """Font height and width."""
        return (9, 5)

    def _initialize_characters(self):
        """Initialize default characters."""
        objs = {}
        objs['A'] = self._build_character("""
|    |    |1.00|    |    |
|    |1.00|    |1.00|    |
|1.00|    |    |    |1.00|
|1.00|1.00|1.00|1.00|1.00|
|1.00|    |    |    |1.00|
|1.00|    |    |    |1.00|
|1.00|    |    |    |1.00|
|    |    |    |    |    |
|    |    |    |    |    |
""")

...

        objs['~'] = self._build_character("""
|    |    |    |    |    |
|    |    |    |    |    |
|    |1.00|    |    |    |
|1.00|    |1.00|    |1.00|
|    |    |    |1.00|    |
|    |    |    |    |    |
|    |    |    |    |    |
|    |    |    |    |    |
|    |    |    |    |    |
""")
        objs['`'] = self._build_character("""
|    |1.00|    |    |    |
|    |    |1.00|    |    |
|    |    |    |1.00|    |
|    |    |    |    |    |
|    |    |    |    |    |
|    |    |    |    |    |
|    |    |    |    |    |
|    |    |    |    |    |
|    |    |    |    |    |
""")
        return objs
```

command
```
timeout -v 40 ty check TEST___FILE.py
```

App was compiled with nightly rust compiler to be able to use address sanitizer
(You can ignore this part if there is no address sanitizer error)
On Ubuntu 24.04, the commands to compile were:
```
rustup default nightly
rustup component add rust-src --toolchain nightly-x86_64-unknown-linux-gnu
rustup component add llvm-tools-preview --toolchain nightly-x86_64-unknown-linux-gnu

export RUST_BACKTRACE=1 # or full depending on project
export ASAN_SYMBOLIZER_PATH=$(which llvm-symbolizer-18)
export ASAN_OPTIONS=symbolize=1
RUSTFLAGS="-Zsanitizer=address" cargo +nightly build --target x86_64-unknown-linux-gnu
```

cause this
```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
AddressSanitizer:DEADLYSIGNAL
=================================================================
==137935==ERROR: AddressSanitizer: stack-overflow on address 0x7bc352fff6e0 (pc 0x7fc356f88a21 bp 0x7bc352ffe510 sp 0x7bc352ffdcc8 T1)
    #0 0x7fc356f88a21  (/lib/x86_64-linux-gnu/libc.so.6+0x188a21) (BuildId: 282c2c16e7b6600b0b22ea0c99010d2795752b5f)
    #1 0x55da1120ea1b in __asan_memcpy /rustc/llvm/src/llvm-project/compiler-rt/lib/asan/asan_interceptors_memintrinsics.cpp:63:3
    #2 0x55da1209508d in ty_python_semantic::semantic_index::builder::SemanticIndexBuilder::add_place::hb5493e89bae3c3b9 /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ty_python_semantic/src/semantic_index/builder.rs:583:74
    #3 0x55da1209508d in _$LT$ty_python_semantic..semantic_index..builder..SemanticIndexBuilder$u20$as$u20$ruff_python_ast..visitor..Visitor$GT$::visit_expr::hc6788a153953f4ab /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ty_python_semantic/src/semantic_index/builder.rs:2340:41
    #4 0x55da12bc80a5 in ruff_python_ast::visitor::walk_expr::h4719d17c0df48d3f /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ruff_python_ast/src/visitor.rs
    #5 0x55da12096179 in _$LT$ty_python_semantic..semantic_index..builder..SemanticIndexBuilder$u20$as$u20$ruff_python_ast..visitor..Visitor$GT$::visit_expr::hc6788a153953f4ab /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/collections/hash/map.rs
    #6 0x55da12bc7e35 in ruff_python_ast::visitor::walk_expr::h4719d17c0df48d3f /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ruff_python_ast/src/visitor.rs:393:21
    #7 0x55da12096179 in _$LT$ty_python_semantic..semantic_index..builder..SemanticIndexBuilder$u20$as$u20$ruff_python_ast..visitor..Visitor$GT$::visit_expr::hc6788a153953f4ab /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/collections/hash/map.rs

...

    #239 0x55da12096179 in _$LT$ty_python_semantic..semantic_index..builder..SemanticIndexBuilder$u20$as$u20$ruff_python_ast..visitor..Visitor$GT$::visit_expr::hc6788a153953f4ab /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/collections/hash/map.rs
    #240 0x55da12bc7e35 in ruff_python_ast::visitor::walk_expr::h4719d17c0df48d3f /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ruff_python_ast/src/visitor.rs:393:21
    #241 0x55da12096179 in _$LT$ty_python_semantic..semantic_index..builder..SemanticIndexBuilder$u20$as$u20$ruff_python_ast..visitor..Visitor$GT$::visit_expr::hc6788a153953f4ab /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/collections/hash/map.rs
    #242 0x55da12bc7e35 in ruff_python_ast::visitor::walk_expr::h4719d17c0df48d3f /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ruff_python_ast/src/visitor.rs:393:21
    #243 0x55da12096179 in _$LT$ty_python_semantic..semantic_index..builder..SemanticIndexBuilder$u20$as$u20$ruff_python_ast..visitor..Visitor$GT$::visit_expr::hc6788a153953f4ab /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/collections/hash/map.rs
    #244 0x55da12bc7e35 in ruff_python_ast::visitor::walk_expr::h4719d17c0df48d3f /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ruff_python_ast/src/visitor.rs:393:21
    #245 0x55da12096179 in _$LT$ty_python_semantic..semantic_index..builder..SemanticIndexBuilder$u20$as$u20$ruff_python_ast..visitor..Visitor$GT$::visit_expr::hc6788a153953f4ab /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/collections/hash/map.rs
    #246 0x55da12bc7e35 in ruff_python_ast::visitor::walk_expr::h4719d17c0df48d3f /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ruff_python_ast/src/visitor.rs:393:21
    #247 0x55da12096179 in _$LT$ty_python_semantic..semantic_index..builder..SemanticIndexBuilder$u20$as$u20$ruff_python_ast..visitor..Visitor$GT$::visit_expr::hc6788a153953f4ab /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/collections/hash/map.rs

SUMMARY: AddressSanitizer: stack-overflow (/lib/x86_64-linux-gnu/libc.so.6+0x188a21) (BuildId: 282c2c16e7b6600b0b22ea0c99010d2795752b5f) 
Thread T1 created by T0 here:
    #0 0x55da111f5301 in pthread_create /rustc/llvm/src/llvm-project/compiler-rt/lib/asan/asan_interceptors.cpp:250:3
    #1 0x55da13c19a86 in std::sys::thread::unix::Thread::new::h2555aeb2bcdf6327 /rustc/0be8e16088894483a7012c5026c3247c14a0c3c2/library/std/src/sys/thread/unix.rs:104:19
    #2 0x55da11f67a30 in std::thread::Builder::spawn_unchecked_::h85a7f1c7b1a25ff3 /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:598:30
    #3 0x55da11f67a30 in std::thread::Builder::spawn_unchecked::h541d1a865816a5bb /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:467:37
    #4 0x55da11f55e89 in std::thread::Builder::spawn::h65b7effa7540d50f /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:399:23
    #5 0x55da11f55e89 in _$LT$rayon_core..registry..DefaultSpawn$u20$as$u20$rayon_core..registry..ThreadSpawn$GT$::spawn::hc5eca8b024d3c5d4 /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:95:11
    #6 0x55da112c4318 in rayon_core::registry::Registry::new::hc4065a4219b0d8e7 /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:313:57
    #7 0x55da112c4318 in rayon_core::registry::init_global_registry::_$u7b$$u7b$closure$u7d$$u7d$::hef75eb5d1ebe8484 /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:182:28
    #8 0x55da112c4318 in rayon_core::registry::set_global_registry::_$u7b$$u7b$closure$u7d$$u7d$::h55b3d26ab058ed6e /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:196:18
    #9 0x55da112c4318 in std::sync::poison::once::Once::call_once::_$u7b$$u7b$closure$u7d$$u7d$::h35b4127dc3bdebd1 /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sync/poison/once.rs:156:41
    #10 0x55da13c189a6 in std::sys::sync::once::futex::Once::call::hbf65caffdcf730e3 /rustc/0be8e16088894483a7012c5026c3247c14a0c3c2/library/std/src/sys/sync/once/futex.rs:178:21
    #11 0x55da112b442e in std::sync::poison::once::Once::call_once::hc999ea7115fd9b82 /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sync/poison/once.rs:156:20
    #12 0x55da112b442e in rayon_core::registry::set_global_registry::hd6b9ca0cd8fef619 /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:195:22
    #13 0x55da112a58f6 in rayon_core::registry::init_global_registry::hc54bf32f7537e6ee /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:182:5
    #14 0x55da112a58f6 in rayon_core::ThreadPoolBuilder$LT$S$GT$::build_global::h022e2f660dbc55aa /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/lib.rs:278:24
    #15 0x55da1124609b in ty::setup_rayon::hfe165997c724802c /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ty/src/lib.rs:518:10
    #16 0x55da1124609b in ty::run::ha867af71524ec85b /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ty/src/lib.rs:36:5
    #17 0x55da1123f35f in ty::main::h2ba48b027e072ccf /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff-main/crates/ty/src/main.rs:6:5
    #18 0x55da11240162 in core::ops::function::FnOnce::call_once::h8f0a9857e62db6fd /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
    #19 0x55da11240162 in std::sys::backtrace::__rust_begin_short_backtrace::h65cdbea652bbb29c /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:158:18
    #20 0x55da1124178b in main (/home/runner/.cargo/bin/ty+0x13c578b) (BuildId: b6a5d2a29c33627ac412f6c89dca6396e1c24bf2)
    #21 0x7fc356e2a1c9  (/lib/x86_64-linux-gnu/libc.so.6+0x2a1c9) (BuildId: 282c2c16e7b6600b0b22ea0c99010d2795752b5f)
    #22 0x7fc356e2a28a in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2a28a) (BuildId: 282c2c16e7b6600b0b22ea0c99010d2795752b5f)
    #23 0x55da11182e64 in _start (/home/runner/.cargo/bin/ty+0x1306e64) (BuildId: b6a5d2a29c33627ac412f6c89dca6396e1c24bf2)

==137935==ABORTING

##### Automatic Fuzzer note, output status "Some(1)", output signal "None"
```

File - [compressed.zip](https://github.com/user-attachments/files/22450800/compressed.zip)

---

_Label `fatal` added by @AlexWaygood on 2025-09-21 09:36_

---

_Label `fatal` removed by @MichaReiser on 2025-09-22 08:15_

---

_Label `testing` added by @MichaReiser on 2025-09-22 08:15_

---

_Comment by @MichaReiser on 2025-09-22 08:18_

ty is highly recursive. That's why stack overflows are certainly possible and why we increased the default stack size. I'll merge this into https://github.com/astral-sh/ty/issues/96 as it isn't address sanitizer specific, it's just that it's more likely to happen with an address sanitizer because of its overhead (as you pointed out)

---

_Closed by @MichaReiser on 2025-09-22 08:18_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-05 13:08_

---
