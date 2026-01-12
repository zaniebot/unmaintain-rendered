```yaml
number: 311
title: Search in large directories causes the system to freeze
type: issue
state: closed
author: klingtnet
labels:
  - bug
assignees: []
created_at: 2017-01-09T12:04:37Z
updated_at: 2017-10-06T02:39:15Z
url: https://github.com/BurntSushi/ripgrep/issues/311
synced_at: 2026-01-12T16:13:21Z
```

# Search in large directories causes the system to freeze

---

_@klingtnet_

Ripgrep uses all available system resources when something is searched within a very large directory, e.g. root `/`.
It takes up the 16GB RAM of my machine instantly which causes it to freeze until the process is killed.
Here is the example call:

```sh
$ rg '$SOMEVAR' / 2&>/dev/null
zsh: killed     rg '$SOMEVAR' / 2&> /dev/null
```

I am using the following ripgrep build:

```sh
$ rg --version
ripgrep 0.3.2
```

---

_Comment by @klingtnet on 2017-01-09 12:07_

The command exits succesfully when [the-silver-searcher](https://github.com/ggreer/the_silver_searcher) is used instead:

```sh
$ ag '$SOMEVAR' 2&>/dev/null /
...
```


---

_Comment by @BurntSushi on 2017-01-09 13:11_

I can reproduce it on my system. There's likely something in `/dev` or `/proc` that's causing an issue.

---

_Label `bug` added by @BurntSushi on 2017-01-09 13:11_

---

_Comment by @BurntSushi on 2017-01-09 13:15_

Yes, `rg /proc` causes the same problem. If I search everything except for `/proc` (i.e., `rg -g '!/proc' '$SOMEVAR' /`), then ripgrep works fine.

---

_Comment by @klingtnet on 2017-01-09 13:38_

I can second that, it works fine when `/proc` is omitted.

---

_Comment by @klingtnet on 2017-01-09 14:08_

I hope this helps, somehow:

```sh
$ strace -f -e open -- rg foobarbaz /proc &>/tmp/rg.log
```

```
$ grep --invert-match 'os error' /tmp/rg.log | tail -n100 
[pid 20954] open("/proc/2162/cmdline", O_RDONLY|O_CLOEXEC) = 10
[pid 20954] open("/proc/2162/stat", O_RDONLY|O_CLOEXEC) = 10
[pid 20954] open("/proc/2162/statm", O_RDONLY|O_CLOEXEC) = 10
[pid 20954] open("/proc/2162/maps", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/numa_maps", O_RDONLY|O_CLOEXEC) = 8
[pid 20954] open("/proc/2162/numa_maps", O_RDONLY|O_CLOEXEC) = 10
[pid 20954] open("/proc/2162/mem", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
[pid 20954] open("/proc/2162/mounts", O_RDONLY|O_CLOEXEC) = 10
[pid 20954] open("/proc/2162/mountinfo", O_RDONLY|O_CLOEXEC) = 10
[pid 20954] open("/proc/2162/mountstats", O_RDONLY|O_CLOEXEC) = 10
[pid 20954] open("/proc/2162/clear_refs", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
/proc/2162/clear_refs: [pid 20956] open("/proc/2164/mem", O_RDONLY|O_CLOEXECPermission denied) = -1 EACCES (Permission denied)
/proc/2164/mem[pid 20954] open("/proc/2162/smaps", O_RDONLY|O_CLOEXEC: ) = 8
[pid 20956] open("/proc/2164/mounts", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/mountinfo", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/mountstats", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/clear_refs", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
[pid 20956] open("/proc/2164/smaps", O_RDONLY|O_CLOEXEC) = 10
[pid 20954] open("/proc/2162/pagemap", O_RDONLY|O_CLOEXEC) = 8
[pid 20956] open("/proc/2164/pagemap", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/wchan", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/stack", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/schedstat", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/cpuset", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/cgroup", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/oom_score", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/oom_adj", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/oom_score_adj", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/coredump_filter", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/io", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2164/timerslack_ns", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181", O_RDONLY|O_NONBLOCK|O_DIRECTORY|O_CLOEXEC) = 6
[pid 20956] open("/proc/2181/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
[pid 20956] open("/proc/2181/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
[pid 20956] open("/proc/2181/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
[pid 20956] open("/proc/2181/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
[pid 20956] open("/proc/2181/environ", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/auxv", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/status", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/personality", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/limits", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/sched", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/autogroup", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/comm", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/syscall", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/cmdline", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/stat", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/statm", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/maps", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/numa_maps", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/mem", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
[pid 20956] open("/proc/2181/mounts", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/mountinfo", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/mountstats", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/clear_refs", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
[pid 20956] open("/proc/2181/smaps", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/pagemap", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/wchan", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/stack", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/schedstat", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/cpuset", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/cgroup", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/oom_score", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/oom_adj", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/oom_score_adj", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/coredump_filter", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/io", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2181/timerslack_ns", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186", O_RDONLY|O_NONBLOCK|O_DIRECTORY|O_CLOEXEC) = 6
[pid 20956] open("/proc/2186/.rgignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
[pid 20956] open("/proc/2186/.ignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
[pid 20956] open("/proc/2186/.gitignore", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
[pid 20956] open("/proc/2186/.git/info/exclude", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
[pid 20956] open("/proc/2186/environ", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/auxv", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/status", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/personality", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/limits", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/sched", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/autogroup", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/comm", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/syscall", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/cmdline", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/stat", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/statm", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/maps", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/numa_maps", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/mem", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
[pid 20956] open("/proc/2186/mounts", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/mountinfo", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/mountstats", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/clear_refs", O_RDONLY|O_CLOEXEC) = -1 EACCES (Permission denied)
[pid 20956] open("/proc/2186/smaps", O_RDONLY|O_CLOEXEC) = 10
[pid 20956] open("/proc/2186/pagemap", O_RDONLY|O_CLOEXEC) = 10
[pid 20955] +++ killed by SIGKILL +++
[pid 20957] +++ killed by SIGKILL +++
[pid 20956] +++ killed by SIGKILL +++
[pid 20954] +++ killed by SIGKILL +++
[pid 20953] +++ killed by SIGKILL +++
+++ killed by SIGKILL +++
```

---

_Comment by @klingtnet on 2017-01-09 14:12_

Process 2186 is gvfsd, [Gnome's virtual file system](https://en.wikipedia.org/wiki/GVfs).

---

_Comment by @BurntSushi on 2017-01-10 02:01_

OK, I've finally had time to look at this and I'm closing in on the problem.

I can reproduce the issue by searching the `pagemap` file of any process:

```
$ rg foo /proc/7869/pagemap
```

If I run the same command using `grep`, then it retains constant memory but doesn't terminate after a minute or so. However, I can seem to compute the size of the file (but this takes minutes):

```
$ cat /proc/7869/pagemap | wc -c
274877906936
```

It turns out that `pagemap` is a virtual file that maps the virtual pages of a process to physical pages. The vast majority of this file are `NUL` bytes.

-----

Here's the bad news: `ag`'s behavior is *accidentally* better. In particular, since `pagemap` files report themselves as *normal* files, `ag` will try to memory map them. But virtual files cannot be memory mapped, so the map fails and `ag` skips the file altogether. In ripgrep, it does the right thing by using a normal stream search on zero sized files, which permits it to read virtual files. This behavior is desirable. Compare the output of `rg MHz /proc/cpuinfo` and `ag MHz /proc/cpuinfo`, for example. (Spoiler: `ag` fails.) That means there's no portable way (AFAIK) to detect that ripgrep shouldn't search `pagemap` files.

Still, at the very least, `grep` will munch on a `pagemap` file until completion (if given enough time), and it will use constant memory. ripgrep *should* do the same. Therefore, there's a bug somewhere in ripgrep that's causing memory exhaustion. (It is undoubtedly related to the abundance of `NUL` bytes.)

With that said, one could argue that ripgrep's current *buggy* behavior is actually better because it is *loud*. Instead of silently **not terminating** for a very long time (like GNU grep), it will instead exhaust memory quickly and get killed.

Therefore, there's a second bug. A UX bug. ripgrep really should ignore the `/proc` directory by default. There's almost no good reason for that directory to be searched by default. Of course, explicitly specifying the `/proc` directory should still work.

For now, I can think of two simple workarounds if you want to continue searching `/` with reckless abandon:

```
$ alias rg="rg -g '!/proc'"
```

or

```
$ sudo sh -c 'echo /proc > /.ignore'
```

---

_Comment by @BurntSushi on 2017-01-10 02:01_

FWIW, the TL;DR is: *don't search `/proc`.* Things don't end well, even if you use GNU grep. :-)

---

_Comment by @klingtnet on 2017-01-10 08:48_

@BurntSushi Thanks for this thorough investigation and explanation of the problem, this was interesting to read.

---

_Comment by @BurntSushi on 2017-01-11 03:16_

While I haven't worked on this yet, I realized something: `grep -a pagemap` should exhibit the same memory exhaustion behavior since it treats binary data as if it were text. Indeed, that is the case.

---

_Closed by @BurntSushi on 2017-02-18 21:20_

---

_Comment by @twmb on 2017-10-06 02:11_

re: "Therefore, there's a bug somewhere in ripgrep that's causing memory exhaustion":

I would suspect this behavior is coming from https://github.com/BurntSushi/ripgrep/blob/214f2bef666efd686b1be250fc8e9f54a2cebb0f/src/search_stream.rs#L580-L612, where the searcher will continually grow its buffer while trying to find a newline.

---

_Comment by @BurntSushi on 2017-10-06 02:14_

Yes. I mentioned that above and fixed it.

---

_Comment by @twmb on 2017-10-06 02:39_

Ah sorry, my mistake. I didn't realize I was running an old version (0.4.0) was trying to track down what possible code path could be causing my problems.

Thanks for the fix in https://github.com/BurntSushi/ripgrep/commit/79d40d0e20a5710476b43c58c42db5124c2010a1!

---
