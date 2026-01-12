```yaml
number: 12108
title: "\"uv tool run\" swallows SIGINT without forwarding to the underlying process"
type: issue
state: closed
author: gibsondan
labels:
  - bug
  - question
assignees: []
created_at: 2025-03-11T01:27:18Z
updated_at: 2025-06-11T13:28:36Z
url: https://github.com/astral-sh/uv/issues/12108
synced_at: 2026-01-12T16:00:55Z
```

# "uv tool run" swallows SIGINT without forwarding to the underlying process

---

_@gibsondan_

### Summary

To repro:

- run:
```
uv tool run --with dagster-webserver dagster dev --empty-workspace
```
Leave it running.

- Find the pid of the process that is running using ps aux

- Run:
```
kill -2 <that pid>
```
- See no reaction from the process even though it is set up to handle SIGINTs.


- Running the same command out of uv, like so:
```
dagster dev --empty-workspace
```

Results in a process that can be cleanly interrupted using 'kill -2'.


I see a similar issue for uv run: https://github.com/astral-sh/uv/issues/6724 - but nothing for uv tool run.


### Platform

Darwin 23.6.0 arm64

### Version

uv 0.6.5 (bcbcd0a1e 2025-03-06)

### Python version

Python 3.11.9

---

_Label `bug` added by @gibsondan on 2025-03-11 01:27_

---

_Comment by @gibsondan on 2025-03-11 01:28_

SIGTERM (kill -15) seems to work, interestingly.

---

_Comment by @zanieb on 2025-03-11 17:27_

See https://github.com/astral-sh/uv/blob/e7fc05e4602b5335dda4b03128b9625452b41fd1/crates/uv/src/commands/run.rs#L11-L24

- https://github.com/astral-sh/uv/pull/11009

This is "intentional" right now. It'd be helpful to hear more about the motivating use-case.

---

_Comment by @gibsondan on 2025-03-11 18:01_

We have a CLI command that programatically launches "uv tool run" in a subprocess and then tries to terminate it when the process is terminated. When running in the terminal, CTRL-C works fine. When testing the command in CI/CD, there's no CTRL-C equivalent, so the command hangs forever.

I've run into the problem described into that comment myself and agree that it's very annoying that you can't tell the terminal not to send CTRL-C to subprocesses.

---

_Comment by @zanieb on 2025-03-11 19:37_

Yeah I'm not sure what to do here, other than suggest SIGINT twice or SIGTERM.

We could add TTY detection and toggle based on that? I'm not sure if that will be reliable, I'd have to give it a try.

---

_Label `question` added by @zanieb on 2025-03-11 19:37_

---

_Comment by @bulletmark on 2025-03-23 00:27_

If I run `marimo`, e.g. `marimo tutorial intro`, then I can stop it easily by typing ctrl+c twice quickly, once to initiate termination and another to not bother answering the subsequent `Are you sure you want to quit? (y/n)` prompt.

However, if I run `uvx marimo tutorial intro` then the second cntl+c just hangs and I have to kill the `uv tool` process manually which I am guessing is related to this issue?

Edit: to be clear, I am guessing the second ctrl+c is not being passed on?

---

_Comment by @zanieb on 2025-03-23 01:39_

Huh

```
DEBUG Spawned child 3116 in process group 2547

        Edit intro.py in your browser 📝

        ➜  URL: http://localhost:2718?access_token=WjuwVKsg1OgPkqRq0Eo0CA

^CDEBUG Received SIGINT, assuming the child received it as part of the process group
        Are you sure you want to quit? (y/n): 
DEBUG Received SIGINT, forwarding to child at 3116
DEBUG Received SIGINT, forwarding to child at 3116

Aborted!
^CDEBUG Received SIGINT, forwarding to child at 3116
^CDEBUG Received SIGINT, forwarding to child at 3116
^CDEBUG Received SIGINT, forwarding to child at 3116
^CDEBUG Received SIGINT, forwarding to child at 3116
^CDEBUG Received SIGINT, forwarding to child at 3116
```

This seems like it might be a problem with `marimo`? Even if I do `kill -2 3116` it doesn't exit. I have to `kill -9` the marimo process. I don't see anything obvious in their code though https://github.com/marimo-team/marimo/blob/984af117b0ec03f8f314eb571b77182f0c17fe8a/marimo/_server/api/interrupt.py#L44-L75

With their development mode, I can see a little more logs..


```
^CDEBUG Received SIGINT, assuming the child received it as part of the process group
        Are you sure you want to quit? (y/n): DEBUG Received SIGINT, forwarding to child at 27319
DEBUG Received SIGINT, forwarding to child at 27319
INFO:     connection closed
[D 250322 21:48:00 ws:735] Websocket terminated with CancelledError
[D 250322 21:48:00 auth:37] Validating auth
INFO:     127.0.0.1:58565 - "GET /api/usage HTTP/1.1" 200 OK

Aborted!
^CDEBUG Received SIGINT, forwarding to child at 27319
```

