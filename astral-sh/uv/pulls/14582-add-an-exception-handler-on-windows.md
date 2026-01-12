```yaml
number: 14582
title: Add an exception handler on Windows
type: pull_request
state: merged
author: geofft
labels:
  - windows
assignees: []
merged: true
base: main
head: windows-exception-handler
created_at: 2025-07-13T02:49:37Z
updated_at: 2025-07-14T23:27:13Z
url: https://github.com/astral-sh/uv/pull/14582
synced_at: 2026-01-12T16:11:17Z
```

# Add an exception handler on Windows

---

_@geofft_

We've seen a few cases of uv.exe exiting with an exception code as its exit status and no user-visible output (#14563 in the field, and #13812 in CI). It seems that recent versions of Windows no longer show dialog boxes on access violations (what UNIX calls segfaults) or similar errors. Something is probably sent to Windows Error Reporting, and we can maybe sign up to get the crashes from Microsoft, but the user experience of seeing uv exit with no output is poor, both for end users and during development. While it's possible to opt out of this behavior or set up a debugger, this isn't the default configuration. (See https://superuser.com/q/1246626 for some pointers.)

In order to get some output on a crash, we need to install our own default handler for unhandled exceptions (or call all our code inside a Structured Exception Handling __try/__catch block, which is complicated on Rust). This is the moral equivalent of a segfault handler on Windows; the kernel creates a new stack frame and passes arguments to it with some processor state.

This commit adds a relatively simple exception handler that leans on Rust's own backtrace implementation and also displays some minimal information from the exception itself. This should be enough info to communicate that something went wrong and let us collect enough information to attempt to debug. There are also a handful of (non-Rust) open-source libraries for this like Breakpad and Crashpad (both from Google) and crashrpt.

The approach here, of using SetUnhandledExceptionFilter, seems to be the standard one taken by other such libraries. Crashpad also seems to try to use a newer mechanism for an out-of-tree DLL to report the crash: https://issues.chromium.org/issues/42310037
If we have serious problems with memory corruption, it might be worth adopting some third-party library that has already implemented this approach. (In general, the docs of other crash reporting libraries are worth skimming to understand how these things ought to work.)

---

_Review requested from @zanieb by @geofft on 2025-07-13 02:49_

---

_Label `windows` added by @geofft on 2025-07-13 02:49_

---

_Comment by @geofft on 2025-07-13 02:59_

I tested this by forcing a segfault:
```diff
diff --git a/crates/uv-client/src/base_client.rs b/crates/uv-client/src/base_client.rs
index 9ddc30e75..45fe0e4e5 100644
--- a/crates/uv-client/src/base_client.rs
+++ b/crates/uv-client/src/base_client.rs
@@ -266,6 +266,7 @@ impl<'a> BaseClientBuilder<'a> {
     }

     pub fn build(&self) -> BaseClient {
+        unsafe {(1 as *mut i32).write(1);}
         // Create user agent.
         let mut user_agent_string = format!("uv/{}", version());

```

Without this patch, it exits quietly:
```
C:\Users\Administrator\uv>target\debug\uv run python

C:\Users\Administrator\uv>echo %errorlevel%
-1073741819
```

