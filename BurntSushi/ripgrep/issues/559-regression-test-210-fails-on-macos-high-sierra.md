```yaml
number: 559
title: "Regression test #210 fails on macOS High Sierra"
type: issue
state: closed
author: jhenahan
labels:
  - bug
assignees: []
created_at: 2017-07-17T05:00:18Z
updated_at: 2017-10-21T00:51:13Z
url: https://github.com/BurntSushi/ripgrep/issues/559
synced_at: 2026-01-12T16:13:22Z
```

# Regression test #210 fails on macOS High Sierra

---

_@jhenahan_

I'm not positive as to why, but I have a feeling it may be related to [normalization-insensitivity in APFS](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html#//apple_ref/doc/uid/TP40016999-CH6-DontLinkElementID_3). If I understand the APFS docs and the test case correctly, this may actually be an issue in `std::ffi`, and if that's the case then I'll report this up the chain.

```
Stack trace:

failures:

---- regression_210 stdout ----
	thread 'regression_210' panicked at '~/src/ripgrep/target/debug/deps/ripgrep-tests/regression_210/93/foo�bar: Error { repr: Os { code: 92, message: "Illegal byte sequence" } }', tests/workdir.rs:285
stack backtrace:
   0: std::sys::imp::backtrace::tracing::imp::unwind_backtrace
   1: std::panicking::default_hook::{{closure}}
   2: std::panicking::default_hook
   3: std::panicking::rust_panic_with_hook
   4: std::panicking::begin_panic
   5: std::panicking::begin_panic_fmt
   6: integration::workdir::nice_err
   7: integration::workdir::WorkDir::create_bytes
   8: integration::workdir::WorkDir::create
   9: integration::regression_210
  10: <F as test::FnBox<T>>::call_box
  11: __rust_maybe_catch_panic
  12: std::panicking::try::do_call
  13: __rust_maybe_catch_panic
  14: <F as alloc::boxed::FnBox<A>>::call_box
  15: std::sys::imp::thread::Thread::new::thread_start
  16: _pthread_body
  17: _pthread_start


failures:
    regression_210

test result: FAILED. 136 passed; 1 failed; 0 ignored; 0 measured
```

---

_Comment by @BurntSushi on 2017-07-17 11:19_

This is the test in question:

```rust
// See: https://github.com/BurntSushi/ripgrep/issues/210
#[cfg(unix)]
#[test]
fn regression_210() {
    use std::ffi::OsStr;
    use std::os::unix::ffi::OsStrExt;

    let badutf8 = OsStr::from_bytes(&b"foo\xffbar"[..]);

    let wd = WorkDir::new("regression_210");
    let mut cmd = wd.command();
    wd.create(badutf8, "test");
    cmd.arg("-H").arg("test").arg(badutf8);

    let out = wd.output(&mut cmd);
    assert_eq!(out.stdout, b"foo\xffbar:test\n".to_vec());
}
```

This test is specifically testing a case where there is an invalid UTF-8 byte in a file path (which is a valid thing to do) and that ripgrep still happily searches said file.

The APFS docs link says this:

> APFS accepts only valid UTF-8 encoded filenames for creation

Which means this test probably doesn't make sense on a file system that specifically rejects invalid UTF-8. This also means that I don't think there is any bug in `std::ffi`, and that according to the APFS docs, this test is failing in exactly the way I'd expect.

I think the proper work-around here is to modify the test to pass if it is unable to create a file with an invalid UTF-8 name.

---

_Label `bug` added by @BurntSushi on 2017-07-17 11:19_

---

_Comment by @DanielJoyce on 2017-09-11 22:16_

Or you can add a #cfg[] param to exclude macosx so the test isn't run at all on that platform

Something like

#[cfg(all(unix,not(target_os="macos")))]

---

_Closed by @BurntSushi on 2017-10-21 00:51_

---
