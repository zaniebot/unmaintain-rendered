```yaml
number: 1291
title: "doesn't work in a nonexistent dir"
type: issue
state: closed
author: torkve
labels:
  - bug
assignees: []
created_at: 2019-06-04T17:51:18Z
updated_at: 2020-02-17T22:16:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1291
synced_at: 2026-01-12T16:13:23Z
```

# doesn't work in a nonexistent dir

---

_@torkve_

#### What version of ripgrep are you using?

```
ripgrep 11.0.1
+SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`cargo install ripgrep`

#### What operating system are you using ripgrep on?

Ubuntu 19.04

#### Describe your question, feature request, or bug.

When the CWD is a nonexistent directory, and ripgrep is invoked, it always fails even if target is a valid absolute path. GNU grep works in this case.

#### If this is a bug, what are the steps to reproduce the behavior?

```
0 ➜ mkdir somepath
0 ➜ cd somepath/
0 ➜ rm -rf ../somepath/
0 ➜ rg something /etc/hostname 
No such file or directory (os error 2)
2 ➜ 
```

According to strace, ripgrep invokes getcwd() at some point and fails with it.

#### If this is a bug, what is the actual behavior?

```
0 ➜ RIPGREP_CONFIG_PATH= rg --debug something /etc/hostname 
DEBUG|grep_regex::literal|/home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-regex-0.1.3/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(something)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.3/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
No such file or directory (os error 2)
```

#### If this is a bug, what is the expected behavior?

I expect ripgrep to fail only in cases when any relative paths are used, while the only-absolute-paths command should be successfully executed.

---

_Comment by @BurntSushi on 2019-06-04 17:56_

Could you please describe the use case for running ripgrep in a directory that doesn't exist?

---

_Comment by @torkve on 2019-06-04 18:03_

I just cd'ed into some temporary directory, created by my tests runner application (running somewhere in the background). And then I was doing some debugging.
When tests runner finished its work, it just removed that temporary directory, so it became nonexistent.

---

_Comment by @torkve on 2019-06-04 18:07_

I may agree to some extent that this case is not very typical, and probably one should watch the directory they work in, and do not execute commands in some their random terminal session :)
But even with that, the error misguides because user assumes that problem is with the file, not the cwd.

---

_Label `bug` added by @BurntSushi on 2019-07-11 11:25_

---

_Comment by @afrubin on 2019-08-28 23:52_

> Could you please describe the use case for running ripgrep in a directory that doesn't exist?

I encountered this bug because I was in a network mounted directory but the network connection (OneDrive via rclone in this case) had been closed.

It did cause quite a bit of confusion until a colleague suggested I check the working directory. This had not occurred to me because I was searching a completely different part of the file system.

---

_Comment by @yump on 2019-11-03 07:33_

I encountered this because I was sitting in `/proc/$some_app_pid`, rg-ing through `smaps`. I restarted `some_app`, and thought it would suffice to rg `/proc/$(pgrep some_app)/smaps`, but instead got this confusing error, "No such file or directory (os error 2)".

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
