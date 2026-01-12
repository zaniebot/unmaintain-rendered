```yaml
number: 19
title: Hangs when searching recursively under Cygwin
type: issue
state: closed
author: vadz
labels: []
assignees: []
created_at: 2016-09-23T16:57:51Z
updated_at: 2016-10-07T19:40:23Z
url: https://github.com/BurntSushi/ripgrep/issues/19
synced_at: 2026-01-12T18:23:11Z
```

# Hangs when searching recursively under Cygwin

---

_@vadz_

This is rather mysterious but the first thing I did after installing the Windows binary (`ripgrep-0.1.17-x86_64-pc-windows-msvc.zip`) was to run `rg fun` from Cygwin shell inside a relatively big repository and it just hung without outputting anything. Trying to search inside a single file works fine, searching in a single leaf subdirectory does too, but searching under a subdirectory containing other subdirectories outputs a number of matches and then hangs on exit. To emphasize, this only happens in a Cygwin shell, the tool doesn't hang when run from the standard Windows DOS window.

Looking at it in process explorer I see that the thread is completely stuck (0% CPU use) and the stack is

```
ntoskrnl.exe!KiSwapContext+0x7a
ntoskrnl.exe!KiCommitThreadWait+0x1d2
ntoskrnl.exe!KeWaitForSingleObject+0x19f
ntoskrnl.exe!KiSuspendThread+0x54
ntoskrnl.exe!KiDeliverApc+0x21d
ntoskrnl.exe!KiCommitThreadWait+0x3dd
ntoskrnl.exe!KeWaitForSingleObject+0x19f
ntoskrnl.exe!NtReadFile+0x8ae
ntoskrnl.exe!KiSystemServiceCopyEnd+0x13
ntdll.dll!NtReadFile+0xa
KERNELBASE.dll!ReadFile+0x76
kernel32.dll!ReadFileImplementation+0x55
rg.exe+0x124f1f
```

which doesn't make much sense to me, but maybe it can be useful to you if you have the map file and see what does `0x124f1f` offset correspond to.

I could try debugging this further later, but unfortunately I really don't have time for this now, I just wanted to quickly check a promising new tool...


---

_Comment by @BurntSushi on 2016-09-24 02:15_

If you do `rg .`, does that hang as well?

I'm wondering if this is a dupe of #27. (I have run `rg` from a cygwin shell before too, and it worked fine.)


---

_Comment by @vadz on 2016-09-24 02:19_

You're right, `rg fun .` works, so it does seem to be a (time travelling :-) dupe of #27.

I still have trouble understanding what's going on with `rg fun src` though, it's weirder than I thought: in this case the `rg` process exits, but the shell remains waiting for it somehow. Equally (more? less? I'm not sure of anything any more) weirdly, `rg -w fun src` works without any problems. And so does `rg fun src/common`.


---

_Comment by @BurntSushi on 2016-09-24 02:23_

I have no clue. I can't think of any reason why the presence or absence of `-w` would have anything to do with the termination of `rg`. :-(

I will try to fire up a cygwin shell soon and see how rg behaves.


---

_Comment by @vadz on 2016-09-24 02:26_

TIA! If you'd like to use exactly the same data as I do for reproducing the problem, I'm running the command in a checkout of [wxWidgets](https://github.com/wxWidgets/wxWidgets.git) in 32 bit Cygwin zsh.


---

_Comment by @BurntSushi on 2016-09-24 02:27_

Thank you so much for that tidbit. I've gotten so used to people running `rg` on their own private data that I didn't even think to ask. Yay!


---

_Comment by @CryZe on 2016-09-25 02:15_

Seems like I have the same problem with a msys2 shell (the workaround works for me as well).


---

_Comment by @BurntSushi on 2016-09-25 02:24_

For the folks on Windows, does `rg` work in a standard command prompt?


---

_Comment by @wjrogers on 2016-09-25 02:45_

Yes, it works normally in the Windows Command Prompt. It hangs in mintty.


---

_Comment by @BurntSushi on 2016-09-25 22:37_

Ah! I can reproduce this, yay. The reason why I hadn't seen this before was because I was SSHing into my Windows VM. If I actually open the cygwin terminal and run `rg` there, it does indeed hang.

FYI, it "hangs" because it's sitting waiting for stdin, so something is buggy in the tty detection.


---

_Comment by @BurntSushi on 2016-09-25 22:55_

So it occurs to me that I have no clue how to determine whether something is being piped to `stdin` on Windows or not. I was using [this approach](https://github.com/BurntSushi/ripgrep/blob/9395076468b7cff1421392dec118e08073b49214/src/atty.rs#L42-L52), but it seems to be reporting `stdin` as a pipe even when there isn't one.

I think what I'm going to do is switch the failure modes. That is, always assume `stdin` is a tty _on Windows_. This means the only way to search `stdin` on Windows is by specifying `-` in the file path list.


---

_Comment by @vadz on 2016-09-25 23:29_

Oh, sorry, I should have thought about this, this is actually a pretty well-known issue -- but without any good solution, unfortunately. The problem is that Windows doesn't have any concept of PTY, so when a program is running in any kind of terminal emulator, and not the standard console window, its stdin is always connected to a pipe, as far as Windows is concerned. Cygwin applications can distinguish between "real" pipes and normal input from the terminal, but I don't know of any way to do this without linking to `cygwin1.dll`.

I think you should still be able to detect whether stdin is a TTY when running inside the native console and you should be able to check for this (running inside console, I mean). But everything console-related in Windows is pretty hairy, just look at [our own code](https://github.com/wxWidgets/wxWidgets/blob/v3.1.0/src/msw/app.cpp#L288) for detecting whether we can _write_ to a console...


---

_Closed by @BurntSushi on 2016-09-26 00:00_

---

_Comment by @BurntSushi on 2016-09-26 00:02_

I've fixed this, but created a new bug in the process, under the premise that this bug is worse than #94.


---

_Comment by @silverwind on 2016-10-07 17:49_

I think it might be better to revert this fix and instruct Cygwin users to compile from source using `cargo` from the GNU build of Rust. Using native Windows binaries in Cygwin (which aims to provide POSIX compat first, Windows second) is just bad practice.

Of course, it'd also be an option to provide prebuilt Cygwin binaries to hopefully solve this.


---

_Comment by @BurntSushi on 2016-10-07 19:04_

Could you please explain why, in detail that would fix this issue? Does it actually fix it for you?

Having `rg` hang indefinitely on its _core use case_ is unacceptable. Almost any other bug (including butchering handling `stdin` automatically) is preferable.

Obviously, we'd like both things to work. As I've stated, I don't know how to do that. Others have given me some threads to pull on, but I haven't had time to look into it.


---

_Comment by @silverwind on 2016-10-07 19:40_

I'll try to build with the fix reverted, it's just a wild guess at this point.


---
