---
number: 12244
title: uv panics when resizing terminal window
type: issue
state: open
author: SteveMcGrath
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-03-17T18:00:49Z
updated_at: 2025-06-01T04:58:49Z
url: https://github.com/astral-sh/uv/issues/12244
synced_at: 2026-01-10T01:25:17Z
---

# uv panics when resizing terminal window

---

_Issue opened by @SteveMcGrath on 2025-03-17 18:00_

### Summary

Whenever i resize my terminal window, I get spammed with the following (or similar) errors for as long as I am resizing my window.  I started getting these when a switch to using ghostty as my terminal app and am wondering if the refresh/repaint interval on the resize is just faster than expected.

```
❯ thread 'main2' panicked at /Users/brew/Library/Caches/Homebrew/cargo_cache/registry/src/index.crates.io-6f17d22bba15001f/clap_complete-4.5.44/src/aot/shells/fish.rs:45:33:
failed to write completion file: Os { code: 32, kind: BrokenPipe, message: "Broken pipe" }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
thread 'main' panicked at /private/tmp/uv-20250219-7867-jnorhr/uv-0.6.2/crates/uv/src/lib.rs:1933:10:
```

When i enable the full backtrace im getting:

```
❯ thread 'main2' panicked at /Users/brew/Library/Caches/Homebrew/cargo_cache/registry/src/index.crates.io-6f17d22bba15001f/clap_complete-4.5.44/src/aot/shells/fish.rs:269:8:
failed to write completion file: Os { code: 32, kind: BrokenPipe, message: "Broken pipe" }
stack backtrace:
   0:        0x1004b8e70 - __mh_execute_header
   1:        0x1001a04cc - __mh_execute_header
   2:        0x1004b8c64 - __mh_execute_header
   3:        0x1004b7638 - __mh_execute_header
   4:        0x1004b9334 - __mh_execute_header
   5:        0x1004b92a4 - __mh_execute_header
   6:        0x1004b6da4 - __mh_execute_header
   7:        0x101643640 - __mh_execute_header
   8:        0x1016435cc - __mh_execute_header
   9:        0x1001915c4 - __mh_execute_header
  10:        0x10069c338 - __mh_execute_header
  11:        0x100685184 - __mh_execute_header
  12:        0x100c802ec - __mh_execute_header
  13:        0x10009a5ec - __mh_execute_header
  14:        0x100495810 - __mh_execute_header
  15:        0x18101c2e4 - __pthread_deallocate
thread 'main' panicked at /private/tmp/uv-20250219-7867-jnorhr/uv-0.6.2/crates/uv/src/lib.rs:1933:10:
Tokio executor failed, was there a panic?: Any { .. }
stack backtrace:
   0:        0x1004b8e70 - __mh_execute_header
   1:        0x1001a04cc - __mh_execute_header
   2:        0x1004b8c64 - __mh_execute_header
   3:        0x1004b7638 - __mh_execute_header
   4:        0x1004b9334 - __mh_execute_header
   5:        0x1004b92a4 - __mh_execute_header
   6:        0x1004b6da4 - __mh_execute_header
   7:        0x101643640 - __mh_execute_header
   8:        0x1016435cc - __mh_execute_header
   9:        0x100da1eec - __mh_execute_header
  10:        0x100c7c7fc - __mh_execute_header
```

system details:

```
❯ neofetch
                    'c.          steve@Aviendha.local
                 ,xNMM.          --------------------
               .OMMMMo           OS: macOS 15.3.2 24D81 arm64
               OMMM0,            Host: MacBookPro18,2
     .;loddo:' loolloddol;.      Kernel: 24.3.0
   cKMMMMMMMMMMNWMMMMMMMMMM0:    Uptime: 2 hours, 34 mins
 .KMMMMMMMMMMMMMMMMMMMMMMMWd.    Packages: 424 (brew)
 XMMMMMMMMMMMMMMMMMMMMMMMX.      Shell: fish 3.7.1
;MMMMMMMMMMMMMMMMMMMMMMMM:       Resolution: 5120x1440
:MMMMMMMMMMMMMMMMMMMMMMMM:       DE: Aqua
.MMMMMMMMMMMMMMMMMMMMMMMMX.      WM: Quartz Compositor
 kMMMMMMMMMMMMMMMMMMMMMMMMWd.    WM Theme: Blue (Light)
 .XMMMMMMMMMMMMMMMMMMMMMMMMMMk   Terminal: ghostty
  .XMMMMMMMMMMMMMMMMMMMMMMMMK.   CPU: Apple M1 Max
    kMMMMMMMMMMMMMMMMMMMMMMd     GPU: Apple M1 Max
     ;KMMMMMMMWXXWMMMMMMMk.      Memory: 8484MiB / 65536MiB
       .cooc,.    .,coo:.
```

### Platform

macOS 15.3

### Version

uv 0.6.2

### Python version

Python 3.12.9

---

_Label `bug` added by @SteveMcGrath on 2025-03-17 18:00_

---

_Comment by @geofft on 2025-03-17 22:15_

At one level, what's going on here is that the shell completion generation code is trying to write a script but the reader is exiting/closing the pipe before it's done, which you can repro in isolation with e.g. `uv generate-shell-completion fish | head`.

So there's sort of two angles here. The first is, should uv exit quietly if that happens? I _think_ the answer is yes. The traditional UNIX behavior is for programs to just exit quietly via the SIGPIPE signal defaulting to killing the process (just like SIGINT, SIGKILL, etc. do); indeed, `| head` basically expects that behavior. I believe that this is largely happening because Rust by default ignores SIGPIPE (rust-lang/rust#62569), and so if you actually do a write to a broken pipe, instead of the program exiting you get an error from the write syscall, which turns into this panic. (There might be something more complicated with Tokio but I think there isn't.) If uv resets SIGPIPE back to the default UNIX behavior, then it would exit quietly and you wouldn't see the error message. There is an argument that diagnostics about the fact that the completion wasn't fully written are useful "explicit is better than implicit"). I'm not sure if that's a real concern here.

The second angle is, why is this code running on resize, and why is it getting into a broken pipe situation? My initial guess is this has something to do with Ghostty's built-in fish integration. The [uv setup instructions](https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion) encourage you to put a `uv generate-shell-completion fish | source` in ~/.config/fish/config.fish, and fish says that config.fish runs on _every_ shell invocation, including non-interactive ones. Perhaps Ghostty is invoking a subshell on every resize step and terminating it before the next resize step, or something? That does seem like a lot of code to run on every single resize step, and it sort of feels at odds with the Ghostty philosphy (which uv shares) of being fast.

It does seem like it might be better anyway to [only generate shell completion in the interactive case as the fish docs encourage](https://fishshell.com/docs/current/language.html#configuration), e.g. something like
```fish
status --is-interactive; and uv generate-shell-completion fish | source
```

---

_Comment by @SteveMcGrath on 2025-03-18 17:18_

So i can definitely confirm that checking for interactivity first seems to solve the issue.  Maybe a minor adjustment to the docs are in order?

```
status --is-interactive; and uv generate-shell-completion fish | source
```

---

_Comment by @zanieb on 2025-03-18 17:57_

That seems prudent

---

_Label `help wanted` added by @zanieb on 2025-06-01 04:58_

---
