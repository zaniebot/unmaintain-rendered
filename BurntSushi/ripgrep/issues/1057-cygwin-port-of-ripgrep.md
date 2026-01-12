```yaml
number: 1057
title: Cygwin port of ripgrep
type: issue
state: closed
author: hashimaziz1
labels:
  - question
assignees: []
created_at: 2018-09-17T18:01:32Z
updated_at: 2020-09-13T08:24:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1057
synced_at: 2026-01-12T16:13:22Z
```

# Cygwin port of ripgrep

---

_@hashimaziz1_

#### What operating system are you using ripgrep on?

Cygwin running on top of Windows 7.

#### Describe your question, feature request, or bug.

Cygwin is currently the most popular, de-facto tool for emulating a Linux/Unix-like environment on top of Windows. Having just come from and digested both the [blog post](https://blog.burntsushi.net/ripgrep/) and [Hacker News thread](https://news.ycombinator.com/item?id=12564442) from 2016, and seeing the lead dev's enthusiasm for the project, I was so sure there would already be a port to it.  

Is there any chance of this being added to the pipeline anytime soon?


---

_Comment by @BurntSushi on 2018-09-17 18:03_

The releases page provides Windows **native** binaries, and those already work on cygwin.

It would be more helpful you could please describe what problem you're trying to solve and why the current solution doesn't work.

---

_Label `question` added by @BurntSushi on 2018-09-17 18:03_

---

_Comment by @hashimaziz1 on 2018-09-17 18:48_

The problem is that `ripgrep` can't currently be installed like all other packages in Cywgin can, via [apt-cyg package manager](https://github.com/transcode-open/apt-cyg), which is an annoyance both because it means it can't be installed by the command-line and because it wouldn't be a part of the `Cygwin/bin/usr` ecosystem but the Windows one, which gets messy for installation and debugging purposes. `apt-cyg` is essentially a script that pulls packages from the sourcebase.org (developers of Cygwin) package repository. The process for submitting a package to the repository is [here](https://cygwin.com/packaging-contributors-guide.html), so once `ripgrep` is a part of that repository, it would be immediately available to install via the `apt-cyg` command. 

---

_Comment by @BurntSushi on 2018-09-17 19:22_

So it sounds like someone just needs to maintain a cygwin package and submit it? I personally don't have the bandwidth to maintain it myself but I encourage others to do so!

---

_Comment by @BurntSushi on 2018-09-17 22:53_

I'm going to close this because I don't think there is anything actionable for me to do here. If it's possible to get ripgrep into cygwin's repos as is, then that would be cool and I'd support that. However, I don't really buy that cygwin is the only thing that gives you the benefits you claim. In particular, there are other package managers on Windows that can install ripgrep for you. For example, [ripgrep is packaged in Chocolatey](https://chocolatey.org/packages/ripgrep). Moreover, my sense is that folks are slowly migrating to other ways of using *nix programs in Windows, but obviously I don't have any numbers.

While I do try to make sure the native binaries work in cygwin, they aren't perfect because ripgrep doesn't actually link with cygwin's core libraries. Fixing this is itself a rat's nest that is unlikely to ever happen. We could attempt to link with cygwin's libraries and attempt to call their functions (like for path translation) manually, or by re-implementing them ourselves. Another possibility is to port Rust's standard library to the cygwin platform, but that task is probably best measured in person months and even if someone were to do it, the entire ecosystem around Windows would also need to grow support for cygwin specifically. Not only is this a lot of effort, but it's not clear what the value of the entire enterprise is in the first place. In particular, cygwin is built to provide a POSIX environment for folks on Windows, and in particular, to enable programs written against POSIX to be able to compile and run on Windows. This isn't nearly as applicable for Rust programs because in the Rust ecosystem, POSIX is an _implementation detail_. Namely, the standard library is itself already built with first class support for Windows in mind, so there really is no need to "port" ripgrep to cygwin in order for it to run as a Windows program.

Thus, as far as I can tell, the only real reason to "port" ripgrep to cygwin is to 1) fix a couple outstanding bugs around path translation that already have at least some work arounds and 2) to make ripgrep work a bit more smoothly in the cygwin ecosystem. IMO, the juice isn't worth the squeeze, and I can't imagine spending months of my time working on that, and I don't think I could possibly recommend that anyone else do it either.

