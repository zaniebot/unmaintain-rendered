```yaml
number: 671
title: "ignore: test_path_should_be_under_root fails"
type: issue
state: closed
author: igor-raits
labels:
  - question
assignees: []
created_at: 2017-11-10T15:28:16Z
updated_at: 2018-07-29T12:31:55Z
url: https://github.com/BurntSushi/ripgrep/issues/671
synced_at: 2026-01-12T16:13:22Z
```

# ignore: test_path_should_be_under_root fails

---

_@igor-raits_

```rust
---- test_path_should_be_under_root stdout ----
	thread 'test_path_should_be_under_root' panicked at 'assertion failed: false', tests/gitignore_matched_path_or_any_parents_tests.rs:26:4
stack backtrace:
   0:     0x5606b3c94a73 - std::sys::imp::backtrace::tracing::imp::unwind_backtrace::h60b1e682f2593357
   1:     0x5606b3c91304 - std::sys_common::backtrace::_print::hf6b70c47b6df06fd
   2:     0x5606b3c96d73 - std::panicking::default_hook::{{closure}}::hfa6c1b0647e28bfe
   3:     0x5606b3c96a75 - std::panicking::default_hook::h41352babe92712f5
   4:     0x5606b3c97246 - std::panicking::rust_panic_with_hook::he3f5d1b3fe35e66c
   5:     0x5606b3bec437 - std::panicking::begin_panic::hc09d473187ffd19e
                               at /builddir/build/BUILD/rustc-1.21.0-src/src/libstd/panicking.rs:572
   6:     0x5606b3bf0ca0 - gitignore_matched_path_or_any_parents_tests::test_path_should_be_under_root::hacddd68d874637f3
                               at tests/gitignore_matched_path_or_any_parents_tests.rs:26
   7:     0x5606b3c60a51 - <F as test::FnBox<T>>::call_box::h9b336d8298e58a38
   8:     0x5606b3c9e16c - __rust_maybe_catch_panic
   9:     0x5606b3c51c2a - std::sys_common::backtrace::__rust_begin_short_backtrace::hfefcdd54aa9f9107
  10:     0x5606b3c52932 - std::panicking::try::do_call::h3b19cb6225148230
  11:     0x5606b3c9e16c - __rust_maybe_catch_panic
  12:     0x5606b3c5a609 - <F as alloc::boxed::FnBox<A>>::call_box::h8f60bdeeb9613397
  13:     0x5606b3c9617b - std::sys::imp::thread::Thread::new::thread_start::h54fcbd7022894d21
  14:     0x7fa5f76c855a - start_thread
  15:     0x7fa5f71e74fe - __clone
  16:                0x0 - <unknown>
note: Panic did not include expected string 'path is expect to be under the root'
```

---

_Comment by @BurntSushi on 2017-11-10 15:34_

The test passes for me, so you'll need to provide more details. My guess is that this is a duplicate of #448, but I don't know for sure.

---

_Label `question` added by @BurntSushi on 2017-11-10 15:34_

---

_Comment by @igor-raits on 2017-11-10 15:42_

@BurntSushi which details do you need to know? build is happening in ~/rpmbuild/BUILD/ignore-X.Y.Z with tarball from crates.io.. 

