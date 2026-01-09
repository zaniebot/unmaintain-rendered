---
number: 713
title: Compliation error with Clap 2.16.2 and Rust 
type: issue
state: closed
author: disconsented
labels: []
assignees: []
created_at: 2016-10-28T03:05:17Z
updated_at: 2018-08-02T03:29:55Z
url: https://github.com/clap-rs/clap/issues/713
synced_at: 2026-01-07T13:12:19-06:00
---

# Compliation error with Clap 2.16.2 and Rust 

---

_Issue opened by @disconsented on 2016-10-28 03:05_

Similar to #663 however this is running with latest stable (1.12.1) on Windows

``` powershell
D:\Git\x>cargo build
   Compiling clap v2.16.2
C:\Users\jpmke\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-2.16.2\src\errors.rs:880:35: 880:46 error: no method named `description` found for type `std::fmt::Error` in the current scope
C:\Users\jpmke\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-2.16.2\src\errors.rs:880         Error::with_description(e.description(), ErrorKind::Format)
                                                                                                                               ^~~~~~~~~~~
error: aborting due to previous error
error: Could not compile "clap".
```

``` powershell
PS C:\WINDOWS\system32> rustup show
Default host: x86_64-pc-windows-gnu

installed toolchains
--------------------

stable-x86_64-pc-windows-gnu (default)
beta-x86_64-pc-windows-gnu

active toolchain
----------------

stable-x86_64-pc-windows-gnu (default)
rustc 1.12.1 (d4f39402a 2016-10-19)
```

Any ideas on how to resolve this?


---

_Comment by @stianeklund on 2016-10-28 12:09_

I'm also having the same issue on current nightly (as well as stable):
nightly-x86_64-pc-windows-msvc (default)
rustc 1.14.0-nightly (c59cb71d9 2016-10-26)


---

_Comment by @kbknapp on 2016-10-29 15:04_

Very strange! Once I test this on a windows box I'll post back here with my results. Thanks for filing this!


---

_Comment by @kbknapp on 2016-10-29 21:26_

I'm not able to reproduce this. I just tried on Windows 10 using all of the below:

`clap` 
- v2.16.3

Rust
- rustc 1.14.0-nightly (3f4408347 2016-10-27)
  -  `nightly-x86_64-pc-windows-gnu`
  -  `nightly-x86_64-pc-windows-msvc`
- rustc 1.12.1 (d4f39402a 2016-10-19) 
  - `stable-x86_64-pc-windows-gnu`
  - `stable-x86_64-pc-windows-msvc`

Could y'all post a link to the source that's failing and maybe I can find something there?


---

_Comment by @disconsented on 2016-10-30 02:25_

I think I have figured out the issue, for my particular setup the version of Cargo I had an older version than what my IDE was using.. Works now so thanks for your time :)
cargo 0.11.0-nightly (259324c 2016-05-20)
cargo 0.13.0-nightly (109cb7c 2016-08-19)


---

_Closed by @disconsented on 2016-10-30 02:25_

---
