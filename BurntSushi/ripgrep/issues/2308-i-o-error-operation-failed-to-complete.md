```yaml
number: 2308
title: "I/O Error: Operation Failed To Complete Synchoronously When Using lsd."
type: issue
state: closed
author: SynCROSS
labels:
  - invalid
assignees: []
created_at: 2022-09-20T07:06:58Z
updated_at: 2023-11-24T19:10:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2308
synced_at: 2026-01-12T16:13:24Z
```

# I/O Error: Operation Failed To Complete Synchoronously When Using lsd.

---

_@SynCROSS_

#### What version of ripgrep are you using?
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Using Cargo With This Command

`cargo install ripgrep`

#### What operating system are you using ripgrep on?

WINDOWS 10

uname -a: 
`MINGW64_NT-10.0-19044 DESKTOP-LMEOL8I 3.3.5-341.x86_64 2022-07-08 09:41 UTC x86_64 Msys`

#### Describe your bug.

There is an I/O error when using ripgrep with lsd.

![image](https://user-images.githubusercontent.com/51585533/191140596-563fb566-a34a-4804-9547-25bcc8c4c82c.png)

#### What are the steps to reproduce the behavior?

`lsd | rg ANY_PATTERN`

#### What is the actual behavior?

`DEBUG|grep_cli::decompress|C:\Users\Username\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-cli-0.1.6\src\decompress.rs:482: lz4: could not find executable in PATH
DEBUG|grep_cli::decompress|C:\Users\Username\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-cli-0.1.6\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|grep_cli::decompress|C:\Users\Username\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-cli-0.1.6\src\decompress.rs:482: zstd: could not find executable in PATH
DEBUG|globset|C:\Users\Username\.cargo\registry\src\github.com-1ecc6299db9ec823\globset-0.4.9\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 9 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes`

#### What is the expected behavior?

![image](https://user-images.githubusercontent.com/51585533/191190190-083817ad-8b10-4952-87f1-2447c2d79baf.png)


---

_Comment by @BurntSushi on 2022-09-20 11:41_

I'm not quite sure why you're reporting this against ripgrep. ripgrep works with `ls` but not with `lsd`, I'd first look into `lsd`.

I'll leave this open for now, but I don't use Windows and it will be quite some time before I'm able to get on my Windows machine to test this.

---

_Label `question` added by @BurntSushi on 2022-09-20 11:41_

---

_Comment by @BurntSushi on 2022-09-20 11:41_

I also don't even know what that error message means. Do you?

---

_Comment by @nathaniel-daniel on 2022-10-04 19:53_

I think the issue is https://github.com/rust-lang/rust/issues/98947?

---

_Comment by @BurntSushi on 2023-11-24 19:10_

It looks like this was unrelated to ripgrep.

---

_Closed by @BurntSushi on 2023-11-24 19:10_

---

_Label `question` removed by @BurntSushi on 2023-11-24 19:10_

---

_Label `invalid` added by @BurntSushi on 2023-11-24 19:10_

---
