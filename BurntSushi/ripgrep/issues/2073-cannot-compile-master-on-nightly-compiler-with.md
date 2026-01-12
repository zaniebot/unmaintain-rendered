```yaml
number: 2073
title: Cannot compile master on nightly compiler with simd-accel
type: issue
state: closed
author: RobinVdBroeck
labels: []
assignees: []
created_at: 2021-11-16T15:51:12Z
updated_at: 2021-11-16T15:53:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2073
synced_at: 2026-01-12T16:13:24Z
```

# Cannot compile master on nightly compiler with simd-accel

---

_@RobinVdBroeck_

#### What version of ripgrep are you using?

Master branch at 7e05cde008c25940fb777ab1ae0c26c181692764

#### What operating system are you using ripgrep on?

Ubuntu 20.04 with `cargo 1.58.0-nightly (2e2a16e98 2021-11-08)` and `rustc 1.58.0-nightly (891ca5f63 2021-11-15)`

#### Describe your bug.

When you compile with `--features 'simd-accel'`

#### What are the steps to reproduce the behavior?

Run `cargo build --features 'simd-accel'`

#### What is the actual behavior?


```
> cargo +nightly build --features 'simd-accel'
   Compiling memchr v2.4.0
   Compiling cfg-if v1.0.0
   Compiling lazy_static v1.4.0
   Compiling libc v0.2.97
   Compiling proc-macro2 v1.0.27
   Compiling log v0.4.14
   Compiling unicode-xid v0.2.2
   Compiling libm v0.1.4
   Compiling regex-automata v0.1.10
   Compiling packed_simd_2 v0.3.5
   Compiling syn v1.0.73
   Compiling cfg-if v0.1.10
   Compiling encoding_rs v0.8.28
   Compiling serde_derive v1.0.126
   Compiling regex-syntax v0.6.25
   Compiling ryu v1.0.5
   Compiling serde v1.0.126
   Compiling bitflags v1.2.1
   Compiling unicode-width v0.1.8
   Compiling fnv v1.0.7
   Compiling same-file v1.0.6
   Compiling serde_json v1.0.64
   Compiling once_cell v1.7.2
   Compiling itoa v0.4.7
   Compiling strsim v0.8.0
   Compiling bytecount v0.6.2
   Compiling crossbeam-utils v0.8.5
   Compiling termcolor v1.1.2
   Compiling base64 v0.13.0
   Compiling textwrap v0.11.0
   Compiling walkdir v2.3.2
   Compiling thread_local v1.1.3
   Compiling clap v2.33.3
   Compiling bstr v0.2.16
   Compiling aho-corasick v0.7.18
   Compiling grep-matcher v0.1.5 (/home/robin/repos/ripgrep/crates/matcher)
   Compiling quote v1.0.9
error[E0557]: feature has been removed
   --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/lib.rs:215:5
    |
215 |     const_generics,
    |     ^^^^^^^^^^^^^^ feature has been removed
    |
    = note: removed in favor of `#![feature(adt_const_params)]` and `#![feature(generic_const_exprs)]`

   Compiling memmap2 v0.3.0
   Compiling atty v0.2.14
   Compiling num_cpus v1.13.0
   Compiling regex v1.5.4
error: type parameters must be declared prior to const parameters
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:20:54
   |
20 | pub unsafe fn __shuffle_vector2<const IDX: [u32; 2], T, U>(x: T, y: T) -> U
   |                                ----------------------^--^- help: reorder the parameters: lifetimes, then types, then consts: `<T, U, const IDX: [u32; 2]>`

error: type parameters must be declared prior to const parameters
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:30:54
   |
30 | pub unsafe fn __shuffle_vector4<const IDX: [u32; 4], T, U>(x: T, y: T) -> U
   |                                ----------------------^--^- help: reorder the parameters: lifetimes, then types, then consts: `<T, U, const IDX: [u32; 4]>`

error: type parameters must be declared prior to const parameters
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:40:54
   |
40 | pub unsafe fn __shuffle_vector8<const IDX: [u32; 8], T, U>(x: T, y: T) -> U
   |                                ----------------------^--^- help: reorder the parameters: lifetimes, then types, then consts: `<T, U, const IDX: [u32; 8]>`

error: type parameters must be declared prior to const parameters
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:50:56
   |
50 | pub unsafe fn __shuffle_vector16<const IDX: [u32; 16], T, U>(x: T, y: T) -> U
   |                                 -----------------------^--^- help: reorder the parameters: lifetimes, then types, then consts: `<T, U, const IDX: [u32; 16]>`

error: type parameters must be declared prior to const parameters
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:60:56
   |
