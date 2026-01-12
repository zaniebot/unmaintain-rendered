```yaml
number: 2727
title: Stop all threads and write ANSI-Reset on Ctrl-C
type: pull_request
state: closed
author: th1000s
labels: []
assignees: []
draft: true
base: master
head: ctrlc_handler
created_at: 2024-02-01T00:21:28Z
updated_at: 2025-07-11T19:53:52Z
url: https://github.com/BurntSushi/ripgrep/pull/2727
synced_at: 2026-01-12T18:23:14Z
```

# Stop all threads and write ANSI-Reset on Ctrl-C

---

_@th1000s_

On Ctrt-C stop all threads currently writing to the terminal, ignoring whatever they have locked, and write the ANSI-Reset code directly to the terminal, then exit.

This uses `pthread_kill()` or `SuspendThread()`, then `libc::write()` or `WriteConsoleA()` on Unix or Windows, respectively.

-------

So far only tested and enabled on Linux, macOS and Windows (cmd.exe, Powershell, Mingw64), but I am sure this will
work on the BSDs and several others which have `pthread_kill()` - but I'd rather have someone confirm this first.

And to compare the before/after, running `rg` with a `-j`  of 7, 11, 22, 33.. etc. will disable this handler.

---

_Comment by @th1000s on 2024-02-01 00:33_

When suggesting to someone to use `rg` instead of "legacy" `grep -R .. | grep -v -e .git -e build` or `git grep ..`
on the _very first try_ (maybe spooked by the speed of the output) after ^C the terminal remained blood red and I had a slightly embarrassed look on my face.

A lot of users have shells or prompts which handle this automatically. But now that `rg` is packaged by most Linux distros, more users which don't customize their shells use it, and first impressions matter as to not end up as "newfangled nonsense" :)

---

_Comment by @BurntSushi on 2024-02-01 01:06_

I've only skimmed this PR so far, but there is a ton of code here. And it also introduces a new dependency on `winapi`, which [I hope to be getting rid of soon](https://github.com/BurntSushi/winapi-util/pull/13).

Separate from that, ripgrep used to have handling like this, but it ended up offering a worse user experience. Namely, `^C` wouldn't always terminate ripgrep. And I believe it worked very poorly on Windows.

> A lot of users have shells or prompts which handle this automatically.

I just use a simple little script to fix coloring if and when it happens (which is rare): https://github.com/BurntSushi/dotfiles/blob/eb14900839a210f31b826f0ab070506528ea6a99/bin/reset-term

> But now that rg is packaged by most Linux distros

This is not a recent development. This has been true for years.

---

_Comment by @th1000s on 2024-02-02 11:50_

> `winapi` -> `windows-sys`

Both crates work fine, I added a commit which uses `windows-sys` instead.

> Separate from that, ripgrep used to have handling like this, but it ended up offering a worse user experience. Namely, `^C` wouldn't always terminate ripgrep. And I believe it worked very poorly on Windows.

I've seen the previous behavior, this is fixed in this PR. It does this by (safely) ignoring any held locks which could slow down a fast exit, so a few syscalls have to be issued directly.

> [rg packaged by most Linux distros is] not a recent development. This has been true for years.

I must be thinking in Debian releases, there it has only been included for one or two releases I think :)

---

_Comment by @BurntSushi on 2024-02-02 12:21_

ripgrep has been in Debian for years.

I'm not sure if I'm going to take this. It is so much code to solve an issue that I don't think is a big deal.

---

_Comment by @BurntSushi on 2025-07-11 19:53_

I'm going to close this. I don't think my opinion has changed since the above. I've tried this in the past, and it just lead to worse problems. 500 lines of code to deal with this just isn't worth it IMO.

---

_Closed by @BurntSushi on 2025-07-11 19:53_

---
