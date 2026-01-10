---
number: 12769
title: "`uv pip insall` on Windows causes \"thread '<unknown>' has overflowed its stack\""
type: issue
state: closed
author: notatallshaw
labels:
  - bug
  - windows
assignees: []
created_at: 2025-04-09T02:28:37Z
updated_at: 2025-04-16T07:27:47Z
url: https://github.com/astral-sh/uv/issues/12769
synced_at: 2026-01-10T01:25:24Z
---

# `uv pip insall` on Windows causes "thread '<unknown>' has overflowed its stack"

---

_Issue opened by @notatallshaw on 2025-04-09 02:28_

### Summary

I was benchmarking the following scenario with pip vs. uv: https://discuss.python.org/t/pep-777-how-to-re-invent-the-wheel/67484/238 and I end up getting: "thread '<unknown>' has overflowed its stack" and `uv pip` fails to install the packages.

#### Steps to set up

There may be a simpler set up but this is what I found:

1. Download https://github.com/winpython/winpython/releases/download/13.1.202502222/build_3.13._.0slim__of_23-02-2025_at_10_22.txt.packages_versions.txt as `requirements.txt`
2. `uv venv -p 3.13`
3. `.venv\Scripts\activate`
4. `uv pip install pip`
5. `pip download -r .\requirements.txt -d packages`
6. `uv cache clean`

####  Command that fails

```
uv pip install -r .\requirements.txt --no-index --find-links .\packages\
```

####  Output

```
thread '<unknown>' has overflowed its stack
```

"-vvv" logs: https://gist.github.com/notatallshaw/404c6444703e5e2a14e3bd5b9994990a

### Platform

Windows 10

### Version

uv 0.6.13 (a0f5c7250 2025-04-07)

### Python version

Python 3.13.2

---

_Label `bug` added by @notatallshaw on 2025-04-09 02:28_

---

_Comment by @zanieb on 2025-04-09 03:21_

Related

 - https://github.com/astral-sh/uv/issues/11837

---

_Comment by @notatallshaw on 2025-04-09 03:30_

Ahah, reading my own comments about not being able to reproduce. Well this one is consistently reproducible for me. 

---

_Assigned to @konstin by @zanieb on 2025-04-09 03:35_

---

_Comment by @notatallshaw on 2025-04-09 03:47_

I'll take a look at the profile comments @konstin left in the other thread when I have some time (probably tomorrow or the day after).

---

_Assigned to @Gankra by @Gankra on 2025-04-09 13:49_

---

_Comment by @notatallshaw on 2025-04-10 02:55_

I tried following the instructions in https://github.com/astral-sh/uv/issues/11837#issuecomment-2698312043

And found I get it happens in the released version of uv:

```
> uv pip install -r .\requirements.txt --no-index --find-links .\packages\
Using Python 3.13.2 environment at: .venv3
Resolved 491 packages in 1.47s
      Built intervaltree==3.0.2
⠼ Preparing packages... (53/490)
thread '<unknown>' has overflowed its stack
```

But not the profiling version of uv:

```
> target\profiling\uv.exe pip install -r .\requirements.txt --no-index --find-links .\packages\                   
Resolved 491 packages in 1.48s
      Built intervaltree==3.0.2
Prepared 490 packages in 2m 15s
Installed 491 packages in 12.67s
 + absl-py==2.0.0
 + adbc-driver-manager==1.3.0
 + aiofiles==23.2.1
 + aiohappyeyeballs==2.4.4

...

 + yt-dlp==2023.7.6
 + zict==3.0.0
 + zipp==3.21.0
 + zstandard==0.23.0
```

I also notice the profiling version of uv is significantly slower than the release version, if that contributes to why this doesn't happen under that mode.

---

_Comment by @konstin on 2025-04-10 08:11_

> I also notice the profiling version of uv is significantly slower than the release version, if that contributes to why this doesn't happen under that mode.

Huh that's bad, can you try removing the line <https://github.com/astral-sh/uv/blob/e0b4dfe923ee807ed2e6e9e16e101a652977ef42/Cargo.toml#L275> when compiling for profiling? It should only be a few percent at most of a difference though, the only other difference we have remaining is that the profiling binary contains debuginfo.

Another option to narrow it down would be testing if the error occurs with `--no-build`, too (after removing intervaltree, pypandoc and pywin32, at least those are the source dists I see).

---

I've done some testing on my linux machine, but when removing the pywin* deps I get ~1.1s for uv pip and ~24s for pip. Could it be that the benchmark is dominated by build time or IO?

---

_Comment by @notatallshaw on 2025-04-10 13:11_

I found the big performance difference was created by Windows Defender, turning that off the performance is indeed about the same, but I still face the issue that this happens in the release build but not the profile version:

```
> uv pip install -r .\requirements.txt --no-index --find-links .\packages\ --no-cache
⠸ Preparing packages... (1/490)
thread '<unknown>' has overflowed its stack
```

