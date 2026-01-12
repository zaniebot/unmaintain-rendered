```yaml
number: 2562
title: "windows: set default path separator to '/' in MSYS"
type: pull_request
state: closed
author: EJammy
labels: []
assignees: []
base: master
head: master
created_at: 2023-07-19T02:23:11Z
updated_at: 2023-08-29T02:53:15Z
url: https://github.com/BurntSushi/ripgrep/pull/2562
synced_at: 2026-01-12T18:23:14Z
```

# windows: set default path separator to '/' in MSYS

---

_@EJammy_

Adapted from https://github.com/sharkdp/fd/pull/730
Also see https://github.com/sharkdp/fd/issues/537#issuecomment-757519135

This removes the need to use `--path-separator` for MSYS and MSYS2 environments (such as Git Bash), as per #275, but not for Cygwin (see linked pr and comment).

Not sure if this is welcomed, since it is handling a special case for specific environment. But I think this is a good improvement as it aligns with the behavior of `fd` and removes error when running commands such as `rg foobar --files-with-matches | xargs ` without `--path-separator`.

---

_Comment by @BurntSushi on 2023-07-19 14:00_

In terms of implementation, I think this logic should probably live in ripgrep proper and not inside `grep-printer`.

In terms of idea, I'm less sure about this. It seems like it's easy enough to work-around by defining an alias or a config file to set the default path separator yourself. It doesn't feel right to be doing this automatically behind your back. And this is a very subtle (IMO) change in behavior and it's not clear to me what the impact of it would be.

---

_Comment by @EJammy on 2023-07-20 03:01_

I'm aware of the workaround. I proposed this change because it took me a while to figure out the error after piping the results of `rg --files-with-matches`, while most tools under MSYS, like fd and git, "just works". I installed ripgrep through MSYS2 with `pacman -S mingw-w64-x86_64-ripgrep`, so I expected it to work well under that environment.

However, this method of installation is not documented, so I figured it might not have the best support. And I have to agree when I discover this solution it also felt a bit wrong to me too. Might be best to just leave it for the user to configure and improve documentation (#1667).

---

_Comment by @BurntSushi on 2023-07-20 13:11_

Apologies for my potential ignorance here, but could you say more about the relationship between cygwin and MSYS? In my brain, I seem to remember them being connected or the same in some way, but I find myself unable to articulate precisely their relationship.

---

_Comment by @EJammy on 2023-07-21 01:39_

I'm not an expert either. I've only recently started using MSYS2 to work in windows and have not touched cygwin before.

But from my understanding, MSYS is built on top of cygwin and provides tools to build and run native windows software. It also has a package manager (`pacman`). It's also worth noting that git bash (from git for windows) is based on MSYS2, so when people mention git bash it's just repackaged MSYS2.

There's more info on their home page: https://www.msys2.org/
Some additional differences: https://www.msys2.org/wiki/How-does-MSYS2-differ-from-Cygwin/

---

_Comment by @BurntSushi on 2023-08-29 02:53_

All righty, thanks. I think I'm going to close this then.

I'm not necessarily 100% opposed to something like this, but I just don't have a ton of visibility into its impact. And in general, I've found the whole cygwin/MSYS2 environment to be pretty difficult to fit into. The fundamental problem is that there is no cygwin Rust target, so Rust programs in cygwin don't get linked with the cygwin POSIX emulation layer (or whatever it's called). They stay native Windows programs. So it's best to think about ripgrep as _always_ a native Windows program and not something that is built for cygwin.

---

_Closed by @BurntSushi on 2023-08-29 02:53_

---