But it's still not clear where it's blocked. So.. a debugger I guess?

```
❯ lldb -p 39324
(lldb) process attach --pid 39324
Process 39324 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
    frame #0: 0x000000019bdb0440 libsystem_kernel.dylib`__wait4 + 8
libsystem_kernel.dylib`__wait4:
->  0x19bdb0440 <+8>:  b.lo   0x19bdb0460    ; <+40>
    0x19bdb0444 <+12>: pacibsp 
    0x19bdb0448 <+16>: stp    x29, x30, [sp, #-0x10]!
    0x19bdb044c <+20>: mov    x29, sp
Executable module set to "/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13".
Architecture set to: arm64-apple-macosx-.
(lldb) l
(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
  * frame #0: 0x000000019bdb0440 libsystem_kernel.dylib`__wait4 + 8
    frame #1: 0x0000000101af40e8 libpython3.13.dylib`os_waitpid + 92
    frame #2: 0x00000001019708ec libpython3.13.dylib`_PyEval_EvalFrameDefault + 48020
    frame #3: 0x0000000101aad01c libpython3.13.dylib`atexit_callfuncs.llvm.15780486606463704554 + 100
    frame #4: 0x0000000101ad3110 libpython3.13.dylib`_Py_Finalize.llvm.7324148869256557854 + 176
    frame #5: 0x0000000101b2409c libpython3.13.dylib`Py_Exit + 64
    frame #6: 0x0000000101b23ffc libpython3.13.dylib`handle_system_exit + 32
    frame #7: 0x0000000101b23ca0 libpython3.13.dylib`_PyErr_PrintEx.llvm.1117032873449786082 + 52
    frame #8: 0x0000000101b42ff8 libpython3.13.dylib`_PyRun_SimpleFileObject + 416
    frame #9: 0x0000000101b42b64 libpython3.13.dylib`_PyRun_AnyFileObject + 80
    frame #10: 0x0000000101b4216c libpython3.13.dylib`pymain_run_file_obj + 164
    frame #11: 0x0000000101b41fb8 libpython3.13.dylib`pymain_run_file + 72
    frame #12: 0x0000000101b4094c libpython3.13.dylib`Py_RunMain + 956
    frame #13: 0x0000000101b403d4 libpython3.13.dylib`pymain_main + 468
    frame #14: 0x0000000101b401f4 libpython3.13.dylib`Py_BytesMain + 40
    frame #15: 0x000000019ba5f154 dyld`start + 2476
(lldb) thread list
Process 39324 stopped
* thread #1: tid = 0x35c4149, 0x000000019bdb0440 libsystem_kernel.dylib`__wait4 + 8, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
(lldb) q
Quitting LLDB will detach from one or more processes. Do you really want to proceed: [Y/n] y
```

So it does appear to be exiting. `os_waitpid` is suspicious. Let's kill the children...

```
❯ pstree 39323
-+= 39323 zb /Users/zb/.cargo/bin/uv tool uvx --offline -vv marimo tutorial intro
 \-+- 39324 zb /Users/zb/.cache/uv/archive-v0/b-WyXMaqxa_OUjxyhedhR/bin/python /Users/zb/.cache/uv/archive-v0
   |--= 39398 zb /Users/zb/.cache/uv/archive-v0/b-WyXMaqxa_OUjxyhedhR/bin/python -c from multiprocessing.spaw
   \--- 39397 zb /Users/zb/.cache/uv/archive-v0/b-WyXMaqxa_OUjxyhedhR/bin/python -c from multiprocessing.reso
❯ kill 39397 39398
```

Now it exits

```
^CDEBUG Received SIGINT, assuming the child received it as part of the process group
        Are you sure you want to quit? (y/n): DEBUG Received SIGINT, forwarding to child at 39324
DEBUG Received SIGINT, forwarding to child at 39324