```
> target\profiling\uv.exe  pip install -r .\requirements.txt --no-index --find-links .\packages\ --no-cache
Resolved 491 packages in 1.15s
      Built intervaltree==3.0.2
Prepared 490 packages in 8.86s
Installed 491 packages in 14.34s
 + absl-py==2.0.0
 + adbc-driver-manager==1.3.0

...

 + zipp==3.21.0
 + zstandard==0.23.0
```



> Huh that's bad, can you try removing the line
> 
> [uv/Cargo.toml](https://github.com/astral-sh/uv/blob/e0b4dfe923ee807ed2e6e9e16e101a652977ef42/Cargo.toml#L275)
> 
> Line 275 in [e0b4dfe](/astral-sh/uv/commit/e0b4dfe923ee807ed2e6e9e16e101a652977ef42)

Tried, didn't help:

```
> target\profiling\uv.exe  pip install -r .\requirements.txt --no-index --find-links .\packages\ --no-cache
Resolved 491 packages in 1.12s
      Built intervaltree==3.0.2
Prepared 490 packages in 8.84s
Installed 491 packages in 14.44s
 + absl-py==2.0.0
 + adbc-driver-manager==1.3.0

...

 + zipp==3.21.0
 + zstandard==0.23.0
```




> Another option to narrow it down would be testing if the error occurs with `--no-build`, too (after removing intervaltree, pypandoc and pywin32, at least those are the source dists I see).

I only had to remove spyder and intervaltree on Windows to get `--no-build` to work:

```
> uv pip install -r .\requirements.txt --no-index --find-links .\packages\ --no-cache --no-build
Resolved 489 packages in 147ms
⠋ Preparing packages... (5/489)
thread '<unknown>' has overflowed its stack
```

But I still don't get this error under the profiling version of uv (this is with `lto = false` still removed if that makes a difference):

```
> target\profiling\uv.exe  pip install -r .\requirements.txt --no-index --find-links .\packages\ --no-cache --no-build
Resolved 489 packages in 169ms
Prepared 489 packages in 8.39s
Installed 489 packages in 13.53s
 + absl-py==2.0.0
 + adbc-driver-manager==1.3.0
 + aiofiles==23.2.1

...

 + zipp==3.21.0
 + zstandard==0.23.0
```

> Could it be that the benchmark is dominated by build time or IO?

I think this use case is dominated by IO, there are nearly 500 wheels that unzip to ~2 GB of packages.

---

_Comment by @konstin on 2025-04-10 14:04_

Can you try if setting `RUST_MIN_STACK` to something a lot higher then the current default (4MB), e.g. `8388608`, fixes the problem?

---

_Comment by @Gankra on 2025-04-10 14:09_

The default is actually back down to 2MB, we just give the faux-main-thread in particular a min of 4MB -- so even RUST_MIN_STACK=4000000 will be a ~doubling of everything but that one. Because the thread is called "unknown" we know it's not the faux-main-thread ("main2") causing problems.

---

_Comment by @konstin on 2025-04-10 14:43_

From profiling, we got our main thread, the rayon threads and (in this case one) tokio worker thread

![Image](https://github.com/user-attachments/assets/a2d7b77a-9c94-4136-ab7a-140965045d96)

---

_Label `windows` added by @konstin on 2025-04-10 14:43_

---

_Comment by @notatallshaw on 2025-04-10 16:02_

I deleted my previous comment as I realized I was in powershell rather than cmd.

Setting `RUST_MIN_STACK=4000000` stops the issue from happening:

```
> uv pip install -r .\requirements.txt --no-index --find-links .\packages\ --no-cache --no-build
Resolved 489 packages in 147ms
⠋ Preparing packages... (5/489)
thread '<unknown>' has overflowed its stack

> set RUST_MIN_STACK=4000000

> uv pip install -r .\requirements.txt --no-index --find-links .\packages\ --no-cache --no-build
Resolved 489 packages in 154ms
Prepared 489 packages in 8.82s
Installed 489 packages in 13.68s
 + absl-py==2.0.0
 + adbc-driver-manager==1.3.0

 ...

 + zipp==3.21.0
 + zstandard==0.23.0

```

---

_Comment by @konstin on 2025-04-11 15:32_

The stacktrace from windbg: https://gist.github.com/konstin/ff30565563675ca8a4b55e4258a7b511, with the top rust frame being https://github.com/trifectatechfoundation/zlib-rs/blob/v0.5.0/zlib-rs/src/inflate.rs#L2278, and the only uv call being

https://github.com/astral-sh/uv/blob/50de4644252a101d0f11f7ca8ff1767642b8d38f/crates/uv-extract/src/sync.rs#L61

```
# Child-SP          RetAddr               Call Site
00 00000071`45706fe0 00007ffe`8d2b018c     ntdll!RtlAllocateHeap+0x994
01 00000071`45707100 00007ffe`8d2737b3     ntdll!RtlDebugAllocateHeap+0xc8
02 00000071`45707160 00007ffe`8d207dbe     ntdll!RtlpAllocateHeap+0x2513
03 00000071`457073d0 00007ffe`8d242b99     ntdll!RtlpAllocateNTHeapInternal+0x32e
04 00000071`45707460 00007ff7`5d3ce63c     ntdll!RtlAllocateHeap+0x999
05 (Inline Function) --------`--------     uv!zlib_rs::inflate::inflate+0x3459 [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\zlib-rs-0.5.0\src\inflate.rs @ 2278] 
06 (Inline Function) --------`--------     uv!libz_rs_sys::inflate+0x34ad [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\libz-rs-sys-0.5.0\src\lib.rs @ 354] 
07 00000071`45707580 00007ff7`5f2e22eb     uv!flate2::ffi::c::impl$10::decompress+0x350c [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\flate2-1.1.1\src\ffi\c.rs @ 270] 
08 (Inline Function) --------`--------     uv!flate2::mem::Decompress::decompress+0x1c [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\flate2-1.1.1\src\mem.rs @ 452] 
09 (Inline Function) --------`--------     uv!flate2::zio::impl$1::run+0x1c [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\flate2-1.1.1\src\zio.rs @ 77] 
0a (Inline Function) --------`--------     uv!flate2::zio::read+0x4b [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\flate2-1.1.1\src\zio.rs @ 140] 
0b (Inline Function) --------`--------     uv!flate2::deflate::bufread::impl$6::read+0x68 [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\flate2-1.1.1\src\deflate\bufread.rs @ 238] 

[...]

f8 (Inline Function) --------`--------     uv!rayon_core::registry::impl$6::in_worker_cold::closure$0::closure$0+0x2c [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs @ 522] 
f9 (Inline Function) --------`--------     uv!rayon_core::job::impl$9::call::closure$0+0x2c [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\job.rs @ 218] 
fa (Inline Function) --------`--------     uv!core::panic::unwind_safe::impl$25::call_once+0x2c [C:\Users\Konsti\.rustup\toolchains\1.86-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\panic\unwind_safe.rs @ 272] 
fb (Inline Function) --------`--------     uv!std::panicking::try::do_call+0x34 [C:\Users\Konsti\.rustup\toolchains\1.86-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs @ 587] 
fc (Inline Function) --------`--------     uv!std::panicking::try+0x34 [C:\Users\Konsti\.rustup\toolchains\1.86-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs @ 550] 
fd (Inline Function) --------`--------     uv!std::panic::catch_unwind+0x34 [C:\Users\Konsti\.rustup\toolchains\1.86-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs @ 358] 
fe (Inline Function) --------`--------     uv!rayon_core::unwind::halt_unwinding+0x34 [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\unwind.rs @ 17] 
ff (Inline Function) --------`--------     uv!enum2$<rayon_core::job::JobResult<tuple$<tuple$<>,tuple$<> > > >::call+0x34 [C:\Users\Konsti\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\job.rs @ 218] 
```

We can set the higher default stack size for rayon, too.

---

_Referenced in [trifectatechfoundation/zlib-rs#340](../../trifectatechfoundation/zlib-rs/issues/340.md) on 2025-04-11 15:47_

---

_Referenced in [astral-sh/uv#12839](../../astral-sh/uv/pulls/12839.md) on 2025-04-11 16:05_

---

_Comment by @konstin on 2025-04-14 09:37_

If you have the chance, could you test #12839?

---

_Comment by @notatallshaw on 2025-04-14 12:33_

I'll try and test it this evening.

---

_Comment by @konstin on 2025-04-14 12:34_

fwiw it makes the error go away on my machine, so it'll go into the next release.

---

_Comment by @Gankra on 2025-04-14 13:22_

The bigger stacks solution is what we should do, but I'm now reasonably sure we're hitting this Fundamental Issue With Rayon where the work-stealing system can result in a thread piling up tons of tasks as every task it steals blocks. The stack ends up looking like a bunch of recursive rayon calls (each catch_unwind is another stolen rayon task) which eventually blows the stack.

(Any kind of limiter on this effect would theoretically be liable to unecessarily deadlock if your tasks have enough dependencies on eachother)

* https://github.com/rayon-rs/rayon/issues/854

---

_Referenced in [rayon-rs/rayon#854](../../rayon-rs/rayon/issues/854.md) on 2025-04-14 15:01_

---

_Comment by @notatallshaw on 2025-04-16 01:20_

> If you have the chance, could you test [#12839](https://github.com/astral-sh/uv/pull/12839)?

I tested your current branch and was unable to reproduce the error, whereas I can reproduce the scenario building against main.

---

_Closed by @konstin on 2025-04-16 07:27_

---

_Referenced in [astral-sh/uv#11837](../../astral-sh/uv/issues/11837.md) on 2025-04-16 14:43_

---
