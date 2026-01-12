```yaml
number: 448
title: the integration test suite depends too much on the contents of the file system
type: issue
state: closed
author: misery
labels:
  - bug
  - question
assignees: []
created_at: 2017-04-13T16:49:20Z
updated_at: 2018-07-29T14:41:04Z
url: https://github.com/BurntSushi/ripgrep/issues/448
synced_at: 2026-01-12T16:13:22Z
```

# the integration test suite depends too much on the contents of the file system

---

_@misery_

Hi there,

I added a ripgrep package to AlpineLinux. It seems it works without a problem. But the unit test "regression_25" fails.

```
failures:

---- regression_25 stdout ----
        thread 'regression_25' panicked at '

==========
command failed but expected success!

command: "/home/andre/aports/testing/ripgrep/src/ripgrep-0.5.1/target/debug/deps/../rg" "test" "."
cwd: /home/andre/aports/testing/ripgrep/src/ripgrep-0.5.1/target/debug/deps/ripgrep-tests/regression_25/90

status: exit code: 1

stdout: 

stderr: No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.


==========
', tests/workdir.rs:227
stack backtrace:
   1:     0x563e982dbfcc - std::sys::imp::backtrace::tracing::imp::write::hcc40e709c47b6024
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/sys/unix/backtrace/tracing/gcc_s.rs:42
   2:     0x563e982e0e7e - std::panicking::default_hook::{{closure}}::ha5b032273abc9158
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panicking.rs:351
   3:     0x563e982e0a13 - std::panicking::default_hook::h4aae1b7cae7d5632
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panicking.rs:361
   4:     0x563e982e131b - std::panicking::rust_panic_with_hook::ha732da68a0d51d4b
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panicking.rs:555
   5:     0x563e982e1164 - std::panicking::begin_panic::h0d7e1ef9bd5b6e18
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panicking.rs:517
   6:     0x563e982e10d9 - std::panicking::begin_panic_fmt::h5592b8b8b8c0df74
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panicking.rs:501
   7:     0x563e9827245a - integration::workdir::WorkDir::expect_success::he46ad897a51e9727
                        at /home/andre/aports/testing/ripgrep/src/ripgrep-0.5.1/tests/workdir.rs:227
   8:     0x563e982718f3 - integration::workdir::WorkDir::output::h1a775c52f50fcd3b
                        at /home/andre/aports/testing/ripgrep/src/ripgrep-0.5.1/tests/workdir.rs:185
   9:     0x563e9827148f - integration::workdir::WorkDir::stdout::hf2dcb74907becd28
                        at /home/andre/aports/testing/ripgrep/src/ripgrep-0.5.1/tests/workdir.rs:172
  10:     0x563e98288393 - integration::regression_25::{{closure}}::hdb2dfaeb1dc01182
                        at /home/andre/aports/testing/ripgrep/src/ripgrep-0.5.1/tests/tests.rs:720
  11:     0x563e982881a7 - integration::regression_25::h041438bf4d696525
                        at /home/andre/aports/testing/ripgrep/src/ripgrep-0.5.1/tests/tests.rs:720
  12:     0x563e982aadde - <F as test::FnBox<T>>::call_box::hf309100b6cb837e1
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libtest/lib.rs:1366
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libtest/lib.rs:140
  13:     0x563e982e7e7a - __rust_maybe_catch_panic
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libpanic_unwind/lib.rs:98
  14:     0x563e9829f0aa - std::panicking::try::do_call::h9d29f4b654939df3
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panicking.rs:436
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panic.rs:361
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libtest/lib.rs:1311
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panic.rs:296
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panicking.rs:460
  15:     0x563e982e7e7a - __rust_maybe_catch_panic
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libpanic_unwind/lib.rs:98
  16:     0x563e982a5d96 - <F as alloc::boxed::FnBox<A>>::call_box::h0b04066a85c3412c
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panicking.rs:436
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/panic.rs:361
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/thread/mod.rs:357
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/liballoc/boxed.rs:614
  17:     0x563e982e0454 - std::sys::imp::thread::Thread::new::thread_start::he98d7185c4a314b9
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/liballoc/boxed.rs:624
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/sys_common/thread.rs:21
                        at /home/buildozer/aports/testing/rust/src/rustc-1.16.0-src/src/libstd/sys/unix/thread.rs:84


failures:
    regression_25

test result: FAILED. 124 passed; 1 failed; 0 ignored; 0 measured
```

---

_Comment by @BurntSushi on 2017-04-13 16:59_

ripgrep's CI includes running tests with MUSL, I don't think that's an issue.

A problem with the current test suite is that it can be influenced by other `.ignore` or `.gitignore` files on your system. I'd recommend to try running the test suite in `/tmp` just to see if the error still occurs. If it doesn't, then my guess is that there's probably a `.gitignore` or `.ignore` somewhere along `/home/andre/aports/testing/` that's messing with the tests.

---

_Comment by @misery on 2017-04-13 17:07_

Ah, thank you! I tried it in /tmp/ (.gitignore was the problem) and all unit tests passed.
The problem is that Travis-CI will checkout the Alpine portstree (git) and run the build / tests in that tree.

Is there something that I can add to the "test" target to have a "clean test environment"?

---

_Comment by @BurntSushi on 2017-04-13 17:28_

I don't really have any work arounds for you. If they existed, I'd use them myself. I haven't given much thought to the matter because it hasn't really been an issue. Basically, the integration tests operate on the file system, and if the file system contains something already that will impact the tests, then there's really no way to prevent that without some other means of isolation. e.g., a chroot or running the tests in a different directory.

---

_Comment by @misery on 2017-04-13 22:10_

Ok, I found a "dirty" work-around for the conflict.

```
diff --git a/tests/tests.rs b/tests/tests.rs
index 578dbcb..d456fab 100644
--- a/tests/tests.rs
+++ b/tests/tests.rs
@@ -721,6 +721,7 @@ clean!(regression_25, "test", ".", |wd: WorkDir, mut cmd: Command| {
     wd.create(".gitignore", "/llvm/");
     wd.create_dir("src/llvm");
     wd.create("src/llvm/foo", "test");
+    wd.create("../.gitignore", "!*");
 
     let lines: String = wd.stdout(&mut cmd);
     let expected = path("src/llvm/foo:test\n");
```

By the way... thank you very much for ripgrep!!!! :-)


---

_Comment by @BurntSushi on 2017-04-13 22:40_

@misery LOL. That's awesome. Nice hack. I probably can't put that in the test suite proper, but thank you for filing this issue. This is a bug I'd like to fix more generally, but it needs more thought.

---

_Renamed from "test: regression_25 failed on Alpine (musl)" to "the integration test suite depends too much on the contents of the file system" by @BurntSushi on 2017-04-13 22:40_

---

_Label `bug` added by @BurntSushi on 2017-04-13 22:40_

---

_Label `question` added by @BurntSushi on 2017-04-13 22:40_

---

_Closed by @BurntSushi on 2018-07-29 14:41_

---
