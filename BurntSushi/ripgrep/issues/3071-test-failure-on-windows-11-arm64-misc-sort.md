```yaml
number: 3071
title: "Test Failure on Windows 11 ARM64: misc::sort_accessed Output Order Mismatch Due to Access Time Handling"
type: issue
state: closed
author: vishvanatarajan
labels:
  - rollup
assignees: []
created_at: 2025-06-21T14:59:51Z
updated_at: 2025-09-20T01:08:43Z
url: https://github.com/BurntSushi/ripgrep/issues/3071
synced_at: 2026-01-12T16:13:25Z
```

# Test Failure on Windows 11 ARM64: misc::sort_accessed Output Order Mismatch Due to Access Time Handling

---

_@vishvanatarajan_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1 (rev 4649aa9700)

Features:
- pcre2

SIMD:
- compile: +NEON
- runtime: +NEON

Note: PCRE2 is not available in this build of ripgrep.

### How did you install ripgrep?

Built from source.

### What operating system are you using ripgrep on?

OS: Microsoft Windows 11 Home (Build 10.0.26100)
Architecture: ARM64

### Describe your bug.

When running cargo test --all, the following two tests fail:

misc::sort_accessed – consistently fails on the system.
misc::sortr_accessed – fails intermittently on the system.

The failure appears to be related to differences in how file access times are handled on Windows ARM64, possibly due to NTFS behavior or platform-specific metadata handling.

### What are the steps to reproduce the behavior?

1. Clone the ripgrep repository.

2. Build the project using cargo build.

3. Run the full test suite with:
`
cargo test --all
`

### What is the actual behavior?

The test misc::sort_accessed fails due to a mismatch in expected vs actual output order. The expected output assumes a specific access time order that is not preserved on this platform.

https://gist.github.com/vishvanatarajan/a2c3301b52f11be9b98ec4a411c62761

### What is the expected behavior?

All the tests must pass.

---

_Comment by @vishvanatarajan on 2025-06-21 15:01_

I would be happy to try a fix for this, however, I have a very basic (almost non-existent) knowledge of Rust and the ripgrep codebase, I would highly appreciate any support I can get.

---

_Comment by @BurntSushi on 2025-06-21 16:49_

The tests are here:

https://github.com/BurntSushi/ripgrep/blob/cbc598f245f3c157a872b69102653e2e349b6d92/tests/misc.rs#L1114-L1130

I would play with the sleep times in the setup code for those tests:

https://github.com/BurntSushi/ripgrep/blob/cbc598f245f3c157a872b69102653e2e349b6d92/tests/misc.rs#L1093-L1106

Maybe Windows is using a clock that lacks precision to the nearest hundredth of a millisecond? Seems kinda crazy if so, but that's where I would start.

---

_Comment by @vishvanatarajan on 2025-06-21 17:25_

playing with the sleep duration does make a difference.

In general, I am able to get all tests passing reasonably consistently with 400ms of sleep. Any lower and it does not pass consistently. However, this could depend on the system load and hardware capabilities.

It does look like Windows lacks precision in updating the metadata of files in the file system. Does it make sense to set the sleep duration to 1000ms in an attempt to ensure that the test is not flaky depending on the system load or hardware capabilites?

P.S - It could also be that Windows/ARM64 lacks precision on file metadata updates in the nearest hundreth of a millisecond. I do not have a x64 Windows system to test this on though.

---

_Comment by @BurntSushi on 2025-06-21 17:47_

I'm not especially enthused about making the test take ten times longer just because of Windows bullshit. What I would be okay with doing is increasing it to 1000md on Windows/arm64 though. I'm on mobile, but something like `#[cfg(all(windows, target_arch = "aarch64"))]`.

---

_Comment by @vishvanatarajan on 2025-06-21 19:40_

I understand of course!

```rust
fn sort_setup(dir: Dir) {
    use std::{thread::sleep, time::Duration};
    
    // As reported in https://github.com/BurntSushi/ripgrep/issues/3071
    // this test fails if sufficient delay is not given on Windows/Aarch64.
    let delay = if cfg!(all(windows, target_arch = "aarch64")) {
        Duration::from_millis(1000)
    } else {
        Duration::from_millis(100)
    };

    let sub_dir = dir.path().join("dir");
    dir.create("a", "test");
    sleep(delay);
    dir.create_dir(&sub_dir);
    sleep(delay);
    dir.create(sub_dir.join("c"), "test");
    sleep(delay);
    dir.create("b", "test");
    sleep(delay);
    dir.create(sub_dir.join("d"), "test");
}
```

Would this be something acceptable? I do believe cfg! is a compile-time conditional macro and this should not cause any runtime overheads.

---

_Comment by @BurntSushi on 2025-06-21 19:48_

Looks okay 

---

_Label `rollup` added by @BurntSushi on 2025-08-19 21:21_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