60 | pub unsafe fn __shuffle_vector32<const IDX: [u32; 32], T, U>(x: T, y: T) -> U
   |                                 -----------------------^--^- help: reorder the parameters: lifetimes, then types, then consts: `<T, U, const IDX: [u32; 32]>`

error: type parameters must be declared prior to const parameters
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:70:56
   |
70 | pub unsafe fn __shuffle_vector64<const IDX: [u32; 64], T, U>(x: T, y: T) -> U
   |                                 -----------------------^--^- help: reorder the parameters: lifetimes, then types, then consts: `<T, U, const IDX: [u32; 64]>`

   Compiling globset v0.4.8 (/home/robin/repos/ripgrep/crates/globset)
   Compiling grep-regex v0.1.9 (/home/robin/repos/ripgrep/crates/regex)
   Compiling grep-cli v0.1.6 (/home/robin/repos/ripgrep/crates/cli)
   Compiling ignore v0.4.18 (/home/robin/repos/ripgrep/crates/ignore)
warning: field is never read: `which`
   --> crates/ignore/src/types.rs:126:9
    |
126 |         which: usize,
    |         ^^^^^^^^^^^^
    |
    = note: `#[warn(dead_code)]` on by default

warning: field is never read: `negated`
   --> crates/ignore/src/types.rs:128:9
    |
128 |         negated: bool,
    |         ^^^^^^^^^^^^^

error: `[u32; 2]` is forbidden as the type of a const generic parameter
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:20:44
   |
20 | pub unsafe fn __shuffle_vector2<const IDX: [u32; 2], T, U>(x: T, y: T) -> U
   |                                            ^^^^^^^^
   |
   = note: the only supported types are integers, `bool` and `char`
   = help: more complex types are supported with `#![feature(adt_const_params)]`

error: `[u32; 4]` is forbidden as the type of a const generic parameter
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:30:44
   |
30 | pub unsafe fn __shuffle_vector4<const IDX: [u32; 4], T, U>(x: T, y: T) -> U
   |                                            ^^^^^^^^
   |
   = note: the only supported types are integers, `bool` and `char`
   = help: more complex types are supported with `#![feature(adt_const_params)]`

error: `[u32; 8]` is forbidden as the type of a const generic parameter
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:40:44
   |
40 | pub unsafe fn __shuffle_vector8<const IDX: [u32; 8], T, U>(x: T, y: T) -> U
   |                                            ^^^^^^^^
   |
   = note: the only supported types are integers, `bool` and `char`
   = help: more complex types are supported with `#![feature(adt_const_params)]`

error: `[u32; 16]` is forbidden as the type of a const generic parameter
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:50:45
   |
50 | pub unsafe fn __shuffle_vector16<const IDX: [u32; 16], T, U>(x: T, y: T) -> U
   |                                             ^^^^^^^^^
   |
   = note: the only supported types are integers, `bool` and `char`
   = help: more complex types are supported with `#![feature(adt_const_params)]`

error: `[u32; 32]` is forbidden as the type of a const generic parameter
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:60:45
   |
60 | pub unsafe fn __shuffle_vector32<const IDX: [u32; 32], T, U>(x: T, y: T) -> U
   |                                             ^^^^^^^^^
   |
   = note: the only supported types are integers, `bool` and `char`
   = help: more complex types are supported with `#![feature(adt_const_params)]`

error: `[u32; 64]` is forbidden as the type of a const generic parameter
  --> /home/robin/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd_2-0.3.5/src/codegen/llvm.rs:70:45
   |
70 | pub unsafe fn __shuffle_vector64<const IDX: [u32; 64], T, U>(x: T, y: T) -> U
   |                                             ^^^^^^^^^
   |
   = note: the only supported types are integers, `bool` and `char`
   = help: more complex types are supported with `#![feature(adt_const_params)]`

   Compiling ripgrep v13.0.0 (/home/robin/repos/ripgrep)
warning: `ignore` (lib) generated 2 warnings
For more information about this error, try `rustc --explain E0557`.
error: could not compile `packed_simd_2` due to 13 previous errors
warning: build failed, waiting for other jobs to finish...
error: build failed
```

#### What is the expected behavior?

It should compile

---

_Comment by @BurntSushi on 2021-11-16 15:53_

This isn't a ripgrep problem, it's a problem with `packed_simd_2`. I would encourage you to report it there: https://github.com/rust-lang/packed_simd

Also, remember that `simd-accel` doesn't give you much. Unless you're specifically hurting from the time it takes for ripgrep to do UTF-16 transcoding, there is no other benefit.

---

_Closed by @BurntSushi on 2021-11-16 15:53_

---