```
++ pwd
+ find /home/brain/rpmbuild/BUILD/ignore-0.3.1
/home/brain/rpmbuild/BUILD/ignore-0.3.1
/home/brain/rpmbuild/BUILD/ignore-0.3.1/COPYING
/home/brain/rpmbuild/BUILD/ignore-0.3.1/Cargo.toml.orig
/home/brain/rpmbuild/BUILD/ignore-0.3.1/Cargo.toml
/home/brain/rpmbuild/BUILD/ignore-0.3.1/LICENSE-MIT
/home/brain/rpmbuild/BUILD/ignore-0.3.1/README.md
/home/brain/rpmbuild/BUILD/ignore-0.3.1/UNLICENSE
/home/brain/rpmbuild/BUILD/ignore-0.3.1/examples
/home/brain/rpmbuild/BUILD/ignore-0.3.1/examples/walk.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/src
/home/brain/rpmbuild/BUILD/ignore-0.3.1/src/dir.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/src/gitignore.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/src/lib.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/src/overrides.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/src/pathutil.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/src/types.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/src/walk.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/tests
/home/brain/rpmbuild/BUILD/ignore-0.3.1/tests/gitignore_matched_path_or_any_parents_tests.gitignore
/home/brain/rpmbuild/BUILD/ignore-0.3.1/tests/gitignore_matched_path_or_any_parents_tests.rs
/home/brain/rpmbuild/BUILD/ignore-0.3.1/.cargo
/home/brain/rpmbuild/BUILD/ignore-0.3.1/.cargo/config
/home/brain/rpmbuild/BUILD/ignore-0.3.1/Cargo.lock
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.cargo-lock
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libignore-7317621099aeaddd.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/ignore-a86f1278e4fb5e39
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libvoid-e57cfdd068c5bb98.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/librand-cf5fbc7b9a52cf5e.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/liblazy_static-1986f2c66c639876.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libfnv-bf4beb544c5632e2.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libwalkdir-f220b08f5cc7c9c9.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libunreachable-95f7d7b097c78550.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libsame_file-40920896398c3396.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libregex-ecb81402e92bcb36.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libglobset-3b9d185bbbd13bf1.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libtempdir-3660369c541856bc.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/gitignore_matched_path_or_any_parents_tests-ccbc77e8f0de808f
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/liblog-498e75fd0d49c8e8.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libutf8_ranges-7760ac8994a8bcfb.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libthread_local-ece0e0cf82dc4cf1.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/liblibc-60d2cb9456cf318c.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libregex_syntax-529a0e20924e1438.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libcrossbeam-ff82b4a2cd3dacd9.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libmemchr-b12354caac6a55e7.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/deps/libaho_corasick-ca8ba4ed8b7ca20e.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/native
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/incremental
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-7317621099aeaddd
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-7317621099aeaddd/dep-lib-ignore-7317621099aeaddd
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-7317621099aeaddd/lib-ignore-7317621099aeaddd
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-7317621099aeaddd/lib-ignore-7317621099aeaddd.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/walkdir-f220b08f5cc7c9c9
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/walkdir-f220b08f5cc7c9c9/dep-lib-walkdir-f220b08f5cc7c9c9
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/walkdir-f220b08f5cc7c9c9/lib-walkdir-f220b08f5cc7c9c9
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/walkdir-f220b08f5cc7c9c9/lib-walkdir-f220b08f5cc7c9c9.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/same-file-40920896398c3396
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/same-file-40920896398c3396/dep-lib-same_file-40920896398c3396
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/same-file-40920896398c3396/lib-same_file-40920896398c3396
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/same-file-40920896398c3396/lib-same_file-40920896398c3396.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/log-498e75fd0d49c8e8
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/log-498e75fd0d49c8e8/dep-lib-log-498e75fd0d49c8e8
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/log-498e75fd0d49c8e8/lib-log-498e75fd0d49c8e8
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/log-498e75fd0d49c8e8/lib-log-498e75fd0d49c8e8.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/regex-ecb81402e92bcb36
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/regex-ecb81402e92bcb36/dep-lib-regex-ecb81402e92bcb36
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/regex-ecb81402e92bcb36/lib-regex-ecb81402e92bcb36
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/regex-ecb81402e92bcb36/lib-regex-ecb81402e92bcb36.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/thread_local-ece0e0cf82dc4cf1
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/thread_local-ece0e0cf82dc4cf1/dep-lib-thread_local-ece0e0cf82dc4cf1
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/thread_local-ece0e0cf82dc4cf1/lib-thread_local-ece0e0cf82dc4cf1
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/thread_local-ece0e0cf82dc4cf1/lib-thread_local-ece0e0cf82dc4cf1.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/lazy_static-1986f2c66c639876
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/lazy_static-1986f2c66c639876/dep-lib-lazy_static-1986f2c66c639876
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/lazy_static-1986f2c66c639876/lib-lazy_static-1986f2c66c639876
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/lazy_static-1986f2c66c639876/lib-lazy_static-1986f2c66c639876.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/unreachable-95f7d7b097c78550
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/unreachable-95f7d7b097c78550/dep-lib-unreachable-95f7d7b097c78550
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/unreachable-95f7d7b097c78550/lib-unreachable-95f7d7b097c78550
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/unreachable-95f7d7b097c78550/lib-unreachable-95f7d7b097c78550.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/void-e57cfdd068c5bb98
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/void-e57cfdd068c5bb98/dep-lib-void-e57cfdd068c5bb98
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/void-e57cfdd068c5bb98/lib-void-e57cfdd068c5bb98
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/void-e57cfdd068c5bb98/lib-void-e57cfdd068c5bb98.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/regex-syntax-529a0e20924e1438
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/regex-syntax-529a0e20924e1438/dep-lib-regex_syntax-529a0e20924e1438
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/regex-syntax-529a0e20924e1438/lib-regex_syntax-529a0e20924e1438
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/regex-syntax-529a0e20924e1438/lib-regex_syntax-529a0e20924e1438.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/aho-corasick-ca8ba4ed8b7ca20e
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/aho-corasick-ca8ba4ed8b7ca20e/dep-lib-aho_corasick-ca8ba4ed8b7ca20e
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/aho-corasick-ca8ba4ed8b7ca20e/lib-aho_corasick-ca8ba4ed8b7ca20e
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/aho-corasick-ca8ba4ed8b7ca20e/lib-aho_corasick-ca8ba4ed8b7ca20e.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/memchr-b12354caac6a55e7
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/memchr-b12354caac6a55e7/dep-lib-memchr-b12354caac6a55e7
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/memchr-b12354caac6a55e7/lib-memchr-b12354caac6a55e7
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/memchr-b12354caac6a55e7/lib-memchr-b12354caac6a55e7.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/libc-60d2cb9456cf318c
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/libc-60d2cb9456cf318c/dep-lib-libc-60d2cb9456cf318c
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/libc-60d2cb9456cf318c/lib-libc-60d2cb9456cf318c
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/libc-60d2cb9456cf318c/lib-libc-60d2cb9456cf318c.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/utf8-ranges-7760ac8994a8bcfb
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/utf8-ranges-7760ac8994a8bcfb/dep-lib-utf8_ranges-7760ac8994a8bcfb
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/utf8-ranges-7760ac8994a8bcfb/lib-utf8_ranges-7760ac8994a8bcfb
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/utf8-ranges-7760ac8994a8bcfb/lib-utf8_ranges-7760ac8994a8bcfb.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/crossbeam-ff82b4a2cd3dacd9
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/crossbeam-ff82b4a2cd3dacd9/dep-lib-crossbeam-ff82b4a2cd3dacd9
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/crossbeam-ff82b4a2cd3dacd9/lib-crossbeam-ff82b4a2cd3dacd9
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/crossbeam-ff82b4a2cd3dacd9/lib-crossbeam-ff82b4a2cd3dacd9.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/globset-3b9d185bbbd13bf1
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/globset-3b9d185bbbd13bf1/dep-lib-globset-3b9d185bbbd13bf1
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/globset-3b9d185bbbd13bf1/lib-globset-3b9d185bbbd13bf1
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/globset-3b9d185bbbd13bf1/lib-globset-3b9d185bbbd13bf1.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/fnv-bf4beb544c5632e2
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/fnv-bf4beb544c5632e2/dep-lib-fnv-bf4beb544c5632e2
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/fnv-bf4beb544c5632e2/lib-fnv-bf4beb544c5632e2
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/fnv-bf4beb544c5632e2/lib-fnv-bf4beb544c5632e2.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-a86f1278e4fb5e39
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-a86f1278e4fb5e39/dep-test-lib-ignore-a86f1278e4fb5e39
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-a86f1278e4fb5e39/test-lib-ignore-a86f1278e4fb5e39
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-a86f1278e4fb5e39/test-lib-ignore-a86f1278e4fb5e39.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/tempdir-3660369c541856bc
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/tempdir-3660369c541856bc/dep-lib-tempdir-3660369c541856bc
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/tempdir-3660369c541856bc/lib-tempdir-3660369c541856bc
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/tempdir-3660369c541856bc/lib-tempdir-3660369c541856bc.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/rand-cf5fbc7b9a52cf5e
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/rand-cf5fbc7b9a52cf5e/dep-lib-rand-cf5fbc7b9a52cf5e
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/rand-cf5fbc7b9a52cf5e/lib-rand-cf5fbc7b9a52cf5e
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/rand-cf5fbc7b9a52cf5e/lib-rand-cf5fbc7b9a52cf5e.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-1097c1e89fcea6b8
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-1097c1e89fcea6b8/dep-example-walk-1097c1e89fcea6b8
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-1097c1e89fcea6b8/example-walk-1097c1e89fcea6b8
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-1097c1e89fcea6b8/example-walk-1097c1e89fcea6b8.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-ccbc77e8f0de808f
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-ccbc77e8f0de808f/dep-test-integration-test-gitignore_matched_path_or_any_parents_tests-ccbc77e8f0de808f
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-ccbc77e8f0de808f/test-integration-test-gitignore_matched_path_or_any_parents_tests-ccbc77e8f0de808f
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/.fingerprint/ignore-ccbc77e8f0de808f/test-integration-test-gitignore_matched_path_or_any_parents_tests-ccbc77e8f0de808f.json
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/examples
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/examples/walk-1097c1e89fcea6b8
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/examples/walk
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/examples/walk.d
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/build
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/libignore.rlib
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/libignore.d
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/gitignore_matched_path_or_any_parents_tests-ccbc77e8f0de808f
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/ignore-a86f1278e4fb5e39
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/ignore-a86f1278e4fb5e39.d
/home/brain/rpmbuild/BUILD/ignore-0.3.1/target/release/gitignore_matched_path_or_any_parents_tests-ccbc77e8f0de808f.d
```

