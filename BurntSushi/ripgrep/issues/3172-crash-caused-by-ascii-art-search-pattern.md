```yaml
number: 3172
title: Crash caused by ASCII art search pattern
type: issue
state: closed
author: adumas
labels:
  - invalid
assignees: []
created_at: 2025-10-08T16:47:23Z
updated_at: 2025-10-08T17:29:11Z
url: https://github.com/BurntSushi/ripgrep/issues/3172
synced_at: 2026-01-12T16:13:25Z
```

# Crash caused by ASCII art search pattern

---

_@adumas_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

 rg --version
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


### How did you install ripgrep?

apt

### What operating system are you using ripgrep on?

Ubuntu 20.04

### Describe your bug.

Searching for a string from an ASCII art motd, I encountered an internal error / thread panic.

### What are the steps to reproduce the behavior?


$ rg "____|"
Seemingly anywhere


### What is the actual behavior?

```

$ RUST_BACKTRACE=full rg "____|"
thread 'main' panicked at 'internal error: entered unreachable code: expected literal or concat, got Hir { kind: Empty, info: HirInfo { bools: 1795 } }', /usr/share/cargo/registry/ripgrep-11.0.2/debian/cargo_registry/regex-1.2.1/src/exec.rs:1410:18
stack backtrace:
   0:     0x5590bb445e46 - <unknown>
   1:     0x5590bb47146c - <unknown>
   2:     0x5590bb436915 - <unknown>
   3:     0x5590bb43f641 - <unknown>
   4:     0x5590bb43f241 - <unknown>
   5:     0x5590bb43fb91 - <unknown>
   6:     0x5590bb446630 - <unknown>
   7:     0x5590bb445f74 - <unknown>
   8:     0x5590bb43f722 - <unknown>
   9:     0x5590bb20c7a1 - <unknown>
  10:     0x5590bb3a644f - <unknown>
  11:     0x5590bb3db477 - <unknown>
  12:     0x5590bb2e6643 - <unknown>
  13:     0x5590bb2e5910 - <unknown>
  14:     0x5590bb2e8781 - <unknown>
  15:     0x5590bb2472d6 - <unknown>
  16:     0x5590bb2a230d - <unknown>
  17:     0x5590bb293b83 - <unknown>
  18:     0x5590bb258059 - <unknown>
  19:     0x5590bb4349f1 - <unknown>
  20:     0x5590bb2a55f8 - <unknown>
  21:     0x7fdd8bf44083 - __libc_start_main
                               at /build/glibc-B3wQXB/glibc-2.31/csu/../csu/libc-start.c:308:16
  22:     0x5590bb20cefe - <unknown>
  23:                0x0 - <unknown>

```

### What is the expected behavior?

ripgrep shouldn't have crashed

---

_Comment by @BurntSushi on 2025-10-08 17:29_

> rg --version
> ripgrep 11.0.2
> -SIMD -AVX (compiled)
> +SIMD +AVX (runtime)

This version of ripgrep is over **6 years** old. This bug doesn't exist on the latest version.

---

_Closed by @BurntSushi on 2025-10-08 17:29_

---

_Label `invalid` added by @BurntSushi on 2025-10-08 17:29_

---
