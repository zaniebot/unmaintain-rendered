```yaml
number: 1353
title: traversing large amounts of symlinks is sub-optimal
type: issue
state: open
author: Swatinem
labels:
  - enhancement
assignees: []
created_at: 2019-08-20T11:34:54Z
updated_at: 2024-02-06T02:57:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1353
synced_at: 2026-01-12T16:13:23Z
```

# traversing large amounts of symlinks is sub-optimal

---

_@Swatinem_

#### What version of ripgrep are you using?

ripgrep 11.0.2

#### How did you install ripgrep?

from the archlinux repository

#### What operating system are you using ripgrep on?
archlinux, latest

#### Describe your question, feature request, or bug.

Some background, pardon the `node` speak:
* I have a `yarn workspaces` setup
* One of our dependencies dependencies can’t deal with hoisted `node_modules`, it is looking up things in `node_modules` based on the `cwd` rather.
* For that reason, I actually create symlinks in a `postinstall` script.

This essentially leads to a lot of mutually recursive symlinks, which are not handled really nicely by `ripgrep` (or rather the way it uses `walkdir` internally?)

#### If this is a bug, what are the steps to reproduce the behavior?

I created a small shell script which creates mutually recursive symlinks:
```sh
#!/bin/sh

N=${1-5}
DIR=${2-symlinks}

for i in $(seq $N)
do
    mkdir -p $DIR/$i
    touch $DIR/$i/file
    for j in $(seq $N)
    do
        ln -s ../$j symlinks/$i/$j
    done
done
```

It appears ripgrep exhibits exponential (?) behavior when walking this tree with `--follow`.

#### If this is a bug, what is the actual behavior?

```
λ ~/Coding rm -rf symlinks && ./symlinks.sh 3 && rg --sort-files --files --follow symlinks 2>/dev/null
symlinks/1/2/3/file
symlinks/1/2/file
symlinks/1/3/2/file
symlinks/1/3/file
symlinks/1/file
symlinks/2/1/3/file
symlinks/2/1/file
symlinks/2/3/1/file
symlinks/2/3/file
symlinks/2/file
symlinks/3/1/2/file
symlinks/3/1/file
symlinks/3/2/1/file
symlinks/3/2/file
symlinks/3/file
λ ~/Coding rm -rf symlinks && ./symlinks.sh 5 && time rg --sort-files --files --follow symlinks 2>/dev/null | wc -l
325
rg --sort-files --files --follow symlinks 2> /dev/null  0,01s user 0,02s system 97% cpu 0,038 total
λ ~/Coding rm -rf symlinks && ./symlinks.sh 7 && time rg --sort-files --files --follow symlinks 2>/dev/null | wc -l
13699
rg --sort-files --files --follow symlinks 2> /dev/null  0,73s user 1,63s system 97% cpu 2,432 total
λ ~/Coding rm -rf symlinks && ./symlinks.sh 8 && time rg --sort-files --files --follow symlinks 2>/dev/null | wc -l
109600
rg --sort-files --files --follow symlinks 2> /dev/null  7,20s user 16,65s system 97% cpu 24,519 total
```

The timing gets worse the more mutually recursive symlinks I have, obviously.

#### If this is a bug, what is the expected behavior?

Well this is rather a matter of debate how the expected behavior of `--follow` should be…
IMO ripgrep should just resolve symlinks to their original directory, keeping track of all the already visited directories.
But that would be a behavior change to the `--follow` option, so not sure what would be the best to avoid this pathological case…

---

_Comment by @BurntSushi on 2019-08-20 12:32_

Thank you for the very detailed bug report. I appreciate it and the easy reproduction.

> It appears ripgrep exhibits exponential (?) behavior when walking this tree
> with `--follow`.

Correct, as do other standard tools such as `grep` and `find`. This is expected
because your `symlinks.sh` program builds a directory tree whose size is an
exponential function of its input:

```
$ rm -rf symlinks && sh symlinks.sh 6 && time find -L ./symlinks/ -type f 2>&1 | wc -l
11742

real    0.072
user    0.020
sys     0.052
maxmem  3 MB
faults  0

$ rm -rf symlinks && sh symlinks.sh 7 && time find -L ./symlinks/ -type f 2>&1 | wc -l
95900

real    0.542
user    0.119
sys     0.420
maxmem  3 MB
faults  0

$ rm -rf symlinks && sh symlinks.sh 8 && time find -L ./symlinks/ -type f 2>&1 | wc -l
876808

real    4.766
user    1.255
sys     3.493
maxmem  3 MB
faults  0

$ rm -rf symlinks && sh symlinks.sh 9 && time find -L ./symlinks/ -type f 2>&1 | wc -l
8877690

real    51.189
user    13.659
sys     37.022
maxmem  3 MB
faults  0
```

