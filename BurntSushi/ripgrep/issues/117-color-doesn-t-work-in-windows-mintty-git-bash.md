```yaml
number: 117
title: "Color doesn't work in windows mintty (git bash)"
type: issue
state: closed
author: auxym
labels:
  - bug
assignees: []
created_at: 2016-09-27T12:57:56Z
updated_at: 2016-11-23T08:38:27Z
url: https://github.com/BurntSushi/ripgrep/issues/117
synced_at: 2026-01-12T18:23:11Z
```

# Color doesn't work in windows mintty (git bash)

---

_@auxym_

I can't get rg to output color in mintty, ie the default term that comes with git for windows and MSYS2. Tried `--color always` and `-p`, to no avail. Color works fine (by default) in standard cmd.exe.

Color (`--color=auto`) works with standard grep in mintty, or at least the grep build that comes with git for windows.


---

_Comment by @BurntSushi on 2016-09-27 13:05_

Do you know if mintty uses the Windows console for coloring? Or does it use something else? I wonder if `ripgrep` is supposed to be using ANSI escape sequences here and is instead trying to use the Windows console.


---

_Comment by @auxym on 2016-09-27 15:40_

Here is the raw output from `echo "AAAaaa" | grep --color=always aa > grepcolor.txt` in mintty:

```
b'AAA\x1b[01;31m\x1b[Kaa\x1b[m\x1b[Ka\n'
```

I tried the same exercise with rg, but couldn't get it to output the escape codes, both in mintty and cmd.exe. It looks like  rg doesn't detect terminal output and ignores the color=always options.


---

_Comment by @BurntSushi on 2016-09-27 15:50_

It does detect terminal output, just apparently not on Windows. :-) I think the issue is that I baked an assumption into `ripgrep` that if it's on Windows, then it should do coloring with the Windows console. It looks like in the case of `mintty`, it should use ANSI escape codes? Is that right?

For `cmd.exe`, I don't think it should be printing escape codes.

cc @retep998 Would you be able to add any insight here? Thanks so much. :-)


---

_Comment by @retep998 on 2016-09-27 16:36_

@BurntSushi Basically with mintty you have a pipe instead of a console. Because of this there is no API you can call to determine whether a pipe is actually a terminal that accepts ansi escape codes. The best you can do is that if you're not talking to a console, then check whether `TERM` is set, if it is then you're _probably_ in a terminal emulator (or being redirected to a file while inside a terminal emulator).

Conhost (which is what `cmd.exe` uses) doesn't support escape codes in older Windows, and in Windows 10 it only supports them if a certain mode is enabled.


---

_Comment by @leafgarland on 2016-09-28 12:31_

