```yaml
number: 3039
title: Unable to build v0.0.248 with rustc 1.67.0
type: issue
state: closed
author: winterqt
labels: []
assignees: []
created_at: 2023-02-19T18:26:04Z
updated_at: 2023-02-21T15:51:02Z
url: https://github.com/astral-sh/ruff/issues/3039
synced_at: 2026-01-12T15:54:43Z
```

# Unable to build v0.0.248 with rustc 1.67.0

---

_@winterqt_

When building Ruff v0.0.248 with rustc 1.67.0, I receive the following compilation error:

```rust
error[E0080]: evaluation of constant value failed
   --> crates/ruff_formatter/src/utility_types.rs:5:24
    |
5   |         const _: i32 = 0 / $expr as i32;
    |                        ^^^^^^^^^^^^^^^^ attempt to divide `0_i32` by zero
    |
   ::: crates/ruff_formatter/src/format_element.rs:388:1
    |
388 | static_assert!(std::mem::size_of::<crate::FormatElement>() == 24usize);
    | ---------------------------------------------------------------------- in this macro invocation
    |
    = note: this error originates in the macro `static_assert` (in Nightly builds, run with -Z macro-backtrace for more info)
```

```
rustc 1.67.0 (fc594f156 2023-01-24) (built from a source tarball)
binary: rustc
commit-hash: fc594f15669680fa70d255faec3ca3fb507c3405
commit-date: 2023-01-24
host: aarch64-apple-darwin
release: 1.67.0
LLVM version: 15.0.7
```

---

_Comment by @figsoda on 2023-02-19 18:32_

I ran into a similar but different issue on x86_64 linux for both rust 1.67.0 and nightly

```rust
error: `T` does not live long enough
  --> crates/ruff_python_formatter/src/shared_traits.rs:36:9
   |
36 |         self.as_ref().map(AsFormat::format)
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: could not compile `ruff_python_formatter` due to previous error
warning: build failed, waiting for other jobs to finish...
```
```
rustc 1.67.0 (fc594f156 2023-01-24) (built from a source tarball)
binary: rustc
commit-hash: fc594f15669680fa70d255faec3ca3fb507c3405
commit-date: 2023-01-24
host: x86_64-unknown-linux-gnu
release: 1.67.0
LLVM version: 15.0.7
```
```
rustc 1.69.0-nightly (4507fdaaa 2023-02-18)
binary: rustc
commit-hash: 4507fdaaa27ea2fb59a41df2ce7d1f290da53dae
commit-date: 2023-02-18
host: x86_64-unknown-linux-gnu
release: 1.69.0-nightly
LLVM version: 15.0.7
```

---

_Comment by @charliermarsh on 2023-02-19 19:22_

Hmm, it's probably not a _new_ error, but might be a result of changing our default members. Can you try `cargo build -p ruff_cli`?

---

_Comment by @charliermarsh on 2023-02-19 19:23_

(Though of course we should fix those. I can take a look.)

---

_Comment by @winterqt on 2023-02-19 19:37_

If it helps, this could have occurred at any point between 0.0.245 and 248. If you'd like, I can bisect.

---

_Comment by @figsoda on 2023-02-19 19:59_

For my error, the regression happened at the 1.66.0 release

```bash
nix shell fenix/6369e55b8ba49b896da496c06013d7cb0d1395c6#stable.minimalToolchain nixpkgs#gnumake -c cargo build
```

works, but the same command fails on https://github.com/nix-community/fenix/commit/d3eaf97d81161bea9177cc80e07d26ba2d96569f

https://blog.rust-lang.org/2022/12/15/Rust-1.66.0.html

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-19 20:11_

---

_Comment by @charliermarsh on 2023-02-19 20:11_

Thanks, taking a look now. We build on 1.65.0, I see the same issue as @figsoda on 1.66.0.

---

_Comment by @charliermarsh on 2023-02-19 20:15_

I can't reproduce @winterqt's issue though.

---

_Comment by @winterqt on 2023-02-19 20:21_

@charliermarsh On Linux, or macOS?

---

_Comment by @charliermarsh on 2023-02-19 20:24_

macOS, Rust 1.67.0. Let me try a `cargo clean` though.

---

_Comment by @charliermarsh on 2023-02-19 20:30_

Still no luck, any ideas?

---

_Comment by @winterqt on 2023-02-19 20:44_

Sorry, to be clear: aarch64 macOS or x86_64?

---

_Comment by @charliermarsh on 2023-02-19 20:44_

Oh sorry, aarch64 :)

---

_Comment by @charliermarsh on 2023-02-19 20:45_

```❯ rustc --version --verbose
rustc 1.67.0 (fc594f156 2023-01-24)
binary: rustc
commit-hash: fc594f15669680fa70d255faec3ca3fb507c3405
commit-date: 2023-01-24
host: aarch64-apple-darwin
release: 1.67.0
LLVM version: 15.0.6
```

---

_Comment by @winterqt on 2023-02-19 20:46_

Not a clue. Would it help if I got whatever size the type is now, though?

---

_Comment by @charliermarsh on 2023-02-19 20:47_

Yeah sure.

---

_Comment by @winterqt on 2023-02-19 21:08_

I get 32 as the size of `FormatElement`.

This isn't an entirely vanilla rustc, we apply one patch to fix an ICE (https://github.com/rust-lang/rust/pull/107688). Can you try to reproduce this on nightly?

---

_Comment by @charliermarsh on 2023-02-20 04:26_

I somehow just hit this. I have no idea what changed, if anything.

---

_Comment by @charliermarsh on 2023-02-20 04:34_

Oh, it was probably just always failing on release builds for that crate, but we never tried to build them since they weren't part of `default-members`.

---

_Comment by @winterqt on 2023-02-20 04:39_

Forgive me if I'm being dumb, but I don't see `default-members` defined anywhere? Either way, we're just running `cargo build` 🤔

---

_Comment by @charliermarsh on 2023-02-20 04:40_

We _used_ to define it, but we removed it: https://github.com/charliermarsh/ruff/pull/3006. So if you're just running `cargo build`, you're now building more crates than before. (`cargo build -p ruff_cli` fixes it.)

---

_Comment by @charliermarsh on 2023-02-20 04:42_

(If you want to build Ruff for distribution, you should be running `cargo build -p ruff_cli` anyway.)

---

_Comment by @alerque on 2023-02-21 10:16_

This is kind of a problem for distributions because many of them are using the `maturin build` option and installing from the generated Wheels. I'm not sure this is the *right* method for distros to be using, but many are. Maturin doesn't seem capable of passing along the `-p ruff_cli` option. I'm a little confused why many/most distros have gone that route. It seems like if the Python API is needed a separate python-ruff library package would be the ticket, but in the mean time is there a way to not have the default builds include things that can't be built with the latest stable Rust toolchain?

---

_Comment by @charliermarsh on 2023-02-21 15:47_

Yeah this should be all fixed up -- I believe I fixed the issues in the non `ruff_cli` crates, and will avoid such breakages going forward.

---

_Closed by @charliermarsh on 2023-02-21 15:47_

---

_Comment by @charliermarsh on 2023-02-21 15:51_

(If you still see errors on `main`, please let me know and I'll of course re-open!)

---
