```yaml
number: 3029
title: Ripgrep can become unresponsive to even ctrl-c for some time when ran with permission issues
type: issue
state: closed
author: Porkepix
labels:
  - duplicate
assignees: []
created_at: 2025-04-11T12:48:13Z
updated_at: 2025-10-22T14:22:49Z
url: https://github.com/BurntSushi/ripgrep/issues/3029
synced_at: 2026-01-12T16:13:25Z
```

# Ripgrep can become unresponsive to even ctrl-c for some time when ran with permission issues

---

_@Porkepix_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

14.1.1

### How did you install ripgrep?

Arch's repos

### What operating system are you using ripgrep on?

ArchLinux

### Describe your bug.

Running ripgrep to match something from the root cause numerous issues which are at least for some normal and expected, at least for the first kind of this list:
- `Permission denied (os error 13)`
- `Invalid argument (os error 22)` in `/proc` locations

### What are the steps to reproduce the behavior?

Run `rg "am_pm" /` for example, then try to cancel it with `ctrl-c`.

### What is the actual behavior?

It hangs for dozens of seconds before `ctrl-c` actually kills it and we get our prompt back.

### What is the expected behavior?

It shouldn't hang on the `ctrl-c` and we should get our prompt back immediately.
Curiously enough, if the exact same command is ran with `sudo`, then`ctrl-c` acts instantly and we get no hangs at all.

To note that it also hangs for a long time after no more errors are printed and it seemingly ended the search before we finally get a prompt back.

---

_Comment by @BurntSushi on 2025-04-11 12:57_

Duplicate of #2897, #1331, #311

Possibly related: #1145, #873

This isn't something that ripgrep can control. ripgrep doesn't even try to handle `^C`, and any unhandled signal means that the OS will attempt to kill the process. But there are lots of reasons why a process can't easily be stopped.

Search `/`, and particularly including special files like `/proc` and what not, is bad juju. In some cases, even just _reading_ files in those directories will have side effects on your system. And particularly so when running as `root`.

---

_Closed by @BurntSushi on 2025-04-11 12:57_

---

_Label `duplicate` added by @BurntSushi on 2025-04-11 12:57_

---

_Comment by @Porkepix on 2025-04-11 13:00_

> This isn't something that ripgrep can control. ripgrep doesn't even try to handle `^C`, and any unhandled signal means that the OS will attempt to kill the process. But there are lots of reasons why a process can't easily be stopped.
> 
> Search `/`, and particularly including special files like `/proc` and what not, is bad juju. In some cases, even just _reading_ files in those directories will have side effects on your system. And particularly so when running as `root`.

But then why the difference of treatment between when it's run by `sudo` or not?

I get that it might not be recommended, but it's sometimes useful when you don't remember where something can be, and `GNU grep` seems to handle it without pain, aside the expected permissions errors?

---

_Comment by @BurntSushi on 2025-04-11 13:23_

Because `sudo` impacts how those files are read. Reconsider what I said above: the mere _act_ of reading some files has side effects, and permissions can impact how those files are read. I cannot _possibly_ give you a precise answer here because there's no MRE. If I run your commands on one of my systems, it completes after 30 seconds without problems. And I can `^C` at any point and it terminates almost instantly. On the other hand, if I run `sudo rg am_pm /`, then the search is still running minutes later. And note that I had to unmount my network shares before doing this test, because other ripgrep would just search over literal terabytes from my NAS. And in my experience, network I/O can absolutely cause the kind of hanging you're seeing. 

GNU grep falls over too, depending on what you're doing:

```
$ time grep -a ZQZQZQZQ /proc/self/pagemap
zsh: killed     grep --color=auto -a ZQZQZQZQ /proc/self/pagemap

real    4.460
user    1.321
sys     3.123
maxmem  6685 MB
faults  0
$ sudo dmesg | rg -i oom
[137771.992498] grep invoked oom-killer: gfp_mask=0x140dca(GFP_HIGHUSER_MOVABLE|__GFP_COMP|__GFP_ZERO), order=0, oom_score_adj=0
[137771.992522]  oom_kill_process.cold+0x8/0x8a
[137771.992676] [  pid  ]   uid  tgid total_vm      rss rss_anon rss_file rss_shmem pgtables_bytes swapents oom_score_adj name
[137771.992800] oom-kill:constraint=CONSTRAINT_NONE,nodemask=(null),cpuset=/,mems_allowed=0,global_oom,task_memcg=/user.slice/user-1000.slice/session-3.scope,task=grep,pid=49358,uid=1000
[137771.992814] Out of memory: Killed process 49358 (grep) total-vm:9476316kB, anon-rss:6845824kB, file-rss:444kB, shmem-rss:0kB, UID:1000 pgtables:13460kB oom_score_adj:0
```

Maybe multi-threading makes ripgrep use more memory, to the point that it grinds your system to a hault. It's very hard to say without being able to stand over your shoulder. You could try running with `rg -j1` to test that theory.

An MRE here means isolating the problematic files.

> but it's sometimes useful when you don't remember where something can be

The linked issues give you work-arounds via `.rgignore` files. I assume that what you're looking for is known not to be in special directories like `/sys`, `/proc` and `/dev`. If so, then you should exclude those directories and you should be fine.

---

_Comment by @Porkepix on 2025-04-11 13:29_

