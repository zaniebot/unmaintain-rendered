```yaml
number: 94
title: "windows doesn't detect stdin automatically"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - question
assignees: []
created_at: 2016-09-26T00:02:17Z
updated_at: 2016-11-20T20:01:11Z
url: https://github.com/BurntSushi/ripgrep/issues/94
synced_at: 2026-01-12T18:23:11Z
```

# windows doesn't detect stdin automatically

---

_@BurntSushi_

Namely, running `rg pat < file` will recursively search the current directory instead of `file`. A workaround is to do `rg pat - < file`, which will search file.

This is a consequence of fixing #19. Maybe there is a better way to detect whether stdin is a pipe automatically, but I don't know it. [@vadz in particular points out that it is quite hairy](https://github.com/BurntSushi/ripgrep/issues/19#issuecomment-249453628):

> Oh, sorry, I should have thought about this, this is actually a pretty well-known issue -- but without any good solution, unfortunately. The problem is that Windows doesn't have any concept of PTY, so when a program is running in any kind of terminal emulator, and not the standard console window, its stdin is always connected to a pipe, as far as Windows is concerned. Cygwin applications can distinguish between "real" pipes and normal input from the terminal, but I don't know of any way to do this without linking to cygwin1.dll.

I think you should still be able to detect whether stdin is a TTY when running inside the native console and you should be able to check for this (running inside console, I mean). But everything console-related in Windows is pretty hairy, just look at [our own code](https://github.com/wxWidgets/wxWidgets/blob/v3.1.0/src/msw/app.cpp#L288) for detecting whether we can write to a console...


---

_Label `bug` added by @BurntSushi on 2016-09-26 00:02_

---

_Label `question` added by @BurntSushi on 2016-09-26 00:02_

---

_Comment by @mungre on 2016-09-29 16:22_

For what it's worth, you can detect this particular case.  If you run `type file | rg pat` then the standard input handle in rg will be a pipe handle, but if you do `rg pat < file` then the standard input handle will be a file handle.  You can distinguish them using the result of

`GetFileType(GetStdHandle(STD_INPUT_HANDLE))`

One caveat: this is true of cmd.exe.  I don't know that other shells implement this the same way.


---

_Comment by @silverwind on 2016-10-07 07:02_

Also noticed that `cat something | rg` does not work as expected on Windows and still searches recursively. I'm on Cygwin and installed through `cargo`, if that matters. `ag` does not show this issue, so maybe some of their checks could be reimplemented.


---

_Comment by @BurntSushi on 2016-10-07 10:16_

@silverwind Does `ag` have the issue described in #19?


---

_Comment by @silverwind on 2016-10-07 17:27_

Nope, no such issue with `ag`. I compiled `ag` from source under the Cygwin. I think many of the Cygwin issues that were reported are because of the fact that people are installing native Windows binaries, which in most cases is very bad. These binaries expect Windows interfaces and behaviour. Much better to build on the system's native toolchain.

I assume by installing through `cargo` , everything is built from source, right?

Oh, and I did some more testing with rg. I've also tested with https://github.com/rprichard/winpty which is basically a wrapper to `cmd` which helps native Windows binaries to run smoothly in a Cygwin terminal. My results:
- `echo x | rg x`  has the issue that it searches recursively
- `winpty cmd /c 'echo x | rg x'` also has the issue
- `echo x | rg x` in a native `cmd` has the issue
- `winpty cmd` and entering `echo x | rg x` has the issue

edit: updated, my bad. It doesn't work in any of the attempts.


---

_Comment by @silverwind on 2016-10-07 17:39_

Oh, and because it likely matters, Rust nightly was installed in the `GNU` version:

```
rustc 1.14.0-nightly (ad19c32a5 2016-10-06)
```

So far, I don't see any other issues. Everything work except this stdin issue.


---

_Comment by @BurntSushi on 2016-10-07 19:02_

I don't have time at the moment to dig into this, but I'm skeptical that there's any big difference between compiling `ripgrep` from source on Windows and distributing a binary for Windows. If using a Windows binary is bad, could you please explain why in detail?

If you install with `cargo`, then yes, it's compiled from source.

Please also consider that I don't know much about Windows. I'm making it up as I go.


---

_Comment by @silverwind on 2016-10-07 19:38_

> If using a Windows binary is bad, could you please explain why in detail?

Take my answer with a grain of salts as I'm not really involved in low-level  development, but as a long-time Cygwin user, this is my understanding:

Natively build binaries (especially ones build under MSVS) target native Windows APIs, which are pretty much second-class citizens inside the Cygwin environment and prone to have a few bugs, especially when it comes to fundamental platform differences like TTY emulation.

On the other hand the POSIX APIs are much better supported and it's a goal of the project enough of POSIX so that projects written for Linux can be built without any changes.


---

_Comment by @elahn on 2016-10-22 09:19_

On Windows 10 native terminal, this works perfectly:

``` rust
/// Returns true if there is a tty on stdin.
#[cfg(windows)]
pub fn on_stdin() -> bool {
    use kernel32::{GetConsoleMode, GetStdHandle};
    use winapi::winbase::STD_INPUT_HANDLE;

    unsafe {
        let mut out = 0;
        GetConsoleMode(GetStdHandle(STD_INPUT_HANDLE), &mut out) != 0
    }
}
```

From what I gather, this was removed due to #19. If the Cygwin environment can't be detected and skip this, I think Cygwin users should use a different build with the current behaviour. I prefer the native terminal and pipe is important enough for me to maintain a fork until this is resolved.

Btw, ripgrep is awesome!


---

_Comment by @BurntSushi on 2016-11-19 22:58_

Allow me to summarize where I'm at with this incredibly annoying issue:
- In #19, ripgrep would hang when running simple queries like `rg foo` in mintty. This is because there's (apparently) no good way to detect whether stdin in a tty or not inside of MSYS2 based terminal emulators like mintty.
- Since #19 represents, IMO, the **worst failure mode possible**, it was critical that we fix it. I therefore changed ripgrep to believe it _always_ had a tty on stdin. This means that `rg foo` will do the right thing, but that piping, e.g., `cat bar | rg foo` no longer works because ripgrep doesn't detect the pipe on stdin. Instead, it searches the current directory recursively and ignores stdin completely. This is a _terrible_ failure mode, but not quite the _worst possible_ failure mode. Thus, we have this issue.
- Applications distributed with cygwin, like grep, appear to get this right. That is, `echo foo | grep foo` does the right thing in both mintty and in `cmd.exe`. So clearly, fixing this bug properly is possible, but the implementation escapes me.
- `git` faced a similar issue and uses a [spectacular hack](http://git.661346.n2.nabble.com/PATCH-mingw-make-isatty-recognize-MSYS2-s-pseudo-terminals-dev-pty-td7654456.html) to work around it. I don't understand it.
- `mintty` has at least two issues on this topic: [mintty/53](https://github.com/mintty/mintty/issues/56) and [mintty/482](https://github.com/mintty/mintty/issues/482). I'm not sure that there's anything actionable there.
- Solutions like "cygwin users just need to compile from source" are _insufficient_, not only because I haven't actually observed that to work (I'm compiling ripgrep from source on Windows when testing all of this), but it's also terrible UX.

What are the next steps?
- The status quo is terrible because native Windows users suffer , but reverting the fix to #19 is _even worse_.
- The key problem is that if `GetConsoleMode(stdin)` returns false, then we _can't trust it_ because it _always returns false_ in mintty. However, if `GetConsoleMode(stdout)` or `GetConsoleMode(stderr)` return true, then we _could_ trust a `GetConsoleMode(stdin)` call that returned false because we know we're in a console.

That leads to the following logic:

``` rust
    let on_stdin = unsafe {
        let mut out = 0;
        GetConsoleMode(GetStdHandle(STD_INPUT_HANDLE), &mut out) != 0
    };
    if on_stdin {
        // good enough, on_stdin is only true if there's a console
        true
    } else if on_stdout() || on_stderr() {
        // on_stdin could be a false negative, but if stdout/stderr is a
        // console, then we can trust the negative result.
        false
    } else {
        // We don't know whether we have a console or not, so always pretend
        // that we have a tty on stdin.
        true
    }
```

I've tried this out and it does now at least let ripgrep operate sanely on _native Windows_. It doesn't fix the mintty problem and it can still spaz out on you. For example, running this produces unexpected results:

```
echo foo | rg foo > log 2>&1
```

In this case, `GetConsoleMode(stdin)`, `GetConsoleMode(stdout)` and `GetConsoleMode(stderr)` all return false, and we therefore cannot distinguish between a false negative and a true negative.


---

_Comment by @vadz on 2016-11-20 01:47_

Sorry if I'm missing something here, but what's wrong with stealing the Git hack? It looks like it allows us to do exactly what we want, i.e. discover whether we're really using the console or not when `GetConsoleMode()` returns false, by checking the pipe name associated with stdin. Isn't this exactly what we want?


---

_Comment by @BurntSushi on 2016-11-20 01:53_

@vadz I don't really understand it and I need to understand it before figuring out how to write it in Rust.

(I am somewhat at the end of my rope on this issue too. For now, at least.)


---

_Comment by @BurntSushi on 2016-11-20 02:02_

@vadz By "understand it" I think I'm just not familiar with these header files (like I said before, I'm making it up as I go when it comes to Windows):

```
#include <winternl.h> 
#include <ntstatus.h> 
```

Presumably these header files provide things like `NtQueryObject` and `GetFileType`. I'm not quite sure anyone has written bindings to those in Rust. I could start down that path and figure it out, but like I said, I'm kind of sick of working on this bug.


---

_Comment by @vadz on 2016-11-20 02:04_

Conceptually it seems simple to me: we just check if we have a pipe and if the name of the pipe fits the pattern used by MSYS for the pipes it uses for terminal emulation.

I think the confusing part could be all this `_pioinfo` stuff, but we wouldn't need this, the original patch probably was done like this to minimize changes to the existing code, but we don't have such constraints. So all we need to do is to call `NtQueryObject()` (which is almost like any other Win32 API, except it's a wrapper for a kernel function and not a Win32 call) and match the name returned by it again `"msys-XXXX-ptyN-XX"`.

BTW, notice that if we don't need to support XP (do we?), then we could use Win32 [GetFileInformationByHandleEx()](https://msdn.microsoft.com/en-us/library/aa364953.aspx) function instead. But OTOH `NtQueryObject()`, despite being officially internal is surely not going anywhere any time this millennium as there is so much code using it...

P.S. Just got the page update with your latest reply. Unfortunately here it's my lack of Rust knowledge that works against me. I hoped that these function could be used in the same way as `GetConsoleMode()`, for example. What's the difference between them from your point of view?

P.P.S. I do understand being sick of it very well though.


---

_Comment by @BurntSushi on 2016-11-20 02:15_

@vadz With respect to `GetConsoleMode` vs `NtQueryObject`... I can [`GetConsoleMode` in our `kernel32-sys` crate](https://docs.rs/kernel32-sys/0.2.2/x86_64-pc-windows-msvc/kernel32/?search=GetConsoleMode) but I can't find [`NtQueryObject` in the same crate](https://docs.rs/kernel32-sys/0.2.2/x86_64-pc-windows-msvc/kernel32/?search=NtQueryObject). I have _no idea_ what kind of interaction is at play here.

> BTW, notice that if we don't need to support XP (do we?), then we could use Win32 GetFileInformationByHandleEx() function instead.

Ah! We definitely don't need to support XP, and [`GetFileInformationByHandleEx` is in the `kernel32-sys` crate](https://docs.rs/kernel32-sys/0.2.2/x86_64-pc-windows-msvc/kernel32/?search=GetFileInformationByHandleEx). Yay!

So I think what I need to do is just use `GetFileInformationByHandleEx` on the `stdin` descriptor and ask for `FILE_NAME_INFO`, and then do the same name comparison check?

> I think the confusing part could be all this _pioinfo stuff, but we wouldn't need this, the original patch probably was done like this to minimize changes to the existing code, but we don't have such constraints. So all we need to do is to call NtQueryObject() (which is almost like any other Win32 API, except it's a wrapper for a kernel function and not a Win32 call) and match the name returned by it again "msys-XXXX-ptyN-XX".

This really helps actually. I didn't really grok much of that, so thank you for explaining that to me!


---

_Comment by @vadz on 2016-11-20 02:21_

> So I think what I need to do is just use GetFileInformationByHandleEx on the stdin descriptor and ask for FILE_NAME_INFO, and then do the same name comparison check?

Yes, I think so (but I didn't test it). AFAICS the only tricky thing risks to be interpreting the data returned by this function, as Rust code will somehow need to transmute the untyped byte buffer `lpFileInformation` to [FILE_NAME_INFO struct](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364388.aspx). But if you know the right incantation to do it, I think you're very close to solving this bug.

Good luck!


---

_Comment by @BurntSushi on 2016-11-20 02:24_

Yup, I can definitely handle that part (Rust literally has a `transmute` function). I just have a tough time navigating the Windows world, but I think you filled in all the missing gaps for me (if it works :P). Thanks again!


---

_Comment by @BurntSushi on 2016-11-20 06:06_

Yup, this does seem to work! Inside mintty, if I run `rg foo`, then the filename for stdin in `\\cygwin-c5e39b7a9d22bafb-pty0-from-master`. But if I run `echo foo | rg foo`, then the filename for stdin is `\\cygwin-c5e39b7a9d22bafb-3172-pipe-0x18`.


---

_Comment by @BurntSushi on 2016-11-20 06:25_

This is the golden ticket for posterity:

``` rust
/// Returns true if there is an MSYS tty on the given handle.
#[cfg(windows)]
fn msys_tty_on_handle(handle: HANDLE) -> bool {
    use std::ffi::OsString;
    use std::mem;
    use std::os::raw::c_void;
    use std::os::windows::ffi::OsStringExt;
    use std::slice;

    use kernel32::{GetFileInformationByHandleEx};
    use winapi::fileapi::FILE_NAME_INFO;
    use winapi::minwinbase::FileNameInfo;
    use winapi::minwindef::MAX_PATH;

    unsafe {
        let size = mem::size_of::<FILE_NAME_INFO>();
        let mut name_info_bytes = vec![0u8; size + MAX_PATH];
        let res = GetFileInformationByHandleEx(
            handle,
            FileNameInfo,
            &mut *name_info_bytes as *mut _ as *mut c_void,
            name_info_bytes.len() as u32);
        if res == 0 {
            return true;
        }
        let name_info: FILE_NAME_INFO =
            *(name_info_bytes[0..size].as_ptr() as *const FILE_NAME_INFO);
        let name_bytes =
            &name_info_bytes[size..size + name_info.FileNameLength as usize];
        let name_u16 = slice::from_raw_parts(
            name_bytes.as_ptr() as *const u16, name_bytes.len() / 2);
        let name = OsString::from_wide(name_u16)
            .as_os_str().to_string_lossy().into_owned();
        name.contains("msys-") || name.contains("-pty")
    }
}
```


---

_Comment by @BurntSushi on 2016-11-20 06:27_

With that bit done, everything seems to work perfectly now in both `cmd.exe` and mintty.


---

_Closed by @BurntSushi on 2016-11-20 20:01_

---
