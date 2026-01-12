```yaml
number: 873
title: Segmentation fault on basic command
type: issue
state: closed
author: atouminet
labels:
  - question
assignees: []
created_at: 2018-04-02T02:14:23Z
updated_at: 2018-04-23T23:43:24Z
url: https://github.com/BurntSushi/ripgrep/issues/873
synced_at: 2026-01-12T16:13:22Z
```

# Segmentation fault on basic command

---

_@atouminet_

#### What version of ripgrep are you using?
ripgrep 0.8.1
-SIMD -AVX

#### What operating system are you using ripgrep on?
Archlinux

#### If this is a bug, what are the steps to reproduce the behavior?

Execute rg as super user on root directory.
Example:
```shell
cd /
rg azefdz
```

This produces the following output
```shell
thread '<unnamed>' panicked at 'slice index starts at 234 but ends at 3', src/libcore/slice/mod.rs:756:5
note: Run with `RUST_BACKTRACE=1` for a backtrace.
[1]    3419 segmentation fault (core dumped)  rg azefdz
```

It segfaults with any string though. 

---

_Comment by @BurntSushi on 2018-04-02 10:58_

Could you please follow the instructions in the error message you got to get a backtrace?

If the backtrace doesn't contain names, then please checkout the ripgrep repo and build it from source by running `cargo build --release`. Then retry via the binary at `target/release/rg`. This binary should have debug symbols enabled, which will give a better backtrace.

I can't reproduce the problem, so you'll have to debug this. Playing with flags like `--no-mmap` and `--threads 1` might help, and running ripgrep under `strace` might help too.

Note that in general, running a privileged process that reads every file on disk, including those found in `/proc` and `/sys`, is probably a bad idea. On my system, ripgrep makes its way through part of the system, but ends up hanging. `grep` behaves the same. If I ignore the `/proc` and `/sys` directories, then both ripgrep and grep behave as expected.

---

_Label `question` added by @BurntSushi on 2018-04-02 11:02_

---

_Comment by @BurntSushi on 2018-04-02 11:02_

And please show the output with the `--debug` flag. The issue template specifically requests this for a reason.

---

_Comment by @BurntSushi on 2018-04-23 23:43_

I can't reproduce this and the bug report doesn't provide enough information to make this remotely actionable.

---

_Closed by @BurntSushi on 2018-04-23 23:43_

---