Aborted!
DEBUG Command exited with code: 1
```

I suspect they're not cleaning up these children

FWIW Ctrl-C then Ctrl-D works for me.

---

_Comment by @bulletmark on 2025-03-23 03:49_

Thanks zanieb. ctrl-c + ctrl-d does work but is slightly less convenient then two ctrl-c's in a row and somewhat side-steps the issue because it avoids the 2nd interrupt. I will raise a bug on `marimo` but can you first comment on why/how this could work when done in `marimo` normally, and only fails when run under `uv`?

---

_Comment by @zanieb on 2025-03-23 21:32_

I don't know. My best guess is that it's something like...

When you use marimo directly and press Ctrl-C, the shell sends a SIGINT to the process group. Marimo captures this, and when sent again, it exits.

When you use uv, the shell sends a SIGINT to the process group — uv ignores it since the shell has sent it to Marimo as part of the process group. When you press Ctrl-C again, the same thing happens — but uv sees this repeated attempt and forwards the SIGINT as well. Now, Marimo receives a _third_ SIGINT which breaks its interrupt handler? 

You can actually test this theory by bumping our SIGINT forwarding count...

```diff
diff --git a/crates/uv/src/commands/run.rs b/crates/uv/src/commands/run.rs
index 8aaa31b56..a26d25012 100644
--- a/crates/uv/src/commands/run.rs
+++ b/crates/uv/src/commands/run.rs
@@ -100,7 +100,7 @@ pub(crate) async fn run_to_completion(mut handle: Child) -> anyhow::Result<ExitS
                     // If the pgid _differs_ from the parent, the child will not receive a SIGINT
                     // and we should forward it. If we've received multiple SIGINTs, forward it
                     // regardless.