> Because `sudo` impacts how those files are read. Reconsider what I said above: the mere _act_ of reading some files has side effects, and permissions can impact how those files are read. I cannot _possibly_ give you a precise answer here because there's no MRE. If I run your commands on one of my systems, it completes after 30 seconds without problems. And I can `^C` at any point and it terminates almost instantly. On the other hand, if I run `sudo rg am_pm /`, then the search is still running minutes later. And note that I had to unmount my network shares before doing this test, because other ripgrep would just search over literal terabytes from my NAS. And in my experience, network I/O can absolutely cause the kind of hanging you're seeing.
> 
> GNU grep falls over too, depending on what you're doing:
> 
> ```
> $ time grep -a ZQZQZQZQ /proc/self/pagemap
> zsh: killed     grep --color=auto -a ZQZQZQZQ /proc/self/pagemap
> 
> real    4.460
> user    1.321
> sys     3.123
> maxmem  6685 MB
> faults  0
> $ sudo dmesg | rg -i oom
> [137771.992498] grep invoked oom-killer: gfp_mask=0x140dca(GFP_HIGHUSER_MOVABLE|__GFP_COMP|__GFP_ZERO), order=0, oom_score_adj=0
> [137771.992522]  oom_kill_process.cold+0x8/0x8a
> [137771.992676] [  pid  ]   uid  tgid total_vm      rss rss_anon rss_file rss_shmem pgtables_bytes swapents oom_score_adj name
> [137771.992800] oom-kill:constraint=CONSTRAINT_NONE,nodemask=(null),cpuset=/,mems_allowed=0,global_oom,task_memcg=/user.slice/user-1000.slice/session-3.scope,task=grep,pid=49358,uid=1000
> [137771.992814] Out of memory: Killed process 49358 (grep) total-vm:9476316kB, anon-rss:6845824kB, file-rss:444kB, shmem-rss:0kB, UID:1000 pgtables:13460kB oom_score_adj:0
> ```
> 
> Maybe multi-threading makes ripgrep use more memory, to the point that it grinds your system to a hault. It's very hard to say without being able to stand over your shoulder. You could try running with `rg -j1` to test that theory.
> 
> An MRE here means isolating the problematic files.
> 
> > but it's sometimes useful when you don't remember where something can be
> 
> The linked issues give you work-arounds via `.rgignore` files. I assume that what you're looking for is known not to be in special directories like `/sys`, `/proc` and `/dev`. If so, then you should exclude those directories and you should be fine.

Just to understand, what does MRE stands for?

Because I think there might have been a misunderstanding here, it doesn't put my whole system to a halt, it just hangs the terminal running the command that doesn't respond instantly to `ctrl-c`.

I had no network storage attached, so this is out of the equation, however `j1` indeed result in `ctrl-c` being handled instantly.

I'll look at the `.rgignore` file, didn't knew this was a thing now!

---

_Comment by @BurntSushi on 2025-04-11 13:52_

For MRE, see <https://en.wikipedia.org/wiki/Minimal_reproducible_example> and <https://stackoverflow.com/help/minimal-reproducible-example>

In this case, an MRE would probably mean isolating the specific directories causing problems. And this case is particularly gnarly because that might not even be possible. What you might be seeing is only possible through an aggregate effect.

> Because I think there might have been a misunderstanding here, it doesn't put my whole system to a halt, it just hangs the terminal running the command that doesn't respond instantly to `ctrl-c`.

No I get that. The imprecision of my response is an artifact of 1) others experience overall system hangs when searching `/` and 2) the inability to know _precisely_ know what is happening in your situation. Think of it like a [diagnosis of exclusion](https://en.wikipedia.org/wiki/Diagnosis_of_exclusion). We could go back-and-forth all day with theories I have and you saying, "well no, my specific situation doesn't have that." (e.g., No network I/O.)

The actual bottom line here this:

1. It is known that searching special directories like `/proc`, `/sys` and `/dev` is problematic. You might get OOMed. It might run forever. It might grind your system to a hault. It might even _reboot_ your system!
2. The best way to avoid those problems, in general, is to exclude them from your search.

---

_Comment by @Porkepix on 2025-10-21 23:35_

@BurntSushi I realize I've let this unanswered, first thanks for explanations (and I still have to look for the `.rgignore` way of managing this.

But I  wondered if maybe it was considered at some point in the past if it'd be relevant for these special directories to get the same treatment as hidden directories or `.gitignore` content and ignore these by default, unless user explicitly request otherwise through flags/config? Cases were people would went to search in these are probably quite rares I'd say?

Also, as I wanted to check if either one of these three would be the real culprit, I just noticed I can't reproduce the issue anymore, whether it's by some system changes or due to the newest `ripgrep 15.0.0`!

---

_Comment by @BurntSushi on 2025-10-22 12:59_

@Porkepix I've seen that question appear in one form or another a few times, so I've decided to just answer it once and for all here: https://github.com/BurntSushi/ripgrep/discussions/3204

---

_Comment by @Porkepix on 2025-10-22 14:12_

> [@Porkepix](https://github.com/Porkepix) I've seen that question appear in one form or another a few times, so I've decided to just answer it once and for all here: [#3204](https://github.com/BurntSushi/ripgrep/discussions/3204)

Thanks for the detailed answer; it's a shame you can't (I think) pin this like you can with an issue. Or maybe a first answer to a FAQ.md to come if you get some other recurring questions like this one!

Maybe something changed for those three special directories anyway, for the better, as the issue that always triggered now doesn't anymore, so it might be better handled now!

---

_Comment by @BurntSushi on 2025-10-22 14:22_

It's probably just luck to be honest. The behavior of files in these directories can be heavily reliant on system state.

I'd just strongly urge against searching them.

---