With the patch I get a relatively short error, and `RUST_BACKTRACE=1` gets me a register dump and backtrace:
```
C:\Users\Administrator\uv>target\debug\uv run python
error: unhandled exception in uv, please report a bug:
code 0xC0000005 at address 0x7ff662e5d03b
EXCEPTION_ACCESS_VIOLATION writing              0x1
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

C:\Users\Administrator\uv>set RUST_BACKTRACE=1

C:\Users\Administrator\uv>target\debug\uv run python
error: unhandled exception in uv, please report a bug:
code 0xC0000005 at address 0x7ff662e5d03b
EXCEPTION_ACCESS_VIOLATION writing              0x1
rax=0000000000000001 rbx=0000000000000000 rcx=0000027d822a8298
rdx=0000027d822a61a8 rsx=0000027d800208f0 rdi=0000000000000000
rsp=000000b975ed3a90 rbp=000000b975ed3b10  r8=0000000000000028
 r9=0000000000000040 r10=00007ff660ec0000 r11=00007ff66433bb11
r12=0000000000000000 r13=0000000000000000 r14=0000000000000000
r15=0000000000000000 rip=00007ff662e5d03b eflags=0000000000010206
stack backtrace:
   0:     0x7ff664206b3e - std::backtrace_rs::backtrace::win64::trace
                               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\..\..\backtrace\src\backtrace\win64.rs:85
   1:     0x7ff664206b3e - std::backtrace_rs::backtrace::trace_unsynchronized
                               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\..\..\backtrace\src\backtrace\mod.rs:66
   2:     0x7ff664206b3e - std::backtrace::Backtrace::create
                               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\backtrace.rs:331
   3:     0x7ff664206847 - std::backtrace::Backtrace::capture
                               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\backtrace.rs:296
   4:     0x7ff661c16792 - uv::windows_exception::unhandled_exception_filter
                               at C:\Users\Administrator\uv\crates\uv\src\windows_exception.rs:109
   5:     0x7ffc9aa8fb4d - UnhandledExceptionFilter
   6:     0x7ffc9d308595 - memset
   7:     0x7ffc9d2eec46 - _C_specific_handler
   8:     0x7ffc9d3043bf - _chkstk
   9:     0x7ffc9d29186e - RtlVirtualUnwind2
  10:     0x7ffc9d3033ae - KiUserExceptionDispatcher
  11:     0x7ff662e5d03b - core::ptr::write
                               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc\library\core\src\ptr\mod.rs:1655
  12:     0x7ff662e5d03b - core::ptr::mut_ptr::impl$0::write
                               at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc\library\core\src\ptr\mut_ptr.rs:1509
  13:     0x7ff662e5d03b - uv_client::base_client::BaseClientBuilder::build
                               at C:\Users\Administrator\uv\crates\uv-client\src\base_client.rs:269
  14:     0x7ff6619bb991 - uv_python::installation::impl$0::fetch::async_fn$0
                               at C:\Users\Administrator\uv\crates\uv-python\src\installation.rs:230
```

Very much open to tweaking the output format.

---

_Review requested from @BurntSushi by @zanieb on 2025-07-13 13:47_

---

_Review requested from @Gankra by @zanieb on 2025-07-13 13:47_

---

_Review requested from @samypr100 by @zanieb on 2025-07-13 13:47_

---

_@samypr100 reviewed on 2025-07-13 20:41_

---

_Review comment by @samypr100 on `crates/uv/src/windows_exception.rs`:97 on 2025-07-13 20:41_

```suggestion
                    eprintln!("EXCEPTION_STACK_OVERFLOW");
```

Fixes failing `typo` job in CI

---

_Review comment by @samypr100 on `crates/uv/src/windows_exception.rs`:11 on 2025-07-13 20:44_

```suggestion
#![allow(unsafe_code)]
#![allow(clippy::print_stderr)]
```

Will help pass clippy failing CI job for windows

---

_@samypr100 reviewed on 2025-07-13 20:44_

---

_@samypr100 reviewed on 2025-07-13 21:00_

---

_Review comment by @samypr100 on `crates/uv/src/windows_exception.rs`:121 on 2025-07-13 21:00_

Is there a reason we'd prefer EXCEPTION_CONTINUE_SEARCH (0) over EXCEPTION_EXECUTE_HANDLER (1)?

---

_@samypr100 reviewed on 2025-07-13 21:01_

---

_Review comment by @samypr100 on `crates/uv/src/windows_exception.rs`:56 on 2025-07-13 21:01_

aarch64 toolchain seems to think accessing `c.Anonymous.Anonymous` is unsafe

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:121 on 2025-07-14 02:44_

I don't totally understand the difference in behavior here when we're talking about an unhandled exception filter of which there is only one (as opposed to a vectored exception handler, SEH, etc. where there might actually be a next thing to call). My vague sense is that this alerts a debugger (as a "second chance exception") instead of crashing the process, which seems good?

