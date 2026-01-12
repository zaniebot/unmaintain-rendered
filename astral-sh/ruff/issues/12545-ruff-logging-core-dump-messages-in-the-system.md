```yaml
number: 12545
title: Ruff logging core dump messages in the system journal
type: issue
state: closed
author: Fissium
labels:
  - bug
  - server
assignees: []
created_at: 2024-07-27T17:46:36Z
updated_at: 2024-08-07T02:57:16Z
url: https://github.com/astral-sh/ruff/issues/12545
synced_at: 2026-01-12T15:54:52Z
```

# Ruff logging core dump messages in the system journal

---

_@Fissium_

I've noticed that ruff continues to run, but there are frequent core dump messages logged in the system journal. Below are the details from the most recent log entry:

```
PID: 4280 (ruff)
UID: 1000 (fissium)
GID: 1000 (fissium)
Signal: 6 (ABRT)
Timestamp: Sat 2024-07-27 20:31:43 MSK (1min 31s ago)
Command Line: /home/fissium/.local/share/nvim/mason/bin/ruff server
Executable: /home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff
Control Group: /user.slice/user-1000.slice/user@1000.service/app.slice/kitty-2546-0.scope
Unit: user@1000.service
User Unit: kitty-2546-0.scope
Slice: user-1000.slice
Owner UID: 1000 (fissium)
Boot ID: 37f1ef78ff7a449faf604ccbdf1e3943
Machine ID: 830b850f950448748db101e10b137d6b
Hostname: archlinux
Storage: /var/lib/systemd/coredump/core.ruff.1000.37f1ef78ff7a449faf604ccbdf1e3943.4280.1722101503000000.zst (present)
Size on Disk: 377K
Message: Process 4280 (ruff) of user 1000 dumped core.
                
    Stack trace of thread 4280:
    #0  0x00007c96eafde3f4 n/a (libc.so.6 + 0x963f4)
    #1  0x00007c96eaf85120 raise (libc.so.6 + 0x3d120)
    #2  0x00007c96eaf6c4c3 abort (libc.so.6 + 0x244c3)
    #3  0x00006023b6fe820a n/a (/home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff + 0xf7820a)
    #4  0x00006023b6fe29c1 n/a (/home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff + 0xf729c1)
    #5  0x00006023b6fe2574 n/a (/home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff + 0xf72574)
    #6  0x00006023b6fe12a9 n/a (/home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff + 0xf712a9)
    #7  0x00006023b6fe22c7 n/a (/home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff + 0xf722c7)
    #8  0x00006023b616a323 n/a (/home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff + 0xfa323)
    #9  0x00006023b6fdd09a n/a (/home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff + 0xf6d09a)
    #10 0x00006023b6de94ab n/a (/home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff + 0xd794ab)
    ELF object binary architecture: AMD x86-64
```

ruff version: `0.5.5`

system: `Arch Linux`

pyproject.toml
```toml
[tool.ruff]
exclude = [
  ".bzr",
  ".direnv",
  ".eggs",
  ".git",
  ".git-rewrite",
  ".hg",
  ".ipynb_checkpoints",
  ".mypy_cache",
  ".nox",
  ".pants.d",
  ".pyenv",
  ".pytest_cache",
  ".pytype",
  ".ruff_cache",
  ".svn",
  ".tox",
  ".venv",
  ".vscode",
  "__pypackages__",
  "_build",
  "buck-out",
  "build",
  "dist",
  "node_modules",
  "site-packages",
  "venv",
]

line-length = 88
indent-width = 4

[tool.ruff.lint]
select = ["ARG", "F", "E", "I", "W", "B", "C4", "UP", "RUF"]
fixable = ["I"]
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"
```

---

_Renamed from "ruff logging core dump messages in the system journal" to "Ruff logging core dump messages in the system journal" by @Fissium on 2024-07-27 17:46_

---

_Label `needs-info` added by @MichaReiser on 2024-07-28 13:37_

---

_Label `server` added by @MichaReiser on 2024-07-28 13:37_

---

_Comment by @MichaReiser on 2024-07-28 13:40_

Thanks for reporting. It will probably be difficult for us to fix this without additional information. What I see from the log is that the failing command is `ruff server`.