grep takes a similar amount of time (and has the same exponential
relationship):

```
$ rm -rf symlinks && sh symlinks.sh 8 && time grep -R foo ./symlinks 2>&1 | wc -l
767208

real    4.661
user    0.909
sys     3.693
maxmem  3 MB
faults  0
```

Unfortunately, ripgrep is quite a bit slower here. But not exponentially
slower:

```
$ rm -rf symlinks && sh symlinks.sh 8 && time rg -uuu -j1 -L foo ./symlinks 2>&1 | wc -l
767208

real    19.324
user    3.975
sys     14.363
maxmem  6 MB
faults  0
```

The fact that both grep and ripgrep have the same number of lines in the output
suggests that they are using the same algorithm to detect cycles in the
directory tree.

My guess here was that the performance difference between grep and ripgrep
was perhaps due to a different syscall profile, based on the gap in `sys`
time shown above. Using `strace -c -o <file> <command>`, I captured aggregate
syscall statistics from both grep and ripgrep on a directory created with
`sh symlinks.sh 6`:

```
$ cat syscalls.grep
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 43.72    1.274477          43     29358           write
 25.65    0.747619          47     15656           stat
  6.59    0.192018          49      3918           openat
  5.68    0.165624          42      3914           getdents64
  5.52    0.160874          41      3920           close
  5.25    0.152904          39      3919           fstat
  4.91    0.143054          36      3914           fcntl
  2.66    0.077684          39      1967           read
  0.01    0.000346          57         6           mprotect
  0.01    0.000151          30         5           brk
  0.00    0.000080          80         1           set_tid_address
  0.00    0.000075          75         1           prlimit64
  0.00    0.000066          66         1           set_robust_list
  0.00    0.000057          19         3           rt_sigaction
  0.00    0.000042          42         1           munmap
  0.00    0.000017          17         1           rt_sigprocmask
  0.00    0.000016           0        17           mmap
  0.00    0.000015          15         1           sigaltstack
  0.00    0.000012           6         2         1 arch_prctl
  0.00    0.000000           0         8           lseek
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           execve
------ ----------- ----------- --------- --------- ----------------
100.00    2.915131                 66615         2 total

$ cat syscalls.rg-uuu
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 26.98    2.686208          48     55774           openat
 22.42    2.231422          40     55774           close
 20.87    2.077032          38     53819           fstat
 20.56    2.046429          41     48930           write
  5.85    0.582764          49     11742           stat
  1.73    0.172649          44      3914           getdents64
  1.56    0.155003          39      3933           read
  0.01    0.001228          38        32           mmap
  0.01    0.000503          55         9           mprotect
  0.00    0.000355          25        14           brk
  0.00    0.000354          44         8           lseek
  0.00    0.000145          29         5           rt_sigaction
  0.00    0.000123          41         3           munmap
  0.00    0.000091          45         2           prlimit64
  0.00    0.000071          23         3           sigaltstack
  0.00    0.000059          59         1           rt_sigprocmask
  0.00    0.000049          49         1           set_tid_address
  0.00    0.000042          42         1           fcntl
  0.00    0.000042          42         1           sched_getaffinity
  0.00    0.000018          18         1           getrandom
  0.00    0.000011           5         2         1 arch_prctl
  0.00    0.000010          10         1           set_robust_list
  0.00    0.000000           0         1           lstat
  0.00    0.000000           0         5         5 ioctl
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         2           getcwd
------ ----------- ----------- --------- --------- ----------------
100.00    9.954608                233980         7 total
```

