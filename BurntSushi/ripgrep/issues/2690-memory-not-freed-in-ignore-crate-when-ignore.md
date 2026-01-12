```yaml
number: 2690
title: Memory not freed in ignore crate when ignore flags are enabled
type: issue
state: closed
author: fe9lix
labels:
  - bug
assignees: []
created_at: 2023-12-18T18:33:36Z
updated_at: 2024-03-24T11:41:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2690
synced_at: 2026-01-12T16:13:24Z
```

# Memory not freed in ignore crate when ignore flags are enabled

---

_@fe9lix_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ignore crate 0.4.21

### How did you install ripgrep?

cargo add ignore

### What operating system are you using ripgrep on?

macOS Sonoma 14.1.1

### Describe your bug.

ignore's WalkParallel does not seem to free memory when ignore flags are enabled.

### What are the steps to reproduce the behavior?

- Run the "stress test" example project (adjust the path and timeout) and observe the memory growth over time in some process monitoring tool. I'm compiling for the ARM aarch64-apple-darwin target and running it on an M1 Macbook Pro.

- Run heap profiling tools to confirm the leaks. I used Instruments' Allocations and Leaks via `cargo instruments` on macOS (latest stable Rust toolchain) and LeakSanitizer via the the nightly toolchain and compiled for x86 (`RUSTFLAGS="-Z sanitizer=leak" cargo run --target x86_64-apple-darwin`). Visual output with stack traces is appended for both the leaking and non-leaking versions (ignore flags disabled), the text output of LeakSanitizer is attached, and the visual and text output of heaptrack (linux VM only).

[ignore-mem.zip](https://github.com/BurntSushi/ripgrep/files/13726976/ignore-mem.zip)

### What is the actual behavior?

Memory usage increases over time, unbounded – but only when the ignore flags are enabled. The memory growth increases faster when custom overrides are enabled. There is _no_ continuous memory growth when the ignore flags are disabled (regardless of whether custom overrides are enabled or not).

### What is the expected behavior?

I would expect that memory usage would be bounded and not grow over time when the threads have been joined and the WalkParallel instance is dropped in the example project. Memory would stay around a baseline level as the allocator reuses freed memory. This happens when the ignore flags are disabled. To exclude memory fragmentation as a possible cause, I've also tested this with jemalloc and observed the same behavior (with a different baseline memory usage and allocation pattern but still unbounded growth).

Since I've also tested older version of this crate and observed the same behavior, it makes me think that I'm 1) missing something obvious, or 2) misusing the crate and its API, 3) holding misconceptions about how memory is supposed to be managed in the example. If so, I'd be happy to be pointed in the right direction how to correctly use the crate for a long-running process that walks different subdirectory trees at certain points in time, while keeping memory bounded within a baseline range.

---

_Label `bug` added by @BurntSushi on 2024-01-06 17:41_

---

_Closed by @BurntSushi on 2024-01-06 17:50_

---

_Comment by @Nicolecolwell on 2024-03-24 11:24_

Ignore 

---