Have you ever noticed any VS Code error dialog telling you that it restarted the Ruff server because it crashed? If so, would you mind sharing the logs ([see troubleshooting section](https://github.com/astral-sh/ruff-vscode?tab=readme-ov-file#troubleshooting)) with us? That would greatly help in getting to the bottom of this.

---

_Comment by @Fissium on 2024-07-28 14:57_

@MichaReiser I've enabled logging for Neovim as described [here](https://docs.astral.sh/ruff/editors/setup/#neovim).

Here is the output of `lsp.log`:

```
[START][2024-07-28 17:48:43] LSP logging initiated
[ERROR][2024-07-28 17:48:43] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   0.002060162s DEBUG ThreadId(04) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /projects/hoff/qa/locust/.ruff_cache\n   0.002255498s DEBUG ThreadId(04) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /projects/hoff/qa/locust/.git\n"
[ERROR][2024-07-28 17:48:43] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   0.008470822s  WARN ruff:main ruff_server::server: LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.\n   0.008610409s DEBUG ruff:worker:0 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/shapes.py\n"
[WARN][2024-07-28 17:48:43] ...lsp/handlers.lua:135	"The language server copilot triggers a registerCapability handler for workspace/didChangeWorkspaceFolders despite dynamicRegistration set to false. Report upstream, this warning is harmless"
[ERROR][2024-07-28 17:48:46] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   3.324471758s DEBUG ruff:worker:1 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/shapes.py\n"
[ERROR][2024-07-28 17:48:46] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   3.647401371s DEBUG ruff:worker:3 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/shapes.py\n"
[ERROR][2024-07-28 17:48:47] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   3.775587620s DEBUG ruff:worker:4 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/shapes.py\n"
[ERROR][2024-07-28 17:48:48] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   4.975728581s DEBUG ruff:worker:2 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/shapes.py\n"
[ERROR][2024-07-28 17:48:49] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   5.988686776s DEBUG ruff:worker:5 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/shapes.py\n"
[START][2024-07-28 17:49:07] LSP logging initiated
[ERROR][2024-07-28 17:49:07] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   0.002190494s DEBUG ThreadId(04) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /projects/hoff/qa/locust/.ruff_cache\n   0.002365530s DEBUG ThreadId(04) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /projects/hoff/qa/locust/.git\n   0.006834485s  WARN ruff:main ruff_server::server: LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.\n   0.006959741s DEBUG ruff:worker:0 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[WARN][2024-07-28 17:49:08] ...lsp/handlers.lua:135	"The language server copilot triggers a registerCapability handler for workspace/didChangeWorkspaceFolders despite dynamicRegistration set to false. Report upstream, this warning is harmless"
[ERROR][2024-07-28 17:49:13] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   5.497319727s DEBUG ruff:worker:1 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:13] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   5.700150891s DEBUG ruff:worker:2 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:13] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   6.021996261s DEBUG ruff:worker:4 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:13] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"   6.022822469s DEBUG ruff:worker:3 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:20] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  13.279885122s DEBUG ruff:worker:5 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:21] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  13.421045496s DEBUG ruff:worker:6 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:22] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  14.528148095s DEBUG ruff:worker:8 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:22] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  14.743507416s DEBUG ruff:worker:9 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:22] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  14.828157864s DEBUG ruff:worker:7 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:22] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  15.002252569s DEBUG ruff:worker:10 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:23] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  16.006754048s DEBUG ruff:worker:15 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:23] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  16.251673924s DEBUG ruff:worker:12 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n  16.251757062s DEBUG  ruff:worker:0 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:38] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  30.459387541s DEBUG ruff:worker:13 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:38] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  30.459731552s DEBUG ruff:worker:14 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:38] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  30.766225537s DEBUG ruff:worker:11 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:38] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  30.964531576s DEBUG ruff:worker:16 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:38] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  31.087091472s DEBUG ruff:worker:17 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:47] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  40.227910493s DEBUG ruff:worker:18 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:49:51] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  43.431315731s DEBUG ruff:worker:19 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  54.527510086s DEBUG ruff:worker:20 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  54.811270187s DEBUG ruff:worker:21 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  54.850078309s DEBUG ruff:worker:22 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  54.880653541s DEBUG ruff:worker:23 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  54.898032891s DEBUG  ruff:worker:1 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  54.927953674s DEBUG  ruff:worker:2 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  54.965888320s DEBUG  ruff:worker:4 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  54.988297012s DEBUG  ruff:worker:3 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  55.018931472s DEBUG  ruff:worker:5 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  55.048670740s DEBUG  ruff:worker:6 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  55.093019078s DEBUG  ruff:worker:8 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:02] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  55.124206983s DEBUG  ruff:worker:9 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:03] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  55.359379598s DEBUG  ruff:worker:7 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
[ERROR][2024-07-28 17:50:03] .../vim/lsp/rpc.lua:770	"rpc"	"/home/fissium/.local/share/nvim/mason/bin/ruff"	"stderr"	"  55.921421891s DEBUG ruff:worker:10 ruff_server::resolve: Included path via `include`: /projects/hoff/qa/locust/src/users.py\n"
```


---

_Comment by @dhruvmanila on 2024-07-29 05:42_

Are there any indications in the editor that the server crashed (notifications?) because there's none in the provided logs? Do you still see the diagnostics and are they being refreshed when you change the buffer content after the crash?

---

_Comment by @Fissium on 2024-07-29 06:53_

There are no notifications in the editor indicating that the server has crashed. And I still see the diagnostics, and they are being refreshed when I change the buffer content even after the crash.

Here is a backtrace that I hope will help in identifying the issue:

```
Reading symbols from /home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff...

This GDB supports auto-downloading debuginfo from the following URLs:
  <https://debuginfod.archlinux.org>
Enable debuginfod for this session? (y or [n]) y
Debuginfod has been enabled.
To make this setting permanent, add 'set debuginfod enabled on' to .gdbinit.
Downloading separate debug info for /home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff
(No debugging symbols found in /home/fissium/.local/share/nvim/mason/packages/ruff/venv/bin/ruff)                                                                           
[New LWP 5765]
Downloading separate debug info for /usr/lib/libpthread.so.0
Downloading separate debug info for /usr/lib/librt.so.1                                                                                                                     
Downloading separate debug info for /usr/lib/libm.so.6                                                                                                                      
Downloading separate debug info for /usr/lib/libdl.so.2                                                                                                                     
Downloading separate debug info for /usr/lib/libc.so.6                                                                                                                      
Downloading separate debug info for /lib64/ld-linux-x86-64.so.2                                                                                                             
Downloading separate debug info for system-supplied DSO at 0x7526370f5000                                                                                                   
[Thread debugging using libthread_db enabled]                                                                                                                               
Using host libthread_db library "/usr/lib/libthread_db.so.1".
Core was generated by `/home/fissium/.local/share/nvim/mason/bin/ruff server'.
Program terminated with signal SIGABRT, Aborted.
Downloading source file /usr/src/debug/glibc/glibc/nptl/pthread_kill.c
#0  __pthread_kill_implementation (threadid=<optimized out>, signo=signo@entry=6, no_tid=no_tid@entry=0) at pthread_kill.c:44                                               
44	     return INTERNAL_SYSCALL_ERROR_P (ret) ? INTERNAL_SYSCALL_ERRNO (ret) : 0;
(gdb) bt
#0  __pthread_kill_implementation (threadid=<optimized out>, signo=signo@entry=6, no_tid=no_tid@entry=0) at pthread_kill.c:44
#1  0x0000752636e48463 in __pthread_kill_internal (threadid=<optimized out>, signo=6) at pthread_kill.c:78
#2  0x0000752636def120 in __GI_raise (sig=sig@entry=6) at ../sysdeps/posix/raise.c:26
#3  0x0000752636dd64c3 in __GI_abort () at abort.c:79
#4  0x00005cc3894cf20a in std::sys::pal::unix::abort_internal ()
#5  0x00005cc3894c99c1 in std::panicking::rust_panic_with_hook ()
#6  0x00005cc3894c9574 in std::panicking::begin_panic_handler::{{closure}} ()
#7  0x00005cc3894c82a9 in std::sys_common::backtrace::__rust_end_short_backtrace ()
#8  0x00005cc3894c92c7 in rust_begin_unwind ()
#9  0x00005cc388651323 in core::panicking::panic_fmt ()
#10 0x00005cc3894c409a in std::io::stdio::_eprint ()
#11 0x00005cc3892d04ab in ruff_server::message::init_messenger::{{closure}} ()
#12 0x00005cc3894c97b8 in std::panicking::rust_panic_with_hook ()
#13 0x00005cc3894c9574 in std::panicking::begin_panic_handler::{{closure}} ()
#14 0x00005cc3894c82a9 in std::sys_common::backtrace::__rust_end_short_backtrace ()
#15 0x00005cc3894c92c7 in rust_begin_unwind ()
#16 0x00005cc388651323 in core::panicking::panic_fmt ()
#17 0x00005cc3894c409a in std::io::stdio::_eprint ()
#18 0x00005cc388c0c9b0 in ruff::main ()
#19 0x00005cc388c10b23 in std::sys_common::backtrace::__rust_begin_short_backtrace ()
#20 0x00005cc388c0fa19 in std::rt::lang_start::{{closure}} ()
#21 0x00005cc3894ba1d6 in std::rt::lang_start_internal ()
#22 0x00005cc388c0d0d5 in main ()
```

---

_Comment by @MichaReiser on 2024-07-30 06:35_

Thanks for sharing the backtrace. 

Looking at the code it seems to me that we panic inside the `eprintln` that we use to print a catcher panic (oh well...) But tracing should have recorded the error already. 

https://github.com/astral-sh/ruff/blob/507f5c1137d18a30057ce4708ba6fc0656570dac/crates/ruff_server/src/message.rs#L36-L41

Maybe we should change the server code to use `log::error!` instead of the eprintln. @dhruvmanila what do you think?

@Fissium could you try setting the [`logFile`](https://docs.astral.sh/ruff/editors/settings/#logfile) setting. Maybe we can find the panic cause in there. 

---

_Comment by @dhruvmanila on 2024-07-30 07:09_

> Looking at the code it seems to me that we panic inside the `eprintln` that we use to print a catcher panic (oh well...) But tracing should have recorded the error already.

Shouldn't it also have notified the user about the panic via notification? It didn't in this case (https://github.com/astral-sh/ruff/issues/12545#issuecomment-2255080592) which is weird because the messenger is set in the `init_messenger` function itself at the start.

---

_Comment by @MichaReiser on 2024-07-30 07:14_

Possibly. It may have failed. We ignore the return type. What I suspect is happening is that the stderr stream gets closed by neovim (e.g. when exiting the editor?). We then try to write a message which panics and so will `eprintln`. 

---

_Comment by @dhruvmanila on 2024-07-30 07:22_

Hmm, that's interesting because the author mentioned that the server keeps running (or is it being restarted by the editor?) and it even refreshes the existing diagnostics.

---

_Comment by @Fissium on 2024-07-30 07:45_

@MichaReiser Here is the log file:
[ruff.log](https://github.com/user-attachments/files/16424047/ruff.log)


---

_Comment by @MichaReiser on 2024-07-30 08:17_

Thanks @Fissium this is very helpful!

```
   3.944361632s  INFO     ruff:main ruff_server::server::connection: Shutdown request received. Waiting for an exit notification...
   3.955358848s ERROR          main ruff_server::message: panicked at library/std/src/io/stdio.rs:1118:9:
failed printing to stderr: Broken pipe (os error 32)
   0: ruff_server::message::init_messenger::{{closure}}
   1: std::panicking::rust_panic_with_hook
   2: std::panicking::begin_panic_handler::{{closure}}
   3: std::sys_common::backtrace::__rust_end_short_backtrace
   4: rust_begin_unwind
   5: core::panicking::panic_fmt
   6: std::io::stdio::_eprint
   7: ruff::main
   8: std::sys_common::backtrace::__rust_begin_short_backtrace
   9: std::rt::lang_start::{{closure}}
  10: std::rt::lang_start_internal
  11: main
  12: <unknown>
  13: __libc_start_main
  14: <unknown>
```

It does seem that the server panics after receiving the exit message, because of a broken pipe

---

_Label `needs-info` removed by @MichaReiser on 2024-07-30 08:17_

---

_Label `bug` added by @MichaReiser on 2024-07-30 08:17_

---

_Comment by @MichaReiser on 2024-08-01 11:03_

I tried to reproduce this error with the following neovim configuration but without success

```lua
local lspconfig = require 'lspconfig'

vim.lsp.set_log_level(vim.env.NVIM_LSP_LOG_LEVEL or vim.lsp.log_levels.DEBUG)
require('vim.lsp.log').set_format_func(vim.inspect)

lspconfig.ruff.setup {
  cmd = { "/home/micha/astral/ruff/release/debug/ruff", "server" },
  cmd_env = { RUFF_TRACE = "verbose" },
  init_options = {
    settings = {
      logLevel = "trace",
      logFile = "~/ruff.log",
    }
  }
}
```

---

_Closed by @MichaReiser on 2024-08-02 10:10_

---

_Reopened by @MichaReiser on 2024-08-02 10:10_

---

_Comment by @MichaReiser on 2024-08-02 10:11_

I merged a fix for the panic. Let me know if you keep seeing the core dump messages after upgrading.

And thanks again for all your help with providing crash reports! 

---

_Closed by @MichaReiser on 2024-08-02 10:11_

---

_Comment by @Fissium on 2024-08-03 12:09_

@MichaReiser After upgrading to 0.5.6, I don't see any core dump messages. Thank you!

---

_Closed by @dhruvmanila on 2024-08-07 02:57_

---