-                    if child_pgid == parent_pgid && sigint_count < 2 {
+                    if child_pgid == parent_pgid && sigint_count < 5 {
                         debug!("Received SIGINT, assuming the child received it as part of the process group");
                         continue;
                     }
```

With this, Marimo consistently exits as I would expect. Interestingly changing it to `< 3` doesn't work because the second Ctrl-C appears to send multiple signals (on my shell)

e.g., this is two Ctrl-C sends

```
DEBUG Spawned child 35957 in process group 35922

        Edit intro.py in your browser 📝

        ➜  URL: http://localhost:2719?access_token=dpcjotKtPa4AvK_kZQsauw

        Are you sure you want to quit? (y/n): 
DEBUG Received SIGINT, assuming the child received it as part of the process group
DEBUG Received SIGINT, assuming the child received it as part of the process group
DEBUG Received SIGINT, assuming the child received it as part of the process group

ERROR:    Cancel 0 running task(s), timeout graceful shutdown exceeded

        Thanks for using marimo! 🌊🍃

DEBUG Command exited with code: 0
```

---

_Comment by @zanieb on 2025-03-23 21:35_

It's hard to say what we could do to improve this here, since there is an expectation that we _will_ forward the signal eventually — otherwise it will never be received if the parent is not a shell. Regardless, the main problem is that when Marimo receives multiple interrupts during shutdown it can get stuck waiting for its children.

---

_Comment by @djcopley on 2025-05-22 18:02_

@zanieb I'm also running into issues with the current behavior. I'm building an interactive CLI that I'd like to run using uv run during development, but the way SIGINT is handled is causing problems.

Specifically, my CLI catches the first Ctrl+C and prompts the user to confirm they want to quit. Pressing Ctrl+C a second time is meant to force exit the app. However, when using uv run, the second SIGINT is duplicated, which causes the app to exit immediately without respecting the confirmation prompt.

I'd appreciate any guidance or workaround, and would love to see more consistent signal forwarding in uv run for use cases like this. I considered having my application change it's process group on startup, but I'd ideally like to avoid hacking runner-specific logic into my application.

For my use case, I honestly prefer that uv didn't forward any signals that my application is already getting from the shell.

---

_Comment by @zanieb on 2025-05-22 18:23_

> Specifically, my CLI catches the first Ctrl+C and prompts the user to confirm they want to quit. Pressing Ctrl+C a second time is meant to force exit the app. However, when using uv run, the second SIGINT is duplicated, which causes the app to exit immediately without respecting the confirmation prompt.

I don't quite follow, the flow should be

Ctrl-C -> Shell sends SIGINT, uv assumes the shell has sent it and does nothing
Your app prompts for confirmation
Ctrl-C -> Shell sends SIGINT, uv sends another SIGINT

> For my use case, I honestly prefer that uv didn't forward any signals that my application is already getting from the shell.

Do you have any ideas for how uv can detect it's running in a shell that will forward SIGINT automatically?

---

_Comment by @djcopley on 2025-05-22 18:58_

> I don't quite follow, the flow should be
> 
> Ctrl-C -> Shell sends SIGINT, uv assumes the shell has sent it and does nothing Your app prompts for confirmation Ctrl-C -> Shell sends SIGINT, uv sends another SIGINT

If they opt "no", the next ctrl c won't prompt, it will just immediately exit. Additionally, several other normal interactions use ctrl+c. For example, I integrate readline reverse search (ctrl+r) in my cli, and to cancel reverse search ctrl+c is used.

> Do you have any ideas for how uv can detect it's running in a shell that will forward SIGINT automatically?

What about checking the $SHELL env var? This might yield false negatives depending on the environment, but I feel like that is preferable over a false positive, and an improvement over the current behavior.

---

_Comment by @zanieb on 2025-05-22 19:13_

> If they opt "no", the next ctrl c won't prompt, it will just immediately exit. 

Thanks, that makes sense.

If we do not forward SIGINT when there is not a shell, the behavior is that you _cannot_ kill the process with SIGINT, which is worse than sending it more than once imo.

---

_Comment by @djcopley on 2025-05-22 19:50_

> If we do not forward SIGINT when there is not a shell, the behavior is that you cannot kill the process with SIGINT, which is worse than sending it more than once imo.

Yeah I do agree with this. Which is why I said a false negative ($SHELL is not set even though you are actually in a shell) is preferable to the inverse. It's probably fairly safe to assume that $SHELL won't be set if you are not in a shell. Are there any known cases where this is not safe?

An alternate approach I considered mentioning was checking the parent process against known shells.

pstree of a uv run
```
 | |-+= 14157 root login -fp djcopley
 | | \-+= 14159 djcopley -zsh
 | |   \-+= 14465 djcopley uv run -p 3.13 python -c import time; time.sleep(10)
``` 

This approach doesn't rely on the $SHELL convention, but may have its own set of drawbacks. 

---

_Comment by @kamitchell on 2025-05-31 20:35_

Please accept the commentary of this new `uv` user who thinks he has some insight.

I think the relevant factor is not "there is a shell", but only has to do with process groups and the controlling terminal. There's also mention that the shell forwards SIGINT, when in fact it's the terminal driver that sends SIGINT to the foreground process group if ^C is pressed. "foreground process group" because there can be more than one process group attached to the terminal due to forking and job control, but only one process group gets terminal input at a time. (This is all kind of magic *ix kernel stuff that's been progressively refined since being introduced in Seventh Edition Unix in 1979 by Bell Labs, and refined in 4.2BSD in 1983).

I think this is where it starts to go somewhat awry:

```
 // ...If a signal is sent directly to the uv parent process 
 // (e.g., `kill -2 <pid>`), the process group is not involved and a signal is not sent to the 
 // child by default. In this context, uv must forward the signal to the child. We work around 
 // this by forwarding SIGINT if it is received more than once.
```

I don't think it's the job of `uv` to forward SIGINT. If something wanted to kill the entire process group, it should use the `killpg` syscall.

SIGINT (^C) is special because it's one of the keyboard interrupts sent to the foreground process group that's attached to a terminal, the others are SIGTSTP (^Z) and SIGQUIT (^\). Sending a _programmatic_ kill with SIGINT is unusual, and if some program wants to _simulate_ the user pressing ^C, it should send to the whole process group like the terminal driver does, and then this workaround is not necessary.

Usually if some actor wants to _programatically_ terminate a process, SIGTERM is the default.

It looks like `uv` heads up its own process group, which means that interactive programs it runs will get ^C/SIGINT automatically. I don't know if `uv` makes the new process group or if it's a feature of the shell, but that new process group is what makes ^C just SIGINT `uv` and the tools it's running, and not _everything_ in the login session. The user presses ^C, `uv` and the processes in its process group get SIGINT, the shell notices that `uv` exited, and gives another prompt.

```
❯ uvx python
Python 3.11.4 (main, Aug 20 2023, 12:51:29) [Clang 14.0.3 (clang-1403.0.22.14.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.system("bash")
bash-3.2$ ps -O ppid,pgid
  PID  PPID  PGID   TT  STAT      TIME COMMAND
33840 33832 33840 s000  S      0:00.66 zsh
35216 33840 35216 s000  S      0:00.02 /Users/kevin/.local/bin/uv tool uvx python
35243 35216 35216 s000  S      0:00.02 python
35251 35243 35251 s000  S      0:00.02 bash
```

See: https://en.wikipedia.org/wiki/Process_group

---

_Comment by @zanieb on 2025-06-02 18:24_

Thanks for the commentary!

> I think the relevant factor is not "there is a shell", but only has to do with process groups and the controlling terminal. There's also mention that the shell forwards SIGINT, when in fact it's the terminal driver that sends SIGINT to the foreground process group if ^C is pressed.

I think this is a good clarification — I can update the documentation accordingly. I don't think there's really an implication here, just improper terminology, yes?

> I don't think it's the job of uv to forward SIGINT. If something wanted to kill the entire process group, it should use the killpg syscall.

I may agree with you philosophically here, but this change was motivated by a real world use-case in https://github.com/astral-sh/uv/issues/10952 and I think it is actually quite important that uv exits there.

> Sending a programmatic kill with SIGINT is unusual, and if some program wants to simulate the user pressing ^C, it should send to the whole process group like the terminal driver does, and then this workaround is not necessary.

I agree programmatic kills should send a SIGINT to the whole group. However, this workaround _is_ necessary for the case above still.

We actually aren't getting complaints about the process group behavior, are you having real problems with that?


---

_Closed by @zanieb on 2025-06-11 13:28_

---