---

_Comment by @BurntSushi on 2017-11-10 15:44_

@ignatenkobrain Can you try the workaround? https://github.com/BurntSushi/ripgrep/issues/448#issuecomment-293960440

---

_Comment by @igor-raits on 2017-11-10 16:01_

@BurntSushi so I copied unpacked crates.io tarball to /tmp/foo and ran cargo test from there, didn't help =(

---

_Comment by @igor-raits on 2017-11-10 16:12_

@BurntSushi but don't worry, this issue doesn't block me from packaging ignore-rs ;)

---

_Comment by @BurntSushi on 2018-01-31 02:33_

I am going to close this because I can't reproduce it. I'm not sure exactly what other details would help here. If it were me that got this error, I'd probably jump straight to debugging the code since I don't know what the problem is.

---

_Closed by @BurntSushi on 2018-01-31 02:33_

---

_Comment by @igor-raits on 2018-07-29 05:45_

@BurntSushi I found the problem.

This is the code from library:

```rust
        let mut path = self.strip(path.as_ref());
        debug_assert!(
            !path.has_root(),
            "path is expect to be under the root"
);
```

Note that you use `debug_assert!()` while I' building tests with `--release` which optimize it away.

---

_Reopened by @BurntSushi on 2018-07-29 12:29_

---

_Closed by @BurntSushi on 2018-07-29 12:31_

---

_Comment by @BurntSushi on 2018-07-29 12:31_

@ignatenkobrain Ah nice catch! Reading your original error message more carefully, I now see that I should have caught that initially. Sorry about that. :-( I've updated the `debug_assert!` to what must have been intended to be an `assert!` and that seems to fix things!

---
