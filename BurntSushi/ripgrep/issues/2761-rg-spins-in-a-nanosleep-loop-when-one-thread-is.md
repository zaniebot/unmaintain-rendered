```yaml
number: 2761
title: rg spins in a nanosleep loop when one thread is blocked on a fifo
type: issue
state: open
author: leahneukirchen
labels:
  - bug
assignees: []
created_at: 2024-03-21T00:38:43Z
updated_at: 2025-10-11T01:56:07Z
url: https://github.com/BurntSushi/ripgrep/issues/2761
synced_at: 2026-01-12T16:13:24Z
```

# rg spins in a nanosleep loop when one thread is blocked on a fifo

---

_@leahneukirchen_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)


### How did you install ripgrep?

Distribution packages of Void Linux

### What operating system are you using ripgrep on?

Void Linux x86_64 glibc

### Describe your bug.

When recursively traversing a large directory and hitting a FIFO, one thread of rg gets stuck on the FIFO (suboptimal, but okay.  Perhaps FIFO should not be opened with -r? ugrep seems to skip over fifo, GNU grep has the option of using `-D skip`).

After the whole tree is traversed, the remaining threads start to busy loop in `ignore::walk::Worker::get_work`. On a system with a bunch of CPU, this results in quite heavy syscall load across the system.

I'd at least expect the other threads to block, requiring no CPU time.

### What are the steps to reproduce the behavior?

```
mkfifo /tmp/myfifo
strace -ff rg foobar /usr/include /tmp/myfifo
```

Notice how the threads spin on `clock_nanosleep`

### What is the actual behavior?

The process keeps looping in clock_nanosleep.

I also attached a backtrace of the threads.

[bt.log](https://github.com/BurntSushi/ripgrep/files/14676178/bt.log)


### What is the expected behavior?

ripgrep should just stall on the `openat` call for the FIFO, and not do busy looping in the other threads.

---

_Comment by @BurntSushi on 2025-10-11 01:56_

This is an interesting case. I think ripgrep's parallel directory traversal assumes that all threads will always make progress toward completion in reasonable time. But this isn't always true. Even putting aside cases like fifos, consider searching two files where one is very large and one is very small. The small file will complete quickly while the larger file will take considerably longer. While the larger file is being searched, the other threads will busy wait.

So I think fixing this will require digging into the traverser to figure out how to avoid the busy waiting regardless of fifos or special devices. The fact that it even busy waits at all has always been something I didn't like. I've had trouble eliminating that due to the fact that producers are consumers and consumers are producers.

As for an option to skip devices/fifos or whatever, that probably seems like a prudent thing to provide. I think I'd rather default to reading (as grep does), but with an option (perhaps identical to grep's `-D/--devices` flag) to skip them.

---

_Label `bug` added by @BurntSushi on 2025-10-11 01:56_

---