Perhaps the `--color` flag could support extra options such as `ansi` and `wincon`, which would behave the same as `always` but also force one or the other color output method. There are enough command line options in Windows that it seems unlikely a cmdline tool can guess the exact requirements (e.g. Windows 10 can support ansi, or when using something like [Maximus5/ConEmu](https://github.com/Maximus5/ConEmu)).


---

_Comment by @BurntSushi on 2016-09-28 12:35_

@leafgarland That's plausible. Good idea.


---

_Comment by @retep998 on 2016-09-28 17:38_

I don't think you really need the user's help deciding which to use. If you're in a Windows console, then the Windows console API will work all the time. If you're not in a Windows console, then the Windows console API will work none of time and the only other option is ansi escape codes. So I'd probably enable color by default in the Windows console, and elsewhere support color using ansi escape codes only if enabled via `--color=always`.


---

_Label `bug` added by @BurntSushi on 2016-09-29 01:12_

---

_Comment by @thierryvolpiatto on 2016-10-15 08:06_

We have the same problem in emacs where ripgrep is apparently not recognizing pseudo emacs terminal (pty) as a terminal, see https://github.com/emacs-helm/helm/issues/1624. 


---

_Comment by @BurntSushi on 2016-10-15 13:18_

@thierryvolpiatto Is that on Windows? If not, it sounds like an entirely distinct issue.


---

_Comment by @thierryvolpiatto on 2016-10-15 14:22_

Andrew Gallant notifications@github.com writes:

> @thierryvolpiatto Is that on Windows?

No, on GNU/Linux.

> If not, it sounds like an entire distinct issue.

Ok, should I open an other issue ?

## 

Thierry


---

_Comment by @BurntSushi on 2016-10-15 14:24_

@thierryvolpiatto I think #182 might be relevant.


---

_Comment by @thierryvolpiatto on 2016-10-15 14:31_

Ok, yes I just see @chunyang have opened another issue, thanks.


---

_Comment by @BurntSushi on 2016-11-19 18:55_

@retep998 I'm in the process of fixing this issue, and it actually looks like I can successfully get a handle to the Windows console when using `mintty`. Namely, no error occurs. Actually using the console has no effect, so it appears that we do indeed need a way to force ANSI escape sequences.


---

_Comment by @retep998 on 2016-11-19 21:41_

@BurntSushi `GetStdHandle` succeeding does not necessarily mean it is a Windows console. You have to call `GetConsoleMode` on that handle to determine whether it is actually a Windows console. If `GetConsoleMode` succeeds, then always use the console API. If `GetConsoleMode` fails, then you _might_ be able to use ANSI escape codes, or you might be redirected to a file.


---

_Comment by @BurntSushi on 2016-11-19 22:09_

@retep998 This is the code I'm using to get a console, and it doesn't appear to return an error in `mintty`:

``` rust
    pub fn new() -> io::Result<Console> {
        let name = b"CONOUT$\0";
        let handle = unsafe {
            kernel32::CreateFileA(
                name.as_ptr() as *const i8,
                winapi::GENERIC_READ | winapi::GENERIC_WRITE,
                winapi::FILE_SHARE_WRITE,
                ptr::null_mut(),
                winapi::OPEN_EXISTING,
                0,
                ptr::null_mut(),
            )
        };
        if handle == winapi::INVALID_HANDLE_VALUE {
            return Err(io::Error::last_os_error());
        }
        let mut info = unsafe { mem::zeroed() };
        let res = unsafe {
            kernel32::GetConsoleScreenBufferInfo(handle, &mut info)
        };
        if res == 0 {
            return Err(io::Error::last_os_error());
        }
        let attr = TextAttributes::from_word(info.wAttributes);
        Ok(Console {
            handle: handle,
            start_attr: attr,
            cur_attr: attr,
        })
    }
```

So I'm not using `GetConsoleMode` but I am using `GetConsoleScreenBufferInfo`. Should that call fail if there's no console available? [The docs don't seem to say, but do seem to imply that it at least _can_ fail.](https://msdn.microsoft.com/en-us/library/windows/desktop/ms683171%28v=vs.85%29.aspx)


---

_Comment by @BurntSushi on 2016-11-19 22:11_

Hmm, I guess the difference here is that in that code above, I am opening `CONOUT$`, where as I do believe it fails if I do it on, say, `stdout`.


---

_Comment by @retep998 on 2016-11-19 22:16_

@BurntSushi Getting `CONOUT$` is usually the wrong thing to do, because it ignores what stdout is set to, completely bypassing any redirection, and just gets whatever the active framebuffer is. Stick with `GetStdHandle` as the way to obtain the handle.


---

_Comment by @BurntSushi on 2016-11-19 22:31_

@retep998 Ah I see now, OK that works! Thanks! My only problem now is figuring out how to detect whether stdout is tty or not inside of mintty. From searching, it seems like the problem is basically unsolveable, although I will note that `grep` from `cygwin` seems to get it right. [`git` itself seems to have hacked around it](http://git.661346.n2.nabble.com/PATCH-mingw-make-isatty-recognize-MSYS2-s-pseudo-terminals-dev-pty-td7654456.html) and the mintty project [doesn't have](https://github.com/mintty/mintty/issues/482) a good answer [either](https://github.com/mintty/mintty/issues/56).

_sigh_


---

_Closed by @BurntSushi on 2016-11-20 20:01_

---

_Comment by @BurntSushi on 2016-11-20 20:34_

For those following this issue but not #94, this will be fixed in the next ripgrep release!


---

_Comment by @lilianmoraru on 2016-11-22 15:36_

Colors stopped working in one of the releases, on an ARM target I tested `ripgrep` on.
I'll test again and see if this fixed the issue.

---

_Comment by @lilianmoraru on 2016-11-23 08:38_

Yey, colors work again on that ARM target.

---
