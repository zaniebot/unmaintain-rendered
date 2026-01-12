```yaml
number: 1308
title: System immediately freezes on keyboard interrupt / kill
type: issue
state: closed
author: Ethan826
labels:
  - question
assignees: []
created_at: 2019-06-20T16:00:55Z
updated_at: 2019-06-25T16:55:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1308
synced_at: 2026-01-12T16:13:23Z
```

# System immediately freezes on keyboard interrupt / kill

---

_@Ethan826_

#### What version of ripgrep are you using?

```
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Using `cargo`, but I also got the same error when running the `brew` version.

#### What operating system are you using ripgrep on?

Mac OS v.10.14.5 (18F132)

#### Describe your question, feature request, or bug.

Attempting to interrupt `ripgrep` is causing my system to freeze. This is true both of `^C` keyboard interrupt and of using another terminal window to issue a `kill` command for the `ripgrep` process. The system freeze is immediate and includes the mouse, the spinner at the top of the terminal tab window, etc.

The system remained frozen for perhaps 10 minutes before I restarted it.

#### If this is a bug, what are the steps to reproduce the behavior?

This occurs on a large directory of Ruby on Rails code. I was searching for the word `hello`.

#### If this is a bug, what is the actual behavior?

It isn't possible to copy output, as the system immediately and apparently irremediably freezes.

---

_Comment by @BurntSushi on 2019-06-20 16:03_

Sorry, but this bug report is not actionable. The only way I'd even be able to begin to diagnose this is if you provide the corpus you're searching.

From experience, it kind of smells like your system is out of memory, but I am not a macOS user, so I'm not sure. You'll probably need to debug this yourself. Try looking at system stats (such as memory/CPU usage via `htop` just before the freeze). Try also running ripgrep in single threaded mode, e.g., with the `-j1` flag.

---

_Label `question` added by @BurntSushi on 2019-06-20 16:03_

---

_Comment by @Ethan826 on 2019-06-20 16:08_

I followed the guidance in the previous "My Mac is freezing" [issue](https://github.com/BurntSushi/ripgrep/issues/975), including having `htop` running. Memory usage is relatively low, and the crash is nearly instantaneous on sending the interrupt. I will see if I can get it to happen with a corpus that's not a proprietary codebase.

---

_Comment by @Ethan826 on 2019-06-20 16:44_

Was able to reproduce with
```bash
git clone https://github.com/torvalds/linux.git
cd linux
rg hello
# ^C
```

---

_Comment by @Ethan826 on 2019-06-20 20:17_

Apparently work has been pushing out BitDefender or messing with its settings in some way, which I've learned is causing coworkers issues with a variety of applications. If my issue isn't reproducible, that may be the cause. I will close the issue pending someone's reproducing it.

---

_Closed by @Ethan826 on 2019-06-20 20:17_

---

_Comment by @BurntSushi on 2019-06-20 20:27_

@Ethan826 Thanks! If you find out anything more about BitDefender or are able to draw any conclusions, then updating this issue would be great for posterity.

FWIW, I was able to checkout the Linux kernel, run your query and `^C` it successfully on my mac dev box.

---

_Comment by @Ethan826 on 2019-06-25 16:49_

The security team at work whitelisted the various install locations (i.e. `/usr/local/bin/rg`, `~/.cargo/bin/rg`, and in the Homebrew directory somewhere) in the configuration files for BitDefender, and the issue is fixed.

---

_Comment by @BurntSushi on 2019-06-25 16:54_

@Ethan826 Awesome, thanks for the follow up!

---

_Comment by @Ethan826 on 2019-06-25 16:55_

Thanks for your responsiveness and for making such a great application.

---
