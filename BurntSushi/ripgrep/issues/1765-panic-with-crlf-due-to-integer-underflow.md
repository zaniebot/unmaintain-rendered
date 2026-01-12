```yaml
number: 1765
title: Panic with --crlf due to integer underflow
type: issue
state: closed
author: whatisaphone
labels:
  - bug
  - rollup
assignees: []
created_at: 2020-12-21T13:59:20Z
updated_at: 2021-06-01T01:51:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1765
synced_at: 2026-01-12T16:13:24Z
```

# Panic with --crlf due to integer underflow

---

_@whatisaphone_

#### What version of ripgrep are you using?

ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

scoop install ripgrep

#### What operating system are you using ripgrep on?

Windows 10 Build 19041

#### Describe your bug.

With some regexes, combining --crlf with input that contains unix line endings causes a panic.

#### What are the steps to reproduce the behavior?

Minimal repro:

```sh
echo '' | rg --crlf x?
```

Don't forget that `echo ''` writes a line terminator:

```
echo '' | xxd
00000000: 0a                                       .
```

#### What is the actual behavior?

```
echo '' | RUST_BACKTRACE=full rg --debug --crlf x?
```

```
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 3 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
thread 'main' panicked at 'index out of bounds: the len is 1 but the index is 18446744073709551615', D:\a\ripgrep\ripgrep\crates\printer\src\standard.rs:1476:38
```

<details>
<summary>You'll be disappointed with the stack trace, but I'm including it anyway, as requested</summary>

```
stack backtrace:
   0:     0x7ff61f25899e - <unknown>
   1:     0x7ff61f276f5c - <unknown>
   2:     0x7ff61f2548ac - <unknown>
   3:     0x7ff61f25bcbb - <unknown>
   4:     0x7ff61f25b908 - <unknown>
   5:     0x7ff61f25c496 - <unknown>
   6:     0x7ff61f25c01f - <unknown>
   7:     0x7ff61f275820 - <unknown>
   8:     0x7ff61f2757e7 - <unknown>
   9:     0x7ff61f030748 - <unknown>
  10:     0x7ff61f01f2c3 - <unknown>
  11:     0x7ff61f03db67 - <unknown>
  12:     0x7ff61f048c3c - <unknown>
  13:     0x7ff61f0109c5 - <unknown>
  14:     0x7ff61efad5b2 - <unknown>
  15:     0x7ff61ef91a57 - <unknown>
  16:     0x7ff61efc3ac4 - <unknown>
  17:     0x7ff61f0ad863 - <unknown>
  18:     0x7ff61f0a9d80 - <unknown>
  19:     0x7ff61eff8f66 - <unknown>
  20:     0x7ff61f25c786 - <unknown>
  21:     0x7ff61f0b1fc7 - <unknown>
  22:     0x7ff61f2c3920 - <unknown>
  23:     0x7ffc0fcb7034 - BaseThreadInitThunk
  24:     0x7ffc1015cec1 - RtlUserThreadStart
1:
```

</details>

#### What is the expected behavior?

ripgrep should not panic.

---

_Label `bug` added by @BurntSushi on 2020-12-21 14:13_

---

_Comment by @BurntSushi on 2020-12-21 14:16_

Thanks! Yeah, I stripped debug info from ripgrep binaries, which means stack traces are essentially useless from the release binaries. I should probably remove that from the issue template or request folks to compile ripgrep themselves with debug info. But in this case, the reproduction was simple enough:

```
$ echo '' | rg --crlf 'x?'
thread 'main' panicked at 'index out of bounds: the len is 1 but the index is 18446744073709551615', /home/agallant/rust/ripgrep/crates/printer/src/standard.rs:1476:38
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Looks like it's just a simple bug where I assumed `end - 1` was always valid:

https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/printer/src/standard.rs#L1471-L1480

---

_Label `rollup` added by @BurntSushi on 2021-05-31 01:39_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