(Some things I am reading on the internet, e.g., [this](https://billdemirkapi.me/abusing-exceptions-for-code-execution-part-2/), and the relevant WINE source, make me think that under the hood the only thing that exists is SEH / unwinding, and something very low level sets up a SEH catch block around `main` that calls some global function pointer and all that `SetUnhandledExceptionFilter` does is update that pointer.)

In practice I did this because Google Crashpad returns EXCEPTION_CONTINUE_SEARCH, not EXCEPTION_EXECUTE_HANDLER.

---

_@geofft reviewed on 2025-07-14 04:53_

Here's something a little interesting: on i686, we don't get a proper backtrace. I wonder if the exception handler is called on a different stack or something.

```
C:\Users\Administrator\uv>cargo build --target i686-pc-windows-msvc -p uv
C:\Users\Administrator\uv>target\i686-pc-windows-msvc\debug\uv run python
error: unhandled exception in uv, please report a bug:
code 0xC0000005 at address 0x2e119b7
EXCEPTION_ACCESS_VIOLATION writing 0x1
eax=00000001 ebx=0effc208 ecx=0ef48ab0 edx=07030fb0 esi=0ef44a80 edi=0effc0a4
eip=02e119b7 ebp=0ef44ec8 esp=0ef44a80 eflags=00010202
stack backtrace:
   0:  0x41e265e - std::backtrace::Backtrace::capture
                       at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library\std\src\backtrace.rs:296
   1:  0x1a052e1 - uv::windows_exception::unhandled_exception_filter
                       at C:\Users\Administrator\uv\crates\uv\src\windows_exception.rs:112
   2: 0x75a354b2 - UnhandledExceptionFilter
   3: 0x779c7fdf - RtlGetFullPathName_UEx
   4: 0x779c7f1b - RtlGetFullPathName_UEx
```

---

_@samypr100 reviewed on 2025-07-14 05:03_

---

_Review comment by @samypr100 on `crates/uv/src/windows_exception.rs`:121 on 2025-07-14 05:03_

👍 Makes sense if the goal is to delegate to a possible outer SE handler (if any exists) and not treat this filter as the final handler. If we wanted to do filter chaining we'd need to keep the prev filter reference alive and loaded so this is probably good enough.

---

_Comment by @geofft on 2025-07-14 12:46_

Re that last comment, presumably we can unwind from the provided `eip` in the context structure, but I don't immediately see either backtrace-rs or libunwind having support for this, so we'll need to reach for another unwinder, at which point there's a really good question about why there already a good third-party crate to do this sort of thing (or really why it isn't in `std`). Maybe there is and I missed it? Maybe I should spin this out into its own crate?

Also - do we support Windows builds on platforms other than x86, x86-64, and aarch64? Right now the code will fail to compile, but I can add a dummy implementation that doesn't print registers on other platforms. We do seem to handle unknown Windows arches in uv-trampoline-builder, for instance. On the other hand I'm pretty sure that `Windows/Win32/System/Diagnostics/Debug/mod.rs` itself will fail to compile on other platforms because there's no definition for `struct CONTEXT`.

---

_Comment by @samypr100 on 2025-07-14 13:08_

> Also - do we support Windows builds on platforms other than x86, x86-64, and aarch64?

At least not to my knowledge

---

_@geofft reviewed on 2025-07-14 13:58_

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:121 on 2025-07-14 13:58_

Yeah, philosophically I want to treat this as something that does an informative print of information while the program is on its way to crashing, not a handler in the sense of something that claims full responsibility for the exception.

I suppose we could store the return value from `SetUnhandledExceptionFilter` with the pointer to the old filter and call it ourselves, but that feels complicated. There's also apparently the problem that [the old filter might have come from a now-unloaded DLL](http://www.nynaeve.net/?p=43) and because there's no explicit unregistration API you have no idea whether that pointer is still valid. Alternatively maybe we should use `AddVectoredExceptionHandler` which does seem to do a registration/unregistration API with a list of handlers? Or we could do this with an actual SEH try-except handler of our own on the stack (which we don't have to do in Rust if that's hard, I bet we can write a tiny C program to forward to calling `uv::main()`).

I am trying so hard not to look at the leaked NT source code to figure out what all of this _actually_ does because there's somehow not a lot of documentation, either from MS or from other people. As a data point, [on ReactOS](https://github.com/reactos/reactos/blob/0.4.15-release/dll/win32/kernel32/client/except.c#L352-L358) (which is reverse-engineered from NT and tries to be compatible with it), returning `EXCEPTION_EXECUTE_HANDLER` from your custom filter will skip over code in `UnhandledExceptionFilter` to print a stack trace (custom to ReactOS, not present in NT), show the dialog box (the thing NT apparently no longer does unless you enable it), and start a debugger (present in NT). And indeed `UnhandledExceptionFilter` is called from a big SEH try/except around calling the start address for [the main process](https://github.com/reactos/reactos/blob/0.4.15-release/dll/win32/kernel32/client/proc.c#L465) or [thread](https://github.com/reactos/reactos/blob/0.4.15-release/dll/win32/kernel32/client/thread.c), and if you return early from it, the process will just terminate without any of those other niceties.

---

_Comment by @Gankra on 2025-07-14 14:19_

FWIW for a rust crate, there is https://github.com/EmbarkStudios/crash-handling/tree/main/crash-handler 

---

_@zanieb approved on 2025-07-14 14:21_

---

_Comment by @Gankra on 2025-07-14 14:32_

Some context on the state of the art of crash-reporting (you do not need to implement this, just infodumping): You want to spawn two processes: a parent "crash monitor" and a child (your actual app). When the child crashes the parent is signalled to scrape info from the child, generate a minidump, and then sends the minidump to the developer's servers. In this past we had a process analyze itself but "i just segfaulted, time to do a bunch of complex logic and networking" is uhhh dubious.

Because shipping two binaries is weird, it's typically to just have a single binary that spawns a second copy of itself.

Minidumps, by virtue of containing the memory of stacks, may contain PII so generally you then process the minidump serverside to produce an anonamized summary (error code, stacktrace, system info...).

All of this is a ton of work and infra which is why I tell people "yeah just use sentry.io" which in fact uses minidump tooling written in rust. However funnily enough (at least a few years ago) their offerings for rust were basically just a panic hook because a rust app segfaulting was so rare! Hopefully since then they've added segfault handling. But anyway us rolling our own here just so the user gets a vaguely useful diagnostic is Fine.

---

_Merged by @geofft on 2025-07-14 14:42_

---

_Closed by @geofft on 2025-07-14 14:42_

---

_@Gankra reviewed on 2025-07-14 14:43_

---

_Review comment by @Gankra on `crates/uv/src/windows_exception.rs`:45 on 2025-07-14 14:43_

rsx is paired with rsi here

---

_Review comment by @BurntSushi on `crates/uv/src/windows_exception.rs`:79 on 2025-07-14 14:44_

Also, `eprintln!` will try to acquire a lock. If that lock is held by some other part of the code (which doesn't seem that unlikely to me) when this handler is executed, then presumably this will deadlock. (IDK if acquiring locks is async-signal-safe on Windows either.)

---

_@BurntSushi reviewed on 2025-07-14 14:49_

I think it would be good to try and fix printing so that we don't go through `eprintln!`, but this otherwise LGTM. (Noting that I don't really have a lot of Windows experience, so my review isn't worth much.)

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:79 on 2025-07-14 15:02_

Yeah, the lock is the thing I was thinking of re async-signal-safety (which isn't exactly a phrase used on Windows, but the concept is exactly the same because you get called in a new stack frame at some arbitrary point when an exception happens) and I do expect that to be a risk. I don't see a lockless/async-signal-safe way to print stuff in `std`, the console APIs looked more complicated than I wanted to deal with (you gotta get a handle, do encoding, etc.), and I sort of assume that Windows' implementation of POSIX `write` isn't actually async-signal-safe.

FWIW I went with `eprintln!` because we're using `anstream::eprintln!` elsewhere for printing messages, which I assume wraps it and takes out the same lock. (It just doesn't trigger the Clippy lint.)

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:45 on 2025-07-14 15:04_

oops, I'll fix this if I fix something else to save the CI ("rsx" is not a register anyway)

---

_@geofft reviewed on 2025-07-14 15:09_

Thanks for the context @Gankra! Also recording here the link you sent me to https://github.com/rust-minidump/minidump-pipeline so I don't lose it, both of these answer my question of "how are you supposed to do this in Rust."

It does seem to me like if we want to do something more complicated we should go with one of the third-party SaaS options, either some vendor that specializes in this sort of reporting or signing up for Microsoft's own Windows Error Reporting thing. Adding an out-of-process handler for `uv.exe` seems complicated and not something we should do by default given that we've seen a single-digit number of segfaults and we care about startup performance.

---

_@BurntSushi reviewed on 2025-07-14 15:22_

---

_Review comment by @BurntSushi on `crates/uv/src/windows_exception.rs`:79 on 2025-07-14 15:22_

I think you can use [`AsRawHandle`](https://doc.rust-lang.org/std/os/windows/io/trait.AsRawHandle.html) on the result of [`std::io::stderr`](https://doc.rust-lang.org/std/io/fn.stderr.html), and then [`FromRawHandle`](https://doc.rust-lang.org/std/os/windows/io/trait.FromRawHandle.html) to create a `File`. Then you should be able to `write` to that. That won't acquire any locks.

I do not know if the underlying syscall (`WriteConsoleW`) is async-signal-safe on Windows, but this will at least avoid the possible deadlock that can occur by using `eprintln!`.

---

_@geofft reviewed on 2025-07-14 15:51_

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:79 on 2025-07-14 15:51_

Okay, hm, that looks easier than what https://github.com/rust-lang/rust/blob/master/library/std/src/sys/stdio/windows.rs and https://github.com/rust-lang/rust/blob/master/library/std/src/sys/pal/windows/handle.rs are doing (which I think is the code behind the `stderr` lock). There are two implementations there:
* If stderr is a console that either uses a non-UTF-8 code page or is on Windows 7, then it writes UTF-16 with `WriteConsoleW`. There's some handling for incomplete/invalid UTF-8 which wouldn't apply since we only need to write `str`s. But I'm surprised at the code calling Windows API function `MultiByteToWideChar` instead of e.g. `str::encode_utf16()`, is that necessary?
* If stderr is either not a console, or the console uses the UTF-8 code page (and we're not on Windows 7), then we write bytes with `FromRawHandle` + `write`, which calls `NtWriteFile` (I got a little scared by the thing it does to `WaitForSingleObject` but I missed that this function is public API and we wouldn't have to reimplement it).

It seems like we have to have both implementations, in that `WriteConsole` won't work if stderr is redirected to a file and writing UTF-8 won't work if stderr is a non-UTF-8-code-page console. But it doesn't seem as bad as I was afraid of.

---

_@BurntSushi reviewed on 2025-07-14 15:59_

---

_Review comment by @BurntSushi on `crates/uv/src/windows_exception.rs`:79 on 2025-07-14 15:59_

Ah yeah, I think I'm wrong. Because writing to `File` won't call `WriteConsole`. So yeah, I think if it's a console, we'd have to do our own Windows FFI stuff. But otherwise we should be able to just get a `File` and write to it like normal.

As for `MultiByteToWideChar`, it looks like we used to use `str::encode_utf16`, but it [was switched due to performance](https://github.com/rust-lang/rust/pull/107110). I don't think we'll care about perf here, so using `str::encode_utf16` seems fine.

---

_Review comment by @samypr100 on `crates/uv/src/windows_exception.rs`:45 on 2025-07-14 17:12_

Was this meant to be fixed? (noticed it still there in main)

---

_@samypr100 reviewed on 2025-07-14 17:12_

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:45 on 2025-07-14 23:26_

Intentionally not yet to save the CI, but #14619 fixes this.

---

_Review comment by @geofft on `crates/uv/src/windows_exception.rs`:79 on 2025-07-14 23:26_

Took a stab at this in #14619. Style feedback is more than welcome.

---

_@geofft reviewed on 2025-07-14 23:27_

---