To try to narrow down where the problem is coming from, I used
[`walkdir-list`](https://github.com/BurntSushi/walkdir/tree/master/walkdir-list) to scan the directory tree, which is a very light CLI for
using `walkdir` (on `sh symlinks.sh 8`):

```
$ time walkdir-list -L ./symlinks 2>&1 | wc -l
986409

real    14.859
user    2.770
sys     11.604
maxmem  4 MB
faults  0
```

And here are the syscall counts (on `sh symlinks.sh 6`):

```
$ cat syscalls.walkdir
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 25.85    3.022064          56     53815           openat
 24.19    2.827694          48     58728           write
 21.75    2.542739          47     53815           close
 20.80    2.431960          45     53815           fstat
  5.65    0.660237          56     11736           stat
  1.65    0.192902          49      3914           getdents64
  0.09    0.010949          59       183           brk
  0.01    0.000963          43        22           mmap
  0.00    0.000486          30        16           read
  0.00    0.000469          67         7           mprotect
  0.00    0.000236          47         5           rt_sigaction
  0.00    0.000178          59         3           sigaltstack
  0.00    0.000151          75         2           munmap
  0.00    0.000096          96         1           set_tid_address
  0.00    0.000079          39         2           prlimit64
  0.00    0.000065          65         1         1 ioctl
  0.00    0.000065          65         1           fcntl
  0.00    0.000063          63         1           lstat
  0.00    0.000063           7         8           lseek
  0.00    0.000060          60         1           getrandom
  0.00    0.000029          29         1           set_robust_list
  0.00    0.000016          16         1           sched_getaffinity
  0.00    0.000014           7         2         1 arch_prctl
  0.00    0.000006           6         1           rt_sigprocmask
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           execve
------ ----------- ----------- --------- --------- ----------------
100.00   11.691584                236083         3 total
```

Which look pretty similar to ripgrep's syscall counts. To get a more accurate
baseline for `walkdir-list`, I also gathered the syscall counts from `find`:

```
$ strace -c -o syscalls.find find -L ./symlinks 2>&1 | wc -l
13699

$ cat syscalls.find
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 48.35    1.533142          47     32295           write
 26.06    0.826343          52     15656           stat
 10.71    0.339465          43      7830           fcntl
  5.86    0.185770          47      3914           getdents64
  3.39    0.107389          54      1962         1 openat
  2.89    0.091518          46      1963           close
  2.72    0.086340          44      1961           fstat
  0.01    0.000431          35        12           mmap
  0.01    0.000294          58         5           mprotect
  0.00    0.000084          84         1           fchdir
  0.00    0.000071           8         8           read
  0.00    0.000047          23         2         1 ioctl
  0.00    0.000031          31         1           munmap
  0.00    0.000023           7         3           brk
  0.00    0.000022           3         6           lseek
  0.00    0.000015           7         2         1 arch_prctl
  0.00    0.000011          11         1           uname
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           execve
------ ----------- ----------- --------- --------- ----------------
100.00    3.170996                 65624         4 total
```

As another baseline, I wanted to compare syscall counts in a more normal
setting. Specifically, the syscall counts from traversing a checkout of the
Linux kernel _without_ following symlinks:

```
$ strace -c -o /tmp/syscalls.linux.find find ./ 2>&1 | wc -l
66715

$ strace -c -o /tmp/syscalls.linux.walkdir walkdir-list ./ 2>&1 | wc -l
66715

$ cat /tmp/syscalls.linux.find
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 44.17    1.805669          45     39632           fcntl
 20.68    0.845417          47     17622           close
 12.62    0.515811          58      8791           getdents64
 10.91    0.446099          50      8788           newfstatat
  5.78    0.236137          53      4442           openat
  5.05    0.206392          46      4442           fstat
  0.78    0.031775          52       611           write
  0.02    0.000700          53        13           brk
  0.00    0.000060          60         1           fchdir
  0.00    0.000000           0         8           read
  0.00    0.000000           0         6           lseek
  0.00    0.000000           0        12           mmap
  0.00    0.000000           0         5           mprotect
  0.00    0.000000           0         1           munmap
  0.00    0.000000           0         2         1 ioctl
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         1           uname
  0.00    0.000000           0         2         1 arch_prctl
------ ----------- ----------- --------- --------- ----------------
100.00    4.088060                 84381         3 total

$ cat /tmp/syscalls.linux.walkdir
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 42.72    0.428360          48      8791           getdents64
 19.85    0.198981          45      4400           openat
 18.67    0.187147          42      4400           close
 16.96    0.170044          38      4400           fstat
  1.64    0.016452          52       311           write
  0.06    0.000581          26        22           mmap
  0.04    0.000405          57         7           mprotect
  0.01    0.000107           6        16           read
  0.01    0.000097          48         2           munmap
  0.01    0.000094          18         5           rt_sigaction
  0.01    0.000064          32         2         1 arch_prctl
  0.01    0.000062           5        11           brk
  0.01    0.000055          55         1           set_tid_address
  0.01    0.000053          53         1           set_robust_list
  0.00    0.000035          17         2           prlimit64
  0.00    0.000026           8         3           sigaltstack
  0.00    0.000012          12         1           rt_sigprocmask
  0.00    0.000009           9         1           lstat
  0.00    0.000007           7         1           getrandom
  0.00    0.000006           6         1         1 ioctl
  0.00    0.000006           6         1           sched_getaffinity
  0.00    0.000000           0         8           lseek
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           execve
------ ----------- ----------- --------- --------- ----------------
100.00    1.002603                 22389         3 total
```

The above is pretty interesting. I'm not sure what the deal is with find's
`fcntl` calls, but other than that, the counts are pretty similar, with
`walkdir` actually having fewer calls in some cases (e.g., `close`).

So to me, this suggests there is something sub-optimal in how `walkdir` is
doing symlink loop detection. I'm not sure what that is yet, but someone will
need to dig into it. From looking at the syscall counts, my guess is that
`walkdir` is _correctly_ opening a handle to a file in order to determine
file equivalence (this is the part where you check for a loop in symlinks),
where as `find` and `grep` are just relying on `stat`. It's possible that
perhaps `walkdir` should switch to `find`/`grep`'s method since it's faster,
even if it's not quite as correct.

With that said, the bottom line here is that if you don't want to take
exponential time to traverse a directory, then don't create directory trees
that are exponentially large.

> keeping track of all the already visited directories.

No. Symlink loop detection only checks whether a child is equivalent to an
ancestor. If so, a loop has been detected and an error is emitted. Doing any
other kind of "duplicate" detection is not appropriate for a tool like ripgrep.

---

_Renamed from "`--follow` with mutually recursive symlinks leads to exponential(?) slowdown" to "traversing large amounts of symlinks is sub-optimal" by @BurntSushi on 2019-08-20 12:32_

---

_Label `enhancement` added by @BurntSushi on 2019-08-20 12:32_

---

_Comment by @Swatinem on 2019-08-20 15:41_

Wow, thanks for this very detailed analysis.

Might be a bit off-topic but let me give some more details:

I noticed this when I tried to add a rust project into this giant nodejs monorepo.
* vscode was starting a ripgrep subprocess as part of some kind of search indexing, which used `--follow` and went completely crazy (no idea why vscode was never doing that as long as I was just using typescript)
* `cargo watch` was also hogging one cpu constantly and growing in memory usage without any progress.

I then went ahead and `perf`d `cargo watch`, which showed that all the time was spent in `notify`, which uses `walkdir` internally, which itself accounted for a lot of the time. So I was not quite sure if `notify` was just using it wrongly, or that the problem was rather with `walkdir` itself. And since I could reproduce the same slowdown with `ripgrep`, here I am.

> Doing any
> other kind of "duplicate" detection is not appropriate for a tool like ripgrep.

Yes, I thought so. However, as the author of `walkdir`, what would be your recommendation for fixing `notify` / `cargo-watch` in this case?

Well in the end, I think I can avoid creating most of these symlinks, which should hopefully improve this situation on my end somewhat.

![Bildschirmfoto von 2019-08-20 17-17-37](https://user-images.githubusercontent.com/580492/63361759-025ed180-c371-11e9-94b8-ec0624548ec9.png)

https://github.com/passcod/notify/blob/0709c940dd7294ef07b176504a89949d462bbe98/src/inotify.rs#L409-L466

---

_Comment by @BurntSushi on 2019-08-20 16:21_

> However, as the author of walkdir, what would be your recommendation for fixing notify / cargo-watch in this case?

If performance of `find`/`grep` is good enough for you, then it's plausible `walkdir` could get to that speed, but it still exhibits exponential growth. Other than that, I'm not really sure what can be done other than not creating exponentially sized directory trees. Note that `walkdir` doesn't prevent the strategy of "keep a set of all visited directories and don't descend if it has been seen," so if that helps your case, then `notify`/`cargo-watch` could potentially implement that.

---

_Comment by @forivall on 2024-02-06 02:57_

Would a `max-follow` option be appropriate to solve this issue? Similar to `max-depth`, but only applying to symlink traversal.

On the topic of vscode (@Swatinem), turning off `search.followSymlinks` resolves the problem for me (it may be new since the original comment)


---
