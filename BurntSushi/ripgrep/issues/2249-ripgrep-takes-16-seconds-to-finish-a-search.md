```yaml
number: 2249
title: ripgrep takes 16 seconds to finish a search
type: issue
state: closed
author: menash134
labels:
  - invalid
assignees: []
created_at: 2022-06-27T22:27:29Z
updated_at: 2022-07-04T22:09:23Z
url: https://github.com/BurntSushi/ripgrep/issues/2249
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep takes 16 seconds to finish a search

---

_@menash134_


[ripgrep_log.zip](https://github.com/BurntSushi/ripgrep/files/8995945/ripgrep_log.zip)
#### What version of ripgrep are you using?

11.0.4 Ubuntu  and 13.0.0 Ubuntu (using compiled image)

#### How did you install ripgrep?

sudo apt install ripgrep or download the compiled image


#### What operating system are you using ripgrep on?
Ubuntu 20.04.4 LTS x86_64 
Kernel: 5.13.0-51-generic
Shell: zsh 5.8 
CPU: Intel i7-10700 (16) @ 4.800GHz
 GPU: Intel Device 9bc5 
Memory: 2660MiB / 64090MiB 

#### Describe your bug.

ripgrep takes too long time for finding a string.

#### What are the steps to reproduce the behavior?

rg l2t_updt . basically everything is slow in my system. I dont know why

#### What is the actual behavior?

output is too large and gist.github is really dumb and go out if i give the large output. Hence I put it in a zipped file.

[ripgrep_log.zip](https://github.com/BurntSushi/ripgrep/files/8995988/ripgrep_log.zip)




---

_Comment by @BurntSushi on 2022-06-27 22:34_

Sorry but this isn't an actionable bug report. You need to either provide a reproduction or dig into debugging ripgrep yourself to figure out where the search is taking most of its time. Try to search in subdirectories to see if you can find which one takes the most time. Then continue recursively until you find the problem.

Of course, 16s might be totally correct. Why would you think otherwise? Your report is totally and completely insufficient to even know whether 16s is slow ot blazingly fast!

I realize you provided an output log, but that isn't enough to reproduce the problem or even characterize it.

---

_Comment by @menash134 on 2022-06-28 13:31_

Hi Andrew,
Thanks for the immediate response and kudos to the ripgrep community. 
I could able to figure the problematic folders which is taking too long . As soon as I went inside the subfolders, the performance is less than a second. Why Iam saying this as a problem is I have a similar environment in debian, there the ripgrep takes only a second to do the same job.

Do you have any idea why it could take too long in Ubuntu and not in debian?
a) I guess it could be that it is taking too long to switch folders recursively to do the search
b) or I guess it could be searching some system folders by default which are not specified.

It would be really of great help if you provide your insights to my problem.

---

_Comment by @menash134 on 2022-06-28 13:38_

rg "l2_updt"  1.17s user 1.05s system 1148% cpu 0.193 total  -- debian

/usr/bin/rg "l2t_updt"  1,35s user 6,07s system 44% cpu 16,607 total - ubuntu

Any idea why?

---

_Comment by @BurntSushi on 2022-06-28 13:57_

The greatest insight I can give you is to examine your assumptions. For example, you've presented this as a problem in terms of Ubuntu vs Debian. The implication of your framing is that Ubuntu vs Debian is the _only difference_ between your two searches. I would instead ask you to question whether that's actually true or not. There are literally dozens of possible differences:

* Different versions of ripgrep.
* One environment has an ignore file somewhere (perhaps in a parent directory) that is causing ripgrep to skip a lot of files.
* One environment has a ripgrep config file somewhere (viewable with `echo $RIPGREP_CONFIG_PATH`) that is impacting how ripgrep is running.
* The things you're searching aren't actually equivalent.

Otherwise, I can think of no valid reason why "Ubuntu vs Debian" would have any material impact on how fast ripgrep runs.

> a) I guess it could be that it is taking too long to switch folders recursively to do the search
> b) or I guess it could be searching some system folders by default which are not specified.

Neither of these make sense to me.

**Examine your assumptions** is the best advice I can give you. You can do it with ripgrep. Start looking for whether the match counts are the same. Or whether `rg --files` outputs the same stuff in both environments.

I'm going to close this for now because it isn't actionable on my end. But happy to re-open it if you can find something to make it actionable.

---

_Closed by @BurntSushi on 2022-06-28 13:57_

---

_Label `invalid` added by @BurntSushi on 2022-06-28 13:57_

---

_Comment by @menash134 on 2022-07-04 21:57_

 strace rg "l2t_updt"                                                                                                                                                                                                ✔  with ashokkumar@HPZ2  04.07.22    at 23:49:10   
execve("/home/ashokkumar/bin/rg", ["rg", "l2t_updt"], 0x7ffcc49af988 /* 43 vars */) = 0
brk(NULL)                               = 0x55a536ba1000
arch_prctl(0x3001 /* ARCH_??? */, 0x7ffd0e9d5650) = -1 EINVAL (Invalid argument)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=99952, ...}) = 0
mmap(NULL, 99952, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fbef2e8c000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\3405\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=104984, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbef2e8a000
mmap(NULL, 107592, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fbef2e6f000
mmap(0x7fbef2e72000, 73728, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3000) = 0x7fbef2e72000
mmap(0x7fbef2e84000, 16384, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15000) = 0x7fbef2e84000
mmap(0x7fbef2e88000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x18000) = 0x7fbef2e88000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220q\0\0\0\0\0\0"..., 832) = 832
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0{E6\364\34\332\245\210\204\10\350-\0106\343="..., 68, 824) = 68
fstat(3, {st_mode=S_IFREG|0755, st_size=157224, ...}) = 0
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0{E6\364\34\332\245\210\204\10\350-\0106\343="..., 68, 824) = 68
mmap(NULL, 140408, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fbef2e4c000
mmap(0x7fbef2e52000, 69632, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x6000) = 0x7fbef2e52000
mmap(0x7fbef2e63000, 24576, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x17000) = 0x7fbef2e63000
mmap(0x7fbef2e69000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1c000) = 0x7fbef2e69000
mmap(0x7fbef2e6b000, 13432, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fbef2e6b000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\300\323\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=1369384, ...}) = 0
mmap(NULL, 1368336, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fbef2cfd000
mmap(0x7fbef2d0a000, 684032, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xd000) = 0x7fbef2d0a000
mmap(0x7fbef2db1000, 626688, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0xb4000) = 0x7fbef2db1000
mmap(0x7fbef2e4a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x14c000) = 0x7fbef2e4a000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0 \22\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=18848, ...}) = 0
mmap(NULL, 20752, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fbef2cf7000
mmap(0x7fbef2cf8000, 8192, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1000) = 0x7fbef2cf8000
mmap(0x7fbef2cfa000, 4096, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3000) = 0x7fbef2cfa000
mmap(0x7fbef2cfb000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3000) = 0x7fbef2cfb000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\300A\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0\20\0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0", 32, 848) = 32
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\30x\346\264ur\f|Q\226\236i\253-'o"..., 68, 880) = 68
fstat(3, {st_mode=S_IFREG|0755, st_size=2029592, ...}) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0\20\0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0", 32, 848) = 32
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\30x\346\264ur\f|Q\226\236i\253-'o"..., 68, 880) = 68
mmap(NULL, 2037344, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fbef2b05000
mmap(0x7fbef2b27000, 1540096, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x22000) = 0x7fbef2b27000
mmap(0x7fbef2c9f000, 319488, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19a000) = 0x7fbef2c9f000
mmap(0x7fbef2ced000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1e7000) = 0x7fbef2ced000
mmap(0x7fbef2cf3000, 13920, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fbef2cf3000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbef2b03000
arch_prctl(ARCH_SET_FS, 0x7fbef2b04500) = 0
mprotect(0x7fbef2ced000, 16384, PROT_READ) = 0
mprotect(0x7fbef2cfb000, 4096, PROT_READ) = 0
mprotect(0x7fbef2e4a000, 4096, PROT_READ) = 0
mprotect(0x7fbef2e69000, 4096, PROT_READ) = 0
mprotect(0x7fbef2e88000, 4096, PROT_READ) = 0
mprotect(0x55a534d18000, 225280, PROT_READ) = 0
mprotect(0x7fbef2ed2000, 4096, PROT_READ) = 0
munmap(0x7fbef2e8c000, 99952)           = 0
set_tid_address(0x7fbef2b047d0)         = 753216
set_robust_list(0x7fbef2b047e0, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7fbef2e52bf0, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7fbef2e60420}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7fbef2e52c90, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7fbef2e60420}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
poll([{fd=0, events=0}, {fd=1, events=0}, {fd=2, events=0}], 3, 0) = 0 (Timeout)
rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x7fbef2b48090}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x55a534bd7870, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7fbef2e60420}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x55a534bd7870, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK|SA_SIGINFO, sa_restorer=0x7fbef2e60420}, NULL, 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef2ea2000
mprotect(0x7fbef2ea2000, 4096, PROT_NONE) = 0
sigaltstack({ss_sp=0x7fbef2ea3000, ss_flags=0, ss_size=8192}, NULL) = 0
brk(NULL)                               = 0x55a536ba1000
brk(0x55a536bc2000)                     = 0x55a536bc2000
openat(AT_FDCWD, "/proc/self/maps", O_RDONLY|O_CLOEXEC) = 3
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
fstat(3, {st_mode=S_IFREG|0444, st_size=0, ...}) = 0
read(3, "55a53494b000-55a534997000 r--p 0"..., 1024) = 1024
read(3, "c9f000-7fbef2ced000 r--p 0019a00"..., 1024) = 1024
read(3, "b/x86_64-linux-gnu/libm-2.31.so\n"..., 1024) = 1024
read(3, " /usr/lib/x86_64-linux-gnu/libpt"..., 1024) = 1024
read(3, "so\n7fbef2ec9000-7fbef2ed1000 r--"..., 1024) = 707
close(3)                                = 0
sched_getaffinity(753216, 32, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
mmap(NULL, 172032, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbef2ad9000
munmap(0x7fbef2ad9000, 172032)          = 0
getrandom("\x6b\x69\xf0\x61\x42\x41\x35\x72\x9b\x8d\x37\x01\x5e\xa1\x83\xa6", 16, GRND_NONBLOCK) = 16
ioctl(0, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(0, NULL, AT_STATX_SYNC_AS_STAT, STATX_ALL, NULL) = -1 EFAULT (Bad address)
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
openat(AT_FDCWD, "/proc/self/cgroup", O_RDONLY|O_CLOEXEC) = 3
read(3, "13:perf_event:/\n12:devices:/user"..., 8192) = 359
close(3)                                = 0
openat(AT_FDCWD, "/proc/self/mountinfo", O_RDONLY|O_CLOEXEC) = 3
read(3, "24 31 0:22 / /sys rw,nosuid,node"..., 8192) = 4090
close(3)                                = 0
openat(AT_FDCWD, "/sys/fs/cgroup/cpu,cpuacct/cpu.cfs_quota_us", O_RDONLY|O_CLOEXEC) = 3
statx(3, "", AT_STATX_SYNC_AS_STAT|AT_EMPTY_PATH, STATX_ALL, {stx_mask=STATX_BASIC_STATS|0x1000, stx_attributes=0, stx_mode=S_IFREG|0644, stx_size=0, ...}) = 0
lseek(3, 0, SEEK_CUR)                   = 0
read(3, "-1\n", 32)                     = 3
read(3, "", 29)                         = 0
close(3)                                = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
statx(1, "", AT_STATX_SYNC_AS_STAT|AT_EMPTY_PATH, STATX_ALL, {stx_mask=STATX_BASIC_STATS|0x1000, stx_attributes=0, stx_mode=S_IFCHR|0620, stx_size=0, ...}) = 0
statx(1, "", AT_STATX_SYNC_AS_STAT|AT_EMPTY_PATH, STATX_ALL, {stx_mask=STATX_BASIC_STATS|0x1000, stx_attributes=0, stx_mode=S_IFCHR|0620, stx_size=0, ...}) = 0
brk(0x55a536beb000)                     = 0x55a536beb000
brk(0x55a536c31000)                     = 0x55a536c31000
brk(0x55a536c73000)                     = 0x55a536c73000
openat(AT_FDCWD, "/home/ashokkumar/.gitconfig", O_RDONLY|O_CLOEXEC) = 3
statx(3, "", AT_STATX_SYNC_AS_STAT|AT_EMPTY_PATH, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFREG|0644, stx_size=149, ...}) = 0
lseek(3, 0, SEEK_CUR)                   = 0
read(3, "[user]\n\temail = ashokkumar.shanm"..., 149) = 149
read(3, "", 32)                         = 0
close(3)                                = 0
openat(AT_FDCWD, "/home/ashokkumar/.config/git/config", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
statx(AT_FDCWD, "/home/ashokkumar/.config/git/ignore", AT_STATX_SYNC_AS_STAT, STATX_ALL, 0x7ffd0e9d3a20) = -1 ENOENT (No such file or directory)
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
futex(0x7fbef2cfc0c8, FUTEX_WAKE_PRIVATE, 2147483647) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef2902000
mprotect(0x7fbef2903000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef2b01ef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753217], tls=0x7fbef2b02700, child_tidptr=0x7fbef2b029d0) = 753217
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef2701000
mprotect(0x7fbef2702000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef2900ef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753218], tls=0x7fbef2901700, child_tidptr=0x7fbef29019d0) = 753218
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef2500000
mprotect(0x7fbef2501000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef26ffef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753219], tls=0x7fbef2700700, child_tidptr=0x7fbef27009d0) = 753219
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
brk(0x55a536c9b000)                     = 0x55a536c9b000
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef22ff000
mprotect(0x7fbef2300000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef24feef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753220], tls=0x7fbef24ff700, child_tidptr=0x7fbef24ff9d0) = 753220
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef20fe000
mprotect(0x7fbef20ff000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef22fdef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753221], tls=0x7fbef22fe700, child_tidptr=0x7fbef22fe9d0) = 753221
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
brk(0x55a536ccb000)                     = 0x55a536ccb000
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef1efd000
mprotect(0x7fbef1efe000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef20fcef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753222], tls=0x7fbef20fd700, child_tidptr=0x7fbef20fd9d0) = 753222
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
1.diff
28:>               if (dpd_interap_check_l2t_updt_pkt(skb)) {
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef1cfc000
mprotect(0x7fbef1cfd000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef1efbef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753223], tls=0x7fbef1efc700, child_tidptr=0x7fbef1efc9d0) = 753223
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
brk(0x55a536cfb000)                     = 0x55a536cfb000
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef1afb000
mprotect(0x7fbef1afc000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef1cfaef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753224], tls=0x7fbef1cfb700, child_tidptr=0x7fbef1cfb9d0) = 753224
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef18f7000
mprotect(0x7fbef18f8000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef1af6ef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753225], tls=0x7fbef1af7700, child_tidptr=0x7fbef1af79d0) = 753225
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
brk(0x55a536d2a000)                     = 0x55a536d2a000
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef16f3000
mprotect(0x7fbef16f4000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef18f2ef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753226], tls=0x7fbef18f3700, child_tidptr=0x7fbef18f39d0) = 753226
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef14ef000
mprotect(0x7fbef14f0000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef16eeef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753227], tls=0x7fbef16ef700, child_tidptr=0x7fbef16ef9d0) = 753227
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
sched_getaffinity(0, 128, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]) = 8
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
statx(AT_FDCWD, "./", AT_STATX_SYNC_AS_STAT, STATX_ALL, {stx_mask=STATX_ALL|0x1000, stx_attributes=0, stx_mode=S_IFDIR|0770, stx_size=4096, ...}) = 0
brk(0x55a536d59000)                     = 0x55a536d59000
mmap(NULL, 2101248, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fbef12eb000
mprotect(0x7fbef12ec000, 2097152, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fbef14eaef0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[753228], tls=0x7fbef14eb700, child_tidptr=0x7fbef14eb9d0) = 753228
futex(0x7fbef2b029d0, FUTEX_WAIT, 753217, NULL   [>>>>>>>XXXXXXXXX: here it is waiting in futex.]
wlan2/linux-drivers/scalance_dpd/dpd_forward.c.keep
1006:              if (dpd_interap_check_l2t_updt_pkt(skb)) {
1022:      if (dpd_interap_check_l2t_updt_pkt(skb)) {

wlan2/linux-drivers/scalance_dpd/dpd_interap.c
337: * FUNCTION : dpd_interap_check_l2t_updt_pkt
349:int dpd_interap_check_l2t_updt_pkt (struct sk_buff *skb)
617:        if (dpd_interap_check_l2t_updt_pkt(skb))
) = 0
futex(0x7fbef1cfb9d0, FUTEX_WAIT, 753224, NULL) = 0
sigaltstack({ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=8192}, NULL) = 0
munmap(0x7fbef2ea2000, 12288)           = 0
exit_group(0)                           = ?
+++ exited with 0 +++



Hi Andrew,
Sorry to disturb you again. I think this might ring the bell to you. Above is the strace of the command which I gave in my project. It look 16 seconds and most of the time it is waiting on the futex() lock ( search for >>>>>>>XXXXXXXXX).  Could you share some light on what it is waiting for?

Iam being persuasive, because I could not able to use the telescope live grep in neovim for my project because of this problem. If I don't get any ideas any more, I plan to install rust-gdb and debug the code on my own. If you have any document for this, that would be of great help. The silver searcher + fzf on neovim kind of sucks. I love telescope as it was much faster in debian.

Thanks once again.


---

_Comment by @BurntSushi on 2022-07-04 22:09_

futex is a low level synchronization primitive. So try disabling parallelism via `-j1`.

---
