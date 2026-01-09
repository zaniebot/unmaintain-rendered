---
number: 11886
title: Handling of signal exit from subprocess is incorrect
type: issue
state: open
author: geofft
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-03-01T22:48:29Z
updated_at: 2025-03-03T16:07:57Z
url: https://github.com/astral-sh/uv/issues/11886
synced_at: 2026-01-07T13:12:18-06:00
---

# Handling of signal exit from subprocess is incorrect

---

_Issue opened by @geofft on 2025-03-01 22:48_

### Summary

crates/uv/src/commands/run.rs has logic to take a [`std::process::ExitStatus`](https://doc.rust-lang.org/nightly/std/process/struct.ExitStatus.html) and convert it to an exit code as follows:

```rust
    if let Some(code) = status.code() {
        debug!("Command exited with code: {code}");
        if let Ok(code) = u8::try_from(code) {
            Ok(ExitStatus::External(code))
        } else {
            #[allow(clippy::exit)]
            std::process::exit(code);
        }
    } else {
        #[cfg(unix)]
        {
            use std::os::unix::process::ExitStatusExt;
            debug!("Command exited with signal: {:?}", status.signal());
            // Following https://tldp.org/LDP/abs/html/exitcodes.html, a fatal signal n gets the
            // exit code 128+n
            if let Some(mapped_code) = status
                .signal()
                .and_then(|signal| u8::try_from(signal).ok())
                .and_then(|signal| 128u8.checked_add(signal))
            {
                return Ok(ExitStatus::External(mapped_code));
            }
        }
        Ok(ExitStatus::Failure)
    }
```

This is not quite correct, as the Rust docs allude to when they say, "An `ExitStatus` represents every possible disposition of a process. On Unix this is the wait status. It is _not_ simply an exit status (a value passed to `exit`)."

The 128 + n behavior is the behavior of the _shell_: if a process exits with a signal, the shell will set `$?` to 128 + n. But the shell will _also_ print a message indicating the signal, e.g. "Segmentation fault (core dumped)" or "Killed". It can do this because at the OS/kernel level, the UNIX wait status is a two-byte field. There are eight bits (not seven!) for the argument to `exit` (or, equivalently, the return value of `main`), seven bits for the signal, and one bit for whether core was dumped. If the signal field is zero, then the process exited normally and the exit status field is meaningful; otherwise the signal field is meaningful. (There are a few more variants of interpreting the 16-bit exit status field if a process is stopped without exiting or continued from being stopped; they aren't particularly relevant here, I think.)

In practice, this produces the bug that `uv run python` etc. differs from directly running `python` if the process terminates with a signal, because to the shell, `uv` appears to _exit normally with an exit code over 128_ as opposed to exiting with a signal:

```
$ cat segfault.py
import ctypes
ctypes.CDLL("").strlen(1)
$ `uv python find` segfault.py
Segmentation fault (core dumped)
$ echo $?
139
$ uv run --script segfault.py
$ echo $?
139
```

So it's very hard for a user to tell that their process died with a segfault (or whatever other signal). Compare with this case where there is no segfault at all:

```
$ cat not-segfault.py 
import sys
sys.exit(139)
$ `uv python find` not-segfault.py 
$ echo $?
139
$ uv run --script not-segfault.py 
$ echo $?
139
```

I think we should, roughly, raise that `debug!` to a normal eprintln, and print the string form (as returned by [strsignal(3)](https://man7.org/linux/man-pages/man3/strsignal.3.html), maybe via [`nix::sys::signal::Signal`](https://docs.rs/nix/latest/nix/sys/signal/enum.Signal.html#method.try_from)'s `try_from` and `as_str`, defaulting to `"Signal {signal}"` if the signal is somehow unknown) and append `" (core dumped)"` if `status.core_dumped()`. This would provide analogous behavior to the shell. (Or alternatively we can pass the full `ExitStatus` back to `main()` and put the signal-printing logic there, or something.)

It is a little unfortunate that if the user is calling from _not_ a shell, they'll get an extra line to stderr that they wouldn't need, plus they'll see a normal exit instead of a signal exit from `uv`. But honestly I think that's fine—I can't think of a situation in practice where this would be terrible. The only real way to avoid this would be either to exec the target process instead of starting it as a child (I assume there's a good reason why we don't) or to artificially signal ourselves with the same signal (which I think would be misleading, in that it would make users try to figure out why `uv` segfaulted when it didn't).

(Originally reported in Discord #help thread ["coverage segfaults, does uv swallow it ?"](https://discord.com/channels/1039017663004942429/1344953548533923870/1344953548533923870).)

### Platform

UNIX

### Version

HEAD (8dd079f2ad530a8e5c91822f967f0ba140e4ec0a)

### Python version

_No response_

---

_Label `bug` added by @geofft on 2025-03-01 22:48_

---

_Comment by @zanieb on 2025-03-02 01:09_

cc @konstin who recently changed this to resolve https://github.com/astral-sh/uv/issues/10751

Discussion on using exec in https://github.com/astral-sh/uv/issues/3095

---

_Label `great writeup` added by @zanieb on 2025-03-02 01:09_

---

_Comment by @konstin on 2025-03-03 15:14_

> The only real way to avoid this would be either to exec the target process instead of starting it as a child (I assume there's a good reason why we don't) or to artificially signal ourselves with the same signal (which I think would be misleading, in that it would make users try to figure out why uv segfaulted when it didn't).

Does this mean that even though we have `[[noreturn]] void exit(int status);`, there is no way to set the signal + code dumped byte ourselves on exit to get the shell handling as if we had `execve`'d?

---

_Comment by @geofft on 2025-03-03 16:07_

> Does this mean that even though we have `[[noreturn]] void exit(int status);`, there is no way to set the signal + code dumped byte ourselves on exit to get the shell handling as if we had `execve`'d?

Correct, there is no way unless you count `raise(SIGSEGV)` (i.e., `kill(getpid(), SIGSEGV)`).

---

_Referenced in [astral-sh/uv#12658](../../astral-sh/uv/issues/12658.md) on 2025-04-03 19:10_

---