TL;DR - ripgrep already works in cygwin as a native Windows program. There are a couple outstanding bugs, some of which have been smoothed over. Some may never be fixed. Some may grow more work arounds as time and priorities dictate. But there will likely never be a true port of ripgrep to cygwin.

---

_Closed by @BurntSushi on 2018-09-17 22:53_

---

_Comment by @hashimaziz1 on 2018-09-22 18:45_

> 
> 
> I'm going to close this because I don't think there is anything actionable for me to do here. If it's possible to get ripgrep into cygwin's repos as is, then that would be cool and I'd support that. However, I don't really buy that cygwin is the only thing that gives you the benefits you claim. In particular, there are other package managers on Windows that can install ripgrep for you. For example, [ripgrep is packaged in Chocolatey](https://chocolatey.org/packages/ripgrep). Moreover, my sense is that folks are slowly migrating to other ways of using *nix programs in Windows, but obviously I don't have any numbers.
> 
> While I do try to make sure the native binaries work in cygwin, they aren't perfect because ripgrep doesn't actually link with cygwin's core libraries. Fixing this is itself a rat's nest that is unlikely to ever happen. We could attempt to link with cygwin's libraries and attempt to call their functions (like for path translation) manually, or by re-implementing them ourselves. Another possibility is to port Rust's standard library to the cygwin platform, but that task is probably best measured in person months and even if someone were to do it, the entire ecosystem around Windows would also need to grow support for cygwin specifically. Not only is this a lot of effort, but it's not clear what the value of the entire enterprise is in the first place. In particular, cygwin is built to provide a POSIX environment for folks on Windows, and in particular, to enable programs written against POSIX to be able to compile and run on Windows. This isn't nearly as applicable for Rust programs because in the Rust ecosystem, POSIX is an _implementation detail_. Namely, the standard library is itself already built with first class support for Windows in mind, so there really is no need to "port" ripgrep to cygwin in order for it to run as a Windows program.
> 
> Thus, as far as I can tell, the only real reason to "port" ripgrep to cygwin is to 1) fix a couple outstanding bugs around path translation that already have at least some work arounds and 2) to make ripgrep work a bit more smoothly in the cygwin ecosystem. IMO, the juice isn't worth the squeeze, and I can't imagine spending months of my time working on that, and I don't think I could possibly recommend that anyone else do it either.
> 
> TL;DR - ripgrep already works in cygwin as a native Windows program. There are a couple outstanding bugs, some of which have been smoothed over. Some may never be fixed. Some may grow more work arounds as time and priorities dictate. But there will likely never be a true port of ripgrep to cygwin.

Thanks for the detailed answer. Is there documentation somewhere that lists the currently active bugs that apply to Cygwin, or can I simply assume that [this is a comprehensive list of them](https://github.com/BurntSushi/ripgrep/issues?q=is%3Aissue+cygwin+is%3Aclosed+label%3Abug)?

---

_Comment by @BurntSushi on 2018-09-22 21:41_

@Kaos-Industries Possibly. Off the top of my head, the primary bug impacting ripgrep in cygwin environment is path translation. That is, ripgrep does not understand cygwin paths, so if you hand it a path that is a cygwin path but not a normal Windows path, then ripgrep won't understand it. This can be disorienting to users because there exists a common subset of cygwin and Windows paths that have the same semantics. Moreover, cygwin generally tries to make sure _any_ Windows or cygwin path "just works," so when ripgrep trips over a cygwin-only path, it is confusing.

---

_Comment by @pepoluan on 2020-09-13 08:24_

> @Kaos-Industries Possibly. Off the top of my head, the primary bug impacting ripgrep in cygwin environment is path translation. That is, ripgrep does not understand cygwin paths, so if you hand it a path that is a cygwin path but not a normal Windows path, then ripgrep won't understand it. This can be disorienting to users because there exists a common subset of cygwin and Windows paths that have the same semantics. Moreover, cygwin generally tries to make sure _any_ Windows or cygwin path "just works," so when ripgrep trips over a cygwin-only path, it is confusing.

If I may add a bit, probably wrap calls to ripgrep using a bash/zsh function, like the [recommendation for the `bat` utility](https://github.com/sharkdp/bat#cygwin). In essence: Loop over all parameters, and if it's not a switch _and_ a path, pass it through the `cygpath` tool to get the Windows path.

---
