```yaml
number: 863
title: Permission denied
type: issue
state: closed
author: shmup
labels: []
assignees: []
created_at: 2018-03-20T00:11:04Z
updated_at: 2018-03-20T01:24:02Z
url: https://github.com/BurntSushi/ripgrep/issues/863
synced_at: 2026-01-12T16:13:22Z
```

# Permission denied

---

_@shmup_

#### What version of ripgrep are you using?
0.8.1

#### What operating system are you using ripgrep on?
Ubuntu 17.10

#### Describe your question, feature request, or bug.

Receiving a permission denied error in a particular directory with fine permissions. I used to be able to ripgrep it fine. Perhaps I'll discover why I was stupid before anyone reads this and I'll close this myself. :)

#### If this is a bug, what are the steps to reproduce the behavior?

```
[~/.vim (master *+)]$ rg shit
./: Permission denied (os error 13)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
[~/.vim (master *+)]$ ll
total 128
drwxr-xr-x 12 jared jared  4096 Mar 16 10:00 ./
drwx------ 59 jared jared 20480 Mar 19 20:07 ../
drwxr-xr-x  3 jared jared  4096 Mar 14 15:54 after/
drwxr-xr-x  2 jared jared  4096 Mar 13 21:12 colors/
drwxr-xr-x  2 jared jared  4096 Mar 14 19:48 docs/
... and more ...
```


---

_Comment by @BurntSushi on 2018-03-20 00:17_

Please show the output of running `strace rg shit` and also `strace rg shit ./`.

---

_Comment by @shmup on 2018-03-20 00:20_

Oh cool, I figured it out ^ sorry haven't done that yet, but I just decide to `mv .vim vim` and then step inside and do a `rg` and the output works fine.

I'll do a strace now.

---

_Comment by @shmup on 2018-03-20 00:22_

Incoming:

```
execve("/snap/bin/rg", ["rg", "shit"], [/* 111 vars */]) = 0
brk(NULL)                               = 0x557eff7f3000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f28f9509000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360a\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=144776, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f28f9507000
mmap(NULL, 2221160, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f28f90f1000
mprotect(0x7f28f910b000, 2093056, PROT_NONE) = 0
mmap(0x7f28f930a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7f28f930a000
mmap(0x7f28f930c000, 13416, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f28f930c000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\22\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1960656, ...}) = 0
mmap(NULL, 4061792, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f28f8d11000
mprotect(0x7f28f8ee7000, 2097152, PROT_NONE) = 0
mmap(0x7f28f90e7000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d6000) = 0x7f28f90e7000
mmap(0x7f28f90ed000, 14944, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f28f90ed000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f28f9504000
arch_prctl(ARCH_SET_FS, 0x7f28f9504740) = 0
mprotect(0x7f28f90e7000, 16384, PROT_READ) = 0
mprotect(0x7f28f930a000, 4096, PROT_READ) = 0
mprotect(0x557efe340000, 3584000, PROT_READ) = 0
mprotect(0x7f28f9537000, 4096, PROT_READ) = 0
munmap(0x7f28f9509000, 185135)          = 0
set_tid_address(0x7f28f9504a10)         = 3073
set_robust_list(0x7f28f9504a20, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f28f90f6c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f28f90f6d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
brk(NULL)                               = 0x557eff7f3000
brk(0x557eff814000)                     = 0x557eff814000
sched_getaffinity(0, 8192, [0, 1, 2, 3]) = 8
mmap(0xc000000000, 65536, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc000000000
munmap(0xc000000000, 65536)             = 0
mmap(NULL, 262144, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f28f94c4000
mmap(0xc420000000, 1048576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc420000000
mmap(0xc41fff8000, 32768, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc41fff8000
mmap(0xc000000000, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc000000000
mmap(NULL, 65536, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f28f9527000
mmap(NULL, 65536, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f28f9517000
rt_sigprocmask(SIG_SETMASK, NULL, [], 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
sigaltstack({ss_sp=0xc420002000, ss_flags=0, ss_size=32672}, NULL) = 0
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
gettid()                                = 3073
rt_sigaction(SIGHUP, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGINT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGINT, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGQUIT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGILL, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGILL, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGTRAP, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTRAP, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGABRT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGABRT, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGFPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGFPE, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGUSR1, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGUSR1, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGUSR2, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGUSR2, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGALRM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGALRM, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGTERM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGSTKFLT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSTKFLT, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGCHLD, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGCHLD, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGURG, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGURG, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGXCPU, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGXCPU, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGXFSZ, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGXFSZ, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGVTALRM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGVTALRM, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGPROF, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPROF, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGWINCH, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGWINCH, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGIO, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGIO, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGPWR, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPWR, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGSYS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSYS, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRTMIN, NULL, {sa_handler=0x7f28f90f6c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f28f9104150}, 8) = 0
rt_sigaction(SIGRTMIN, NULL, {sa_handler=0x7f28f90f6c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f28f9104150}, 8) = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f28f90f6c70, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, NULL, {sa_handler=0x7f28f90f6d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, 8) = 0
rt_sigaction(SIGRT_1, NULL, {sa_handler=0x7f28f90f6d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f28f90f6d00, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_2, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_2, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_3, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_3, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_4, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_4, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_5, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_5, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_6, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_6, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_7, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_7, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_8, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_8, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_9, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_9, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_10, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_10, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_11, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_11, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_12, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_12, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_13, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_13, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_14, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_14, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_15, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_15, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_16, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_16, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_17, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_17, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_18, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_18, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_19, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_19, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_20, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_20, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_21, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_21, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_22, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_22, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_23, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_23, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_24, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_24, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_25, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_25, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_26, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_26, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_27, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_27, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_28, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_28, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_29, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_29, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_30, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_30, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_31, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_31, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigaction(SIGRT_32, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_32, {sa_handler=0x557efdc8de30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f28f9104150}, NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f28f8510000
mprotect(0x7f28f8511000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f28f8d0ffb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f28f8d109d0, tls=0x7f28f8d10700, child_tidptr=0x7f28f8d109d0) = 3075
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f28f7d0f000
mprotect(0x7f28f7d10000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f28f850efb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f28f850f9d0, tls=0x7f28f850f700, child_tidptr=0x7f28f850f9d0) = 3076
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f28f6d0d000
mprotect(0x7f28f6d0e000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f28f750cfb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f28f750d9d0, tls=0x7f28f750d700, child_tidptr=0x7f28f750d9d0) = 3078
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
readlinkat(AT_FDCWD, "/proc/self/exe", "/usr/bin/snap", 128) = 13
mmap(NULL, 262144, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f28f9484000
stat("/sys/kernel/security/apparmor/features", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/caps", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/dbus", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/domain", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/file", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/mount", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/namespaces", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/network", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/ptrace", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/signal", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
openat(AT_FDCWD, "/etc/os-release", O_RDONLY|O_CLOEXEC) = 3
read(3, "NAME=\"Ubuntu\"\nVERSION=\"17.10 (Ar"..., 4096) = 376
read(3, "", 3720)                       = 0
stat("/snap/core/current/usr/share/locale-langpack/en_CA/LC_MESSAGES/snappy.mo", 0xc42008b078) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale/en_CA/LC_MESSAGES/snappy.mo", 0xc42008b148) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale-langpack/en_CA/LC_MESSAGES/snappy.mo", 0xc42008b218) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale/en_CA/LC_MESSAGES/snappy.mo", 0xc42008b2e8) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale-langpack/en/LC_MESSAGES/snappy.mo", 0xc42008b3b8) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale/en/LC_MESSAGES/snappy.mo", 0xc42008b488) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale-langpack/en/LC_MESSAGES/snappy.mo", 0xc42008b558) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale/en/LC_MESSAGES/snappy.mo", 0xc42008b628) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/sys/net/core/somaxconn", O_RDONLY|O_CLOEXEC) = 4
read(4, "128\n", 4096)                  = 4
read(4, "", 4092)                       = 0
close(4)                                = 0
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 4
close(4)                                = 0
socket(AF_INET6, SOCK_STREAM, IPPROTO_TCP) = 4
setsockopt(4, SOL_IPV6, IPV6_V6ONLY, [1], 4) = 0
bind(4, {sa_family=AF_INET6, sin6_port=htons(0), inet_pton(AF_INET6, "::1", &sin6_addr), sin6_flowinfo=htonl(0), sin6_scope_id=0}, 28) = 0
socket(AF_INET6, SOCK_STREAM, IPPROTO_TCP) = 5
setsockopt(5, SOL_IPV6, IPV6_V6ONLY, [0], 4) = 0
bind(5, {sa_family=AF_INET6, sin6_port=htons(0), inet_pton(AF_INET6, "::ffff:127.0.0.1", &sin6_addr), sin6_flowinfo=htonl(0), sin6_scope_id=0}, 28) = 0
close(5)                                = 0
close(4)                                = 0
mmap(0xc420100000, 1048576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc420100000
mmap(0xc41fff0000, 32768, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc41fff0000
mmap(NULL, 1439992, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f28f6bad000
ioctl(0, TCGETS, {B38400 opost isig icanon echo ...}) = 0
uname({sysname="Linux", nodename="ns1", ...}) = 0
readlinkat(AT_FDCWD, "/proc/self/exe", "/usr/bin/snap", 128) = 13
stat("/snap/core/current/usr/bin/snap", {st_mode=S_IFREG|0755, st_size=18088688, ...}) = 0
stat("/snap/core/current/usr/lib/snapd/info", {st_mode=S_IFREG|0644, st_size=15, ...}) = 0
openat(AT_FDCWD, "/snap/core/current/usr/lib/snapd/info", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=15, ...}) = 0
read(4, "VERSION=2.31.2\n", 527)        = 15
read(4, "", 512)                        = 0
close(4)                                = 0
execve("/snap/core/current/usr/bin/snap", ["rg", "shit"], [/* 112 vars */]) = 0
brk(NULL)                               = 0x55fd89946000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fabe6943000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360a\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=144776, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fabe6941000
mmap(NULL, 2221160, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fabe652b000
mprotect(0x7fabe6545000, 2093056, PROT_NONE) = 0
mmap(0x7fabe6744000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7fabe6744000
mmap(0x7fabe6746000, 13416, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fabe6746000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\22\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1960656, ...}) = 0
mmap(NULL, 4061792, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fabe614b000
mprotect(0x7fabe6321000, 2097152, PROT_NONE) = 0
mmap(0x7fabe6521000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d6000) = 0x7fabe6521000
mmap(0x7fabe6527000, 14944, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fabe6527000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fabe693e000
arch_prctl(ARCH_SET_FS, 0x7fabe693e740) = 0
mprotect(0x7fabe6521000, 16384, PROT_READ) = 0
mprotect(0x7fabe6744000, 4096, PROT_READ) = 0
mprotect(0x55fd88405000, 6176768, PROT_READ) = 0
mprotect(0x7fabe6971000, 4096, PROT_READ) = 0
munmap(0x7fabe6943000, 185135)          = 0
set_tid_address(0x7fabe693ea10)         = 3073
set_robust_list(0x7fabe693ea20, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7fabe6530c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7fabe653e150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7fabe6530d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7fabe653e150}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
brk(NULL)                               = 0x55fd89946000
brk(0x55fd89967000)                     = 0x55fd89967000
sched_getaffinity(0, 8192, [0, 1, 2, 3]) = 8
mmap(0xc000000000, 65536, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc000000000
munmap(0xc000000000, 65536)             = 0
mmap(NULL, 262144, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fabe68fe000
mmap(0xc820000000, 1048576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc820000000
mmap(0xc81fff8000, 32768, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc81fff8000
mmap(0xc000000000, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc000000000
mmap(NULL, 65536, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fabe6961000
rt_sigprocmask(SIG_SETMASK, NULL, [], 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
sigaltstack({ss_sp=0xc820002000, ss_flags=0, ss_size=32672}, NULL) = 0
gettid()                                = 3073
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
rt_sigaction(SIGHUP, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGINT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGINT, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGQUIT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGILL, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGILL, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGTRAP, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTRAP, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGABRT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGABRT, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGFPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGFPE, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGUSR1, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGUSR1, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGUSR2, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGUSR2, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGALRM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGALRM, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGTERM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGSTKFLT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSTKFLT, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGCHLD, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGCHLD, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGURG, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGURG, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGXCPU, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGXCPU, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGXFSZ, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGXFSZ, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGVTALRM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGVTALRM, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGPROF, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPROF, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGWINCH, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGWINCH, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGIO, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGIO, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGPWR, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPWR, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGSYS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSYS, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRTMIN, NULL, {sa_handler=0x7fabe6530c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7fabe653e150}, 8) = 0
rt_sigaction(SIGRTMIN, NULL, {sa_handler=0x7fabe6530c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7fabe653e150}, 8) = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7fabe6530c70, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_SIGINFO, sa_restorer=0x7fabe653e150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, NULL, {sa_handler=0x7fabe6530d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7fabe653e150}, 8) = 0
rt_sigaction(SIGRT_1, NULL, {sa_handler=0x7fabe6530d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7fabe653e150}, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7fabe6530d00, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7fabe653e150}, NULL, 8) = 0
rt_sigaction(SIGRT_2, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_2, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_3, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_3, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_4, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_4, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_5, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_5, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_6, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_6, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_7, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_7, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_8, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_8, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_9, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_9, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_10, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_10, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_11, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_11, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_12, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_12, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_13, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_13, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_14, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_14, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_15, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_15, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_16, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_16, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_17, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_17, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_18, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_18, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_19, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_19, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_20, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_20, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_21, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_21, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_22, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_22, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_23, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_23, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_24, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_24, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_25, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_25, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_26, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_26, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_27, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_27, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_28, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_28, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_29, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_29, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_30, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_30, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_31, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_31, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigaction(SIGRT_32, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_32, {sa_handler=0x55fd87cbfaa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd87cbfac0}, NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fabe594a000
mprotect(0x7fabe594b000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fabe6149fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7fabe614a9d0, tls=0x7fabe614a700, child_tidptr=0x7fabe614a9d0) = 3079
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
mmap(NULL, 262144, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fabe68be000
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fabe5149000
mprotect(0x7fabe514a000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fabe5948fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7fabe59499d0, tls=0x7fabe5949700, child_tidptr=0x7fabe59499d0) = 3080
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
futex(0xc820032908, FUTEX_WAKE, 1)      = 1
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fabe4948000
mprotect(0x7fabe4949000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fabe5147fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7fabe51489d0, tls=0x7fabe5148700, child_tidptr=0x7fabe51489d0) = 3081
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
futex(0x55fd88a2d3e0, FUTEX_WAKE, 1)    = 1
futex(0xc820064108, FUTEX_WAKE, 1)      = 1
stat("/sys/kernel/security/apparmor/features", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/caps", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/dbus", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/domain", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/file", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/mount", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/namespaces", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/network", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/ptrace", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/signal", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
openat(AT_FDCWD, "/etc/os-release", O_RDONLY|O_CLOEXEC) = 3
read(3, "NAME=\"Ubuntu\"\nVERSION=\"17.10 (Ar"..., 4096) = 376
read(3, "", 3720)                       = 0
stat("/snap/core/current/usr/share/locale-langpack/en_CA/LC_MESSAGES/snappy.mo", 0xc820071ca8) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale/en_CA/LC_MESSAGES/snappy.mo", 0xc820071d78) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale-langpack/en_CA/LC_MESSAGES/snappy.mo", 0xc820071e48) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale/en_CA/LC_MESSAGES/snappy.mo", 0xc820071f18) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale-langpack/en/LC_MESSAGES/snappy.mo", 0xc8200a8038) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale/en/LC_MESSAGES/snappy.mo", 0xc8200a8108) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale-langpack/en/LC_MESSAGES/snappy.mo", 0xc8200a81d8) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale/en/LC_MESSAGES/snappy.mo", 0xc8200a82a8) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/sys/net/core/somaxconn", O_RDONLY|O_CLOEXEC) = 4
read(4, "128\n", 4096)                  = 4
read(4, "", 4092)                       = 0
close(4)                                = 0
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 4
close(4)                                = 0
socket(AF_INET6, SOCK_STREAM, IPPROTO_TCP) = 4
setsockopt(4, SOL_IPV6, IPV6_V6ONLY, [1], 4) = 0
bind(4, {sa_family=AF_INET6, sin6_port=htons(0), inet_pton(AF_INET6, "::1", &sin6_addr), sin6_flowinfo=htonl(0), sin6_scope_id=0}, 28) = 0
socket(AF_INET6, SOCK_STREAM, IPPROTO_TCP) = 5
setsockopt(5, SOL_IPV6, IPV6_V6ONLY, [0], 4) = 0
bind(5, {sa_family=AF_INET6, sin6_port=htons(0), inet_pton(AF_INET6, "::ffff:127.0.0.1", &sin6_addr), sin6_flowinfo=htonl(0), sin6_scope_id=0}, 28) = 0
close(5)                                = 0
close(4)                                = 0
mmap(0xc820100000, 1048576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc820100000
mmap(0xc81fff0000, 32768, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc81fff0000
futex(0xc820032908, FUTEX_WAKE, 1)      = 1
ioctl(0, TCGETS, {B38400 opost isig icanon echo ...}) = 0
uname({sysname="Linux", nodename="ns1", ...}) = 0
readlinkat(AT_FDCWD, "/proc/self/exe", "/snap/core/4206/usr/bin/snap", 128) = 28
lstat("/snap/bin/rg", {st_mode=S_IFLNK|0777, st_size=13, ...}) = 0
readlinkat(AT_FDCWD, "/snap/bin/rg", "/usr/bin/snap", 128) = 13
readlinkat(AT_FDCWD, "/snap/rg/current", "15", 128) = 2
openat(AT_FDCWD, "/snap/rg/15/meta/snap.yaml", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=543, ...}) = 0
read(4, "name: rg\nversion: 0.8.1\nsummary:"..., 1055) = 543
read(4, "", 512)                        = 0
close(4)                                = 0
stat("/var/lib/snapd/snaps/rg_15.snap", {st_mode=S_IFREG|0644, st_size=7364608, ...}) = 0
stat("/snap/rg/15/meta/hooks", 0xc8200a9ca8) = -1 ENOENT (No such file or directory)
readlinkat(AT_FDCWD, "/proc/self/exe", "/snap/core/4206/usr/bin/snap", 128) = 28
stat("/snap/core/current/usr/lib/snapd/snap-confine", {st_mode=S_IFREG|S_ISUID|S_ISGID|0755, st_size=94056, ...}) = 0
getuid()                                = 1000
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 4
connect(4, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(4)                                = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 4
connect(4, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(4)                                = 0
open("/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=540, ...}) = 0
read(4, "# /etc/nsswitch.conf\n#\n# Example"..., 4096) = 540
read(4, "", 4096)                       = 0
close(4)                                = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 4, 0) = 0x7fabe6890000
close(4)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_compat.so.2", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0 \23\0\0\0\0\0\0"..., 832) = 832
fstat(4, {st_mode=S_IFREG|0644, st_size=35720, ...}) = 0
mmap(NULL, 2131072, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7fabd7df7000
mprotect(0x7fabd7dff000, 2093056, PROT_NONE) = 0
mmap(0x7fabd7ffe000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0x7000) = 0x7fabd7ffe000
close(4)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnsl.so.1", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\320@\0\0\0\0\0\0"..., 832) = 832
fstat(4, {st_mode=S_IFREG|0644, st_size=97248, ...}) = 0
mmap(NULL, 2202200, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7fabd7bdd000
mprotect(0x7fabd7bf4000, 2093056, PROT_NONE) = 0
mmap(0x7fabd7df3000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0x16000) = 0x7fabd7df3000
mmap(0x7fabd7df5000, 6744, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fabd7df5000
close(4)                                = 0
mprotect(0x7fabd7df3000, 4096, PROT_READ) = 0
mprotect(0x7fabd7ffe000, 4096, PROT_READ) = 0
munmap(0x7fabe6890000, 185135)          = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 4, 0) = 0x7fabe6890000
close(4)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_nis.so.2", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0!\0\0\0\0\0\0"..., 832) = 832
fstat(4, {st_mode=S_IFREG|0644, st_size=47656, ...}) = 0
mmap(NULL, 2143656, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7fabd79d1000
mprotect(0x7fabd79dc000, 2093056, PROT_NONE) = 0
mmap(0x7fabd7bdb000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0xa000) = 0x7fabd7bdb000
close(4)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_files.so.2", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360!\0\0\0\0\0\0"..., 832) = 832
fstat(4, {st_mode=S_IFREG|0644, st_size=47608, ...}) = 0
mmap(NULL, 2168600, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7fabd77bf000
mprotect(0x7fabd77ca000, 2093056, PROT_NONE) = 0
mmap(0x7fabd79c9000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0xa000) = 0x7fabd79c9000
mmap(0x7fabd79cb000, 22296, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fabd79cb000
close(4)                                = 0
mprotect(0x7fabd79c9000, 4096, PROT_READ) = 0
mprotect(0x7fabd7bdb000, 4096, PROT_READ) = 0
munmap(0x7fabe6890000, 185135)          = 0
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=2469, ...}) = 0
mmap(NULL, 2469, PROT_READ, MAP_SHARED, 4, 0) = 0x7fabe6960000
lseek(4, 2469, SEEK_SET)                = 2469
munmap(0x7fabe6960000, 2469)            = 0
close(4)                                = 0
stat("/home/jared/snap/rg/15", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
stat("/home/jared/snap/rg/common", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
readlinkat(AT_FDCWD, "/home/jared/snap/rg/current", "15", 128) = 2
getuid()                                = 1000
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=2469, ...}) = 0
mmap(NULL, 2469, PROT_READ, MAP_SHARED, 4, 0) = 0x7fabe6960000
lseek(4, 2469, SEEK_SET)                = 2469
munmap(0x7fabe6960000, 2469)            = 0
close(4)                                = 0
stat("/run/user/1000", {st_mode=S_IFDIR|0700, st_size=280, ...}) = 0
stat("/run/user/1000/gdm/Xauthority", {st_mode=S_IFREG|0700, st_size=94, ...}) = 0
openat(AT_FDCWD, "/run/user/1000/gdm/Xauthority", O_RDONLY|O_CLOEXEC) = 4
lstat("/run", {st_mode=S_IFDIR|0755, st_size=1120, ...}) = 0
lstat("/run/user", {st_mode=S_IFDIR|0755, st_size=80, ...}) = 0
lstat("/run/user/1000", {st_mode=S_IFDIR|0700, st_size=280, ...}) = 0
lstat("/run/user/1000/gdm", {st_mode=S_IFDIR|0711, st_size=60, ...}) = 0
lstat("/run/user/1000/gdm/Xauthority", {st_mode=S_IFREG|0700, st_size=94, ...}) = 0
close(4)                                = 0
getuid()                                = 1000
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=2469, ...}) = 0
mmap(NULL, 2469, PROT_READ, MAP_SHARED, 4, 0) = 0x7fabe6960000
lseek(4, 2469, SEEK_SET)                = 2469
munmap(0x7fabe6960000, 2469)            = 0
close(4)                                = 0
geteuid()                               = 1000
execve("/snap/core/current/usr/lib/snapd/snap-confine", ["/snap/core/current/usr/lib/snapd"..., "snap.rg.rg", "/usr/lib/snapd/snap-exec", "rg", "shit"], [/* 123 vars */]) = 0
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
brk(NULL)                               = 0xafc000
fcntl(0, F_GETFD)                       = 0
fcntl(1, F_GETFD)                       = 0
fcntl(2, F_GETFD)                       = 0
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f752bbde000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libudev.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\3206\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=116920, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f752bbdc000
mmap(NULL, 2214136, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f752b7c8000
mprotect(0x7f752b7e4000, 2093056, PROT_NONE) = 0
mmap(0x7f752b9e3000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0x7f752b9e3000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360a\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=144776, ...}) = 0
mmap(NULL, 2221160, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f752b5a9000
mprotect(0x7f752b5c3000, 2093056, PROT_NONE) = 0
mmap(0x7f752b7c2000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7f752b7c2000
mmap(0x7f752b7c4000, 13416, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f752b7c4000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\22\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1960656, ...}) = 0
mmap(NULL, 4061792, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f752b1c9000
mprotect(0x7f752b39f000, 2097152, PROT_NONE) = 0
mmap(0x7f752b59f000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d6000) = 0x7f752b59f000
mmap(0x7f752b5a5000, 14944, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f752b5a5000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f752bbd9000
arch_prctl(ARCH_SET_FS, 0x7f752bbd9740) = 0
mprotect(0x7f752b59f000, 16384, PROT_READ) = 0
mprotect(0x7f752b7c2000, 4096, PROT_READ) = 0
mprotect(0x7f752b9e3000, 4096, PROT_READ) = 0
mprotect(0x615000, 4096, PROT_READ)     = 0
mprotect(0x7f752bc0c000, 4096, PROT_READ) = 0
munmap(0x7f752bbde000, 185135)          = 0
set_tid_address(0x7f752bbd9a10)         = 3073
set_robust_list(0x7f752bbd9a20, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f752b5aec70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f752b5bc150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f752b5aed00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f752b5bc150}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
brk(NULL)                               = 0xafc000
brk(0xb1d000)                           = 0xb1d000
getresuid([1000], [1000], [1000])       = 0
getresgid([1000], [1000], [1000])       = 0
geteuid()                               = 1000
write(2, "need to run as root or suid", 27need to run as root or suid) = 27
write(2, "\n", 1
)                       = 1
exit_group(1)                           = ?
+++ exited with 1 +++
```

---

_Comment by @shmup on 2018-03-20 00:23_

And the other request:

```
execve("/snap/bin/rg", ["rg", "shit", "./"], [/* 111 vars */]) = 0
brk(NULL)                               = 0x55a3e6cf6000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f0189dd2000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360a\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=144776, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0189dd0000
mmap(NULL, 2221160, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f01899ba000
mprotect(0x7f01899d4000, 2093056, PROT_NONE) = 0
mmap(0x7f0189bd3000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7f0189bd3000
mmap(0x7f0189bd5000, 13416, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f0189bd5000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\22\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1960656, ...}) = 0
mmap(NULL, 4061792, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f01895da000
mprotect(0x7f01897b0000, 2097152, PROT_NONE) = 0
mmap(0x7f01899b0000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d6000) = 0x7f01899b0000
mmap(0x7f01899b6000, 14944, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f01899b6000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0189dcd000
arch_prctl(ARCH_SET_FS, 0x7f0189dcd740) = 0
mprotect(0x7f01899b0000, 16384, PROT_READ) = 0
mprotect(0x7f0189bd3000, 4096, PROT_READ) = 0
mprotect(0x55a3e518c000, 3584000, PROT_READ) = 0
mprotect(0x7f0189e00000, 4096, PROT_READ) = 0
munmap(0x7f0189dd2000, 185135)          = 0
set_tid_address(0x7f0189dcda10)         = 4143
set_robust_list(0x7f0189dcda20, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f01899bfc70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f01899bfd00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
brk(NULL)                               = 0x55a3e6cf6000
brk(0x55a3e6d17000)                     = 0x55a3e6d17000
sched_getaffinity(0, 8192, [0, 1, 2, 3]) = 8
mmap(0xc000000000, 65536, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc000000000
munmap(0xc000000000, 65536)             = 0
mmap(NULL, 262144, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0189d8d000
mmap(0xc420000000, 1048576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc420000000
mmap(0xc41fff8000, 32768, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc41fff8000
mmap(0xc000000000, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc000000000
mmap(NULL, 65536, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0189df0000
mmap(NULL, 65536, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0189de0000
rt_sigprocmask(SIG_SETMASK, NULL, [], 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
sigaltstack({ss_sp=0xc420002000, ss_flags=0, ss_size=32672}, NULL) = 0
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
gettid()                                = 4143
rt_sigaction(SIGHUP, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGINT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGINT, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGQUIT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGILL, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGILL, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGTRAP, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTRAP, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGABRT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGABRT, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGFPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGFPE, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGUSR1, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGUSR1, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGUSR2, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGUSR2, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGALRM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGALRM, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGTERM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGSTKFLT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSTKFLT, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGCHLD, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGCHLD, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGURG, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGURG, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGXCPU, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGXCPU, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGXFSZ, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGXFSZ, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGVTALRM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGVTALRM, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGPROF, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPROF, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGWINCH, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGWINCH, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGIO, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGIO, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGPWR, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPWR, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGSYS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSYS, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRTMIN, NULL, {sa_handler=0x7f01899bfc70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f01899cd150}, 8) = 0
rt_sigaction(SIGRTMIN, NULL, {sa_handler=0x7f01899bfc70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f01899cd150}, 8) = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f01899bfc70, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, NULL, {sa_handler=0x7f01899bfd00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, 8) = 0
rt_sigaction(SIGRT_1, NULL, {sa_handler=0x7f01899bfd00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f01899bfd00, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_2, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_2, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_3, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_3, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_4, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_4, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_5, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_5, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_6, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_6, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_7, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_7, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_8, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_8, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_9, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_9, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_10, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_10, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_11, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_11, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_12, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_12, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_13, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_13, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_14, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_14, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_15, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_15, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_16, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_16, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_17, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_17, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_18, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_18, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_19, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_19, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_20, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_20, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_21, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_21, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_22, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_22, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_23, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_23, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_24, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_24, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_25, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_25, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_26, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_26, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_27, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_27, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_28, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_28, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_29, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_29, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_30, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_30, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_31, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_31, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigaction(SIGRT_32, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_32, {sa_handler=0x55a3e4ad9e30, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f01899cd150}, NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f0188dd9000
mprotect(0x7f0188dda000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f01895d8fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f01895d99d0, tls=0x7f01895d9700, child_tidptr=0x7f01895d99d0) = 4144
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f01885d8000
mprotect(0x7f01885d9000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f0188dd7fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f0188dd89d0, tls=0x7f0188dd8700, child_tidptr=0x7f0188dd89d0) = 4146
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f01837ff000
mprotect(0x7f0183800000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f0183ffefb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f0183fff9d0, tls=0x7f0183fff700, child_tidptr=0x7f0183fff9d0) = 4147
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
futex(0x55a3e55388f0, FUTEX_WAIT, 0, NULL) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f0182ffe000
mprotect(0x7f0182fff000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f01837fdfb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f01837fe9d0, tls=0x7f01837fe700, child_tidptr=0x7f01837fe9d0) = 4148
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
futex(0x55a3e55388f0, FUTEX_WAIT, 0, NULL) = 0
readlinkat(AT_FDCWD, "/proc/self/exe", "/usr/bin/snap", 128) = 13
futex(0xc420035110, FUTEX_WAKE, 1)      = 1
mmap(NULL, 262144, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0189d4d000
stat("/sys/kernel/security/apparmor/features", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/caps", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/dbus", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/domain", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/file", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/mount", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/namespaces", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/network", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/ptrace", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/signal", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
openat(AT_FDCWD, "/etc/os-release", O_RDONLY|O_CLOEXEC) = 3
read(3, "NAME=\"Ubuntu\"\nVERSION=\"17.10 (Ar"..., 4096) = 376
read(3, "", 3720)                       = 0
stat("/snap/core/current/usr/share/locale-langpack/en_CA/LC_MESSAGES/snappy.mo", 0xc420079218) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale/en_CA/LC_MESSAGES/snappy.mo", 0xc4200792e8) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale-langpack/en_CA/LC_MESSAGES/snappy.mo", 0xc4200793b8) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale/en_CA/LC_MESSAGES/snappy.mo", 0xc420079488) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale-langpack/en/LC_MESSAGES/snappy.mo", 0xc420079558) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale/en/LC_MESSAGES/snappy.mo", 0xc420079628) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale-langpack/en/LC_MESSAGES/snappy.mo", 0xc4200796f8) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale/en/LC_MESSAGES/snappy.mo", 0xc4200797c8) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/sys/net/core/somaxconn", O_RDONLY|O_CLOEXEC) = 4
read(4, "128\n", 4096)                  = 4
read(4, "", 4092)                       = 0
close(4)                                = 0
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 4
close(4)                                = 0
socket(AF_INET6, SOCK_STREAM, IPPROTO_TCP) = 4
setsockopt(4, SOL_IPV6, IPV6_V6ONLY, [1], 4) = 0
bind(4, {sa_family=AF_INET6, sin6_port=htons(0), inet_pton(AF_INET6, "::1", &sin6_addr), sin6_flowinfo=htonl(0), sin6_scope_id=0}, 28) = 0
socket(AF_INET6, SOCK_STREAM, IPPROTO_TCP) = 5
setsockopt(5, SOL_IPV6, IPV6_V6ONLY, [0], 4) = 0
bind(5, {sa_family=AF_INET6, sin6_port=htons(0), inet_pton(AF_INET6, "::ffff:127.0.0.1", &sin6_addr), sin6_flowinfo=htonl(0), sin6_scope_id=0}, 28) = 0
close(5)                                = 0
close(4)                                = 0
mmap(0xc420100000, 1048576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc420100000
mmap(0xc41fff0000, 32768, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc41fff0000
futex(0xc420034910, FUTEX_WAKE, 1)      = 1
ioctl(0, TCGETS, {B38400 opost isig icanon echo ...}) = 0
uname({sysname="Linux", nodename="ns1", ...}) = 0
readlinkat(AT_FDCWD, "/proc/self/exe", "/usr/bin/snap", 128) = 13
stat("/snap/core/current/usr/bin/snap", {st_mode=S_IFREG|0755, st_size=18088688, ...}) = 0
stat("/snap/core/current/usr/lib/snapd/info", {st_mode=S_IFREG|0644, st_size=15, ...}) = 0
openat(AT_FDCWD, "/snap/core/current/usr/lib/snapd/info", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=15, ...}) = 0
read(4, "VERSION=2.31.2\n", 527)        = 15
read(4, "", 512)                        = 0
close(4)                                = 0
execve("/snap/core/current/usr/bin/snap", ["rg", "shit", "./"], [/* 112 vars */]) = 0
brk(NULL)                               = 0x55fd68612000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f870eeac000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360a\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=144776, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f870eeaa000
mmap(NULL, 2221160, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f870ea94000
mprotect(0x7f870eaae000, 2093056, PROT_NONE) = 0
mmap(0x7f870ecad000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7f870ecad000
mmap(0x7f870ecaf000, 13416, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f870ecaf000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\22\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1960656, ...}) = 0
mmap(NULL, 4061792, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f870e6b4000
mprotect(0x7f870e88a000, 2097152, PROT_NONE) = 0
mmap(0x7f870ea8a000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d6000) = 0x7f870ea8a000
mmap(0x7f870ea90000, 14944, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f870ea90000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f870eea7000
arch_prctl(ARCH_SET_FS, 0x7f870eea7740) = 0
mprotect(0x7f870ea8a000, 16384, PROT_READ) = 0
mprotect(0x7f870ecad000, 4096, PROT_READ) = 0
mprotect(0x55fd6724f000, 6176768, PROT_READ) = 0
mprotect(0x7f870eeda000, 4096, PROT_READ) = 0
munmap(0x7f870eeac000, 185135)          = 0
set_tid_address(0x7f870eea7a10)         = 4143
set_robust_list(0x7f870eea7a20, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f870ea99c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f870eaa7150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f870ea99d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f870eaa7150}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
brk(NULL)                               = 0x55fd68612000
brk(0x55fd68633000)                     = 0x55fd68633000
sched_getaffinity(0, 8192, [0, 1, 2, 3]) = 8
mmap(0xc000000000, 65536, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc000000000
munmap(0xc000000000, 65536)             = 0
mmap(NULL, 262144, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f870ee67000
mmap(0xc820000000, 1048576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc820000000
mmap(0xc81fff8000, 32768, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc81fff8000
mmap(0xc000000000, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc000000000
mmap(NULL, 65536, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f870eeca000
rt_sigprocmask(SIG_SETMASK, NULL, [], 8) = 0
sigaltstack(NULL, {ss_sp=NULL, ss_flags=SS_DISABLE, ss_size=0}) = 0
sigaltstack({ss_sp=0xc820002000, ss_flags=0, ss_size=32672}, NULL) = 0
gettid()                                = 4143
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
rt_sigaction(SIGHUP, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGHUP, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGINT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGINT, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGQUIT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGILL, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGILL, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGTRAP, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTRAP, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGABRT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGABRT, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGBUS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGBUS, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGFPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGFPE, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGUSR1, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGUSR1, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGSEGV, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSEGV, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGUSR2, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGUSR2, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGPIPE, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPIPE, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGALRM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGALRM, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGTERM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGSTKFLT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSTKFLT, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGCHLD, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGCHLD, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGURG, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGURG, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGXCPU, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGXCPU, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGXFSZ, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGXFSZ, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGVTALRM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGVTALRM, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGPROF, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPROF, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGWINCH, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGWINCH, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGIO, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGIO, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGPWR, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGPWR, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGSYS, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGSYS, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRTMIN, NULL, {sa_handler=0x7f870ea99c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f870eaa7150}, 8) = 0
rt_sigaction(SIGRTMIN, NULL, {sa_handler=0x7f870ea99c70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f870eaa7150}, 8) = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f870ea99c70, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_SIGINFO, sa_restorer=0x7f870eaa7150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, NULL, {sa_handler=0x7f870ea99d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f870eaa7150}, 8) = 0
rt_sigaction(SIGRT_1, NULL, {sa_handler=0x7f870ea99d00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f870eaa7150}, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f870ea99d00, sa_mask=[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f870eaa7150}, NULL, 8) = 0
rt_sigaction(SIGRT_2, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_2, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_3, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_3, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_4, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_4, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_5, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_5, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_6, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_6, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_7, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_7, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_8, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_8, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_9, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_9, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_10, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_10, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_11, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_11, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_12, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_12, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_13, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_13, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_14, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_14, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_15, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_15, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_16, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_16, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_17, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_17, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_18, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_18, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_19, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_19, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_20, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_20, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_21, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_21, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_22, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_22, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_23, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_23, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_24, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_24, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_25, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_25, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_26, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_26, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_27, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_27, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_28, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_28, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_29, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_29, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_30, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_30, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_31, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_31, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigaction(SIGRT_32, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGRT_32, {sa_handler=0x55fd66b09aa0, sa_mask=~[], sa_flags=SA_RESTORER|SA_STACK|SA_RESTART|SA_SIGINFO, sa_restorer=0x55fd66b09ac0}, NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f870deb3000
mprotect(0x7f870deb4000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f870e6b2fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f870e6b39d0, tls=0x7f870e6b3700, child_tidptr=0x7f870e6b39d0) = 4156
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
mmap(NULL, 262144, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f870ee27000
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f870d6b2000
mprotect(0x7f870d6b3000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f870deb1fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f870deb29d0, tls=0x7f870deb2700, child_tidptr=0x7f870deb29d0) = 4157
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
rt_sigprocmask(SIG_SETMASK, ~[RTMIN RT_1], [], 8) = 0
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f870ceb1000
mprotect(0x7f870ceb2000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7f870d6b0fb0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f870d6b19d0, tls=0x7f870d6b1700, child_tidptr=0x7f870d6b19d0) = 4158
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
futex(0x55fd678773e0, FUTEX_WAKE, 1)    = 1
futex(0xc820032908, FUTEX_WAKE, 1)      = 1
futex(0xc820064108, FUTEX_WAKE, 1)      = 1
stat("/sys/kernel/security/apparmor/features", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/caps", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/dbus", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/domain", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/file", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/mount", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/namespaces", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/network", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/ptrace", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
stat("/sys/kernel/security/apparmor/features/signal", {st_mode=S_IFDIR|0755, st_size=0, ...}) = 0
openat(AT_FDCWD, "/etc/os-release", O_RDONLY|O_CLOEXEC) = 3
read(3, "NAME=\"Ubuntu\"\nVERSION=\"17.10 (Ar"..., 4096) = 376
read(3, "", 3720)                       = 0
stat("/snap/core/current/usr/share/locale-langpack/en_CA/LC_MESSAGES/snappy.mo", 0xc820071ca8) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale/en_CA/LC_MESSAGES/snappy.mo", 0xc820071d78) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale-langpack/en_CA/LC_MESSAGES/snappy.mo", 0xc820071e48) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale/en_CA/LC_MESSAGES/snappy.mo", 0xc820071f18) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale-langpack/en/LC_MESSAGES/snappy.mo", 0xc8200a8038) = -1 ENOENT (No such file or directory)
stat("/snap/core/current/usr/share/locale/en/LC_MESSAGES/snappy.mo", 0xc8200a8108) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale-langpack/en/LC_MESSAGES/snappy.mo", 0xc8200a81d8) = -1 ENOENT (No such file or directory)
stat("/usr/share/locale/en/LC_MESSAGES/snappy.mo", 0xc8200a82a8) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/proc/sys/net/core/somaxconn", O_RDONLY|O_CLOEXEC) = 4
read(4, "128\n", 4096)                  = 4
read(4, "", 4092)                       = 0
close(4)                                = 0
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 4
close(4)                                = 0
socket(AF_INET6, SOCK_STREAM, IPPROTO_TCP) = 4
setsockopt(4, SOL_IPV6, IPV6_V6ONLY, [1], 4) = 0
bind(4, {sa_family=AF_INET6, sin6_port=htons(0), inet_pton(AF_INET6, "::1", &sin6_addr), sin6_flowinfo=htonl(0), sin6_scope_id=0}, 28) = 0
socket(AF_INET6, SOCK_STREAM, IPPROTO_TCP) = 5
setsockopt(5, SOL_IPV6, IPV6_V6ONLY, [0], 4) = 0
bind(5, {sa_family=AF_INET6, sin6_port=htons(0), inet_pton(AF_INET6, "::ffff:127.0.0.1", &sin6_addr), sin6_flowinfo=htonl(0), sin6_scope_id=0}, 28) = 0
close(5)                                = 0
close(4)                                = 0
mmap(0xc820100000, 1048576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc820100000
mmap(0xc81fff0000, 32768, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xc81fff0000
futex(0xc820032908, FUTEX_WAKE, 1)      = 1
ioctl(0, TCGETS, {B38400 opost isig icanon echo ...}) = 0
uname({sysname="Linux", nodename="ns1", ...}) = 0
readlinkat(AT_FDCWD, "/proc/self/exe", "/snap/core/4206/usr/bin/snap", 128) = 28
lstat("/snap/bin/rg", {st_mode=S_IFLNK|0777, st_size=13, ...}) = 0
readlinkat(AT_FDCWD, "/snap/bin/rg", "/usr/bin/snap", 128) = 13
readlinkat(AT_FDCWD, "/snap/rg/current", "15", 128) = 2
openat(AT_FDCWD, "/snap/rg/15/meta/snap.yaml", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=543, ...}) = 0
read(4, "name: rg\nversion: 0.8.1\nsummary:"..., 1055) = 543
read(4, "", 512)                        = 0
close(4)                                = 0
stat("/var/lib/snapd/snaps/rg_15.snap", {st_mode=S_IFREG|0644, st_size=7364608, ...}) = 0
stat("/snap/rg/15/meta/hooks", 0xc8201e0038) = -1 ENOENT (No such file or directory)
readlinkat(AT_FDCWD, "/proc/self/exe", "/snap/core/4206/usr/bin/snap", 128) = 28
stat("/snap/core/current/usr/lib/snapd/snap-confine", {st_mode=S_IFREG|S_ISUID|S_ISGID|0755, st_size=94056, ...}) = 0
getuid()                                = 1000
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 4
connect(4, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(4)                                = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 4
connect(4, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(4)                                = 0
open("/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=540, ...}) = 0
read(4, "# /etc/nsswitch.conf\n#\n# Example"..., 4096) = 540
read(4, "", 4096)                       = 0
close(4)                                = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 4, 0) = 0x7f870edf9000
close(4)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_compat.so.2", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0 \23\0\0\0\0\0\0"..., 832) = 832
fstat(4, {st_mode=S_IFREG|0644, st_size=35720, ...}) = 0
mmap(NULL, 2131072, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7f870c4a7000
mprotect(0x7f870c4af000, 2093056, PROT_NONE) = 0
mmap(0x7f870c6ae000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0x7000) = 0x7f870c6ae000
close(4)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnsl.so.1", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\320@\0\0\0\0\0\0"..., 832) = 832
fstat(4, {st_mode=S_IFREG|0644, st_size=97248, ...}) = 0
mmap(NULL, 2202200, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7f870c28d000
mprotect(0x7f870c2a4000, 2093056, PROT_NONE) = 0
mmap(0x7f870c4a3000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0x16000) = 0x7f870c4a3000
mmap(0x7f870c4a5000, 6744, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f870c4a5000
close(4)                                = 0
mprotect(0x7f870c4a3000, 4096, PROT_READ) = 0
mprotect(0x7f870c6ae000, 4096, PROT_READ) = 0
munmap(0x7f870edf9000, 185135)          = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
fstat(4, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 4, 0) = 0x7f870edf9000
close(4)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_nis.so.2", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0!\0\0\0\0\0\0"..., 832) = 832
fstat(4, {st_mode=S_IFREG|0644, st_size=47656, ...}) = 0
mmap(NULL, 2143656, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7f870c081000
mprotect(0x7f870c08c000, 2093056, PROT_NONE) = 0
mmap(0x7f870c28b000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0xa000) = 0x7f870c28b000
close(4)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_files.so.2", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360!\0\0\0\0\0\0"..., 832) = 832
fstat(4, {st_mode=S_IFREG|0644, st_size=47608, ...}) = 0
mmap(NULL, 2168600, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7f86ffdee000
mprotect(0x7f86ffdf9000, 2093056, PROT_NONE) = 0
mmap(0x7f86ffff8000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0xa000) = 0x7f86ffff8000
mmap(0x7f86ffffa000, 22296, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f86ffffa000
close(4)                                = 0
mprotect(0x7f86ffff8000, 4096, PROT_READ) = 0
mprotect(0x7f870c28b000, 4096, PROT_READ) = 0
munmap(0x7f870edf9000, 185135)          = 0
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=2469, ...}) = 0
mmap(NULL, 2469, PROT_READ, MAP_SHARED, 4, 0) = 0x7f870eec9000
lseek(4, 2469, SEEK_SET)                = 2469
munmap(0x7f870eec9000, 2469)            = 0
close(4)                                = 0
stat("/home/jared/snap/rg/15", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
stat("/home/jared/snap/rg/common", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
readlinkat(AT_FDCWD, "/home/jared/snap/rg/current", "15", 128) = 2
getuid()                                = 1000
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=2469, ...}) = 0
mmap(NULL, 2469, PROT_READ, MAP_SHARED, 4, 0) = 0x7f870eec9000
lseek(4, 2469, SEEK_SET)                = 2469
munmap(0x7f870eec9000, 2469)            = 0
close(4)                                = 0
stat("/run/user/1000", {st_mode=S_IFDIR|0700, st_size=280, ...}) = 0
stat("/run/user/1000/gdm/Xauthority", {st_mode=S_IFREG|0700, st_size=94, ...}) = 0
openat(AT_FDCWD, "/run/user/1000/gdm/Xauthority", O_RDONLY|O_CLOEXEC) = 4
lstat("/run", {st_mode=S_IFDIR|0755, st_size=1120, ...}) = 0
lstat("/run/user", {st_mode=S_IFDIR|0755, st_size=80, ...}) = 0
lstat("/run/user/1000", {st_mode=S_IFDIR|0700, st_size=280, ...}) = 0
lstat("/run/user/1000/gdm", {st_mode=S_IFDIR|0711, st_size=60, ...}) = 0
lstat("/run/user/1000/gdm/Xauthority", {st_mode=S_IFREG|0700, st_size=94, ...}) = 0
close(4)                                = 0
getuid()                                = 1000
openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 4
lseek(4, 0, SEEK_CUR)                   = 0
fstat(4, {st_mode=S_IFREG|0644, st_size=2469, ...}) = 0
mmap(NULL, 2469, PROT_READ, MAP_SHARED, 4, 0) = 0x7f870eec9000
lseek(4, 2469, SEEK_SET)                = 2469
munmap(0x7f870eec9000, 2469)            = 0
close(4)                                = 0
geteuid()                               = 1000
execve("/snap/core/current/usr/lib/snapd/snap-confine", ["/snap/core/current/usr/lib/snapd"..., "snap.rg.rg", "/usr/lib/snapd/snap-exec", "rg", "shit", "./"], [/* 123 vars */]) = 0
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
brk(NULL)                               = 0xd91000
fcntl(0, F_GETFD)                       = 0
fcntl(1, F_GETFD)                       = 0
fcntl(2, F_GETFD)                       = 0
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=185135, ...}) = 0
mmap(NULL, 185135, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fb86f57c000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libudev.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\3206\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=116920, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fb86f57a000
mmap(NULL, 2214136, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fb86f166000
mprotect(0x7fb86f182000, 2093056, PROT_NONE) = 0
mmap(0x7fb86f381000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0x7fb86f381000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\360a\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=144776, ...}) = 0
mmap(NULL, 2221160, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fb86ef47000
mprotect(0x7fb86ef61000, 2093056, PROT_NONE) = 0
mmap(0x7fb86f160000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19000) = 0x7fb86f160000
mmap(0x7fb86f162000, 13416, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fb86f162000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\22\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1960656, ...}) = 0
mmap(NULL, 4061792, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fb86eb67000
mprotect(0x7fb86ed3d000, 2097152, PROT_NONE) = 0
mmap(0x7fb86ef3d000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1d6000) = 0x7fb86ef3d000
mmap(0x7fb86ef43000, 14944, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fb86ef43000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fb86f577000
arch_prctl(ARCH_SET_FS, 0x7fb86f577740) = 0
mprotect(0x7fb86ef3d000, 16384, PROT_READ) = 0
mprotect(0x7fb86f160000, 4096, PROT_READ) = 0
mprotect(0x7fb86f381000, 4096, PROT_READ) = 0
mprotect(0x615000, 4096, PROT_READ)     = 0
mprotect(0x7fb86f5aa000, 4096, PROT_READ) = 0
munmap(0x7fb86f57c000, 185135)          = 0
set_tid_address(0x7fb86f577a10)         = 4143
set_robust_list(0x7fb86f577a20, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7fb86ef4cc70, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7fb86ef5a150}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7fb86ef4cd00, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7fb86ef5a150}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
brk(NULL)                               = 0xd91000
brk(0xdb2000)                           = 0xdb2000
getresuid([1000], [1000], [1000])       = 0
getresgid([1000], [1000], [1000])       = 0
geteuid()                               = 1000
write(2, "need to run as root or suid", 27need to run as root or suid) = 27
write(2, "\n", 1
)                       = 1
exit_group(1)                           = ?
+++ exited with 1 +++
```

Ok I'll parse this now too heh

---

_Comment by @BurntSushi on 2018-03-20 01:05_

You used snap to install ripgrep? I think snap has permission issues. You might want to google it.

---

_Comment by @shmup on 2018-03-20 01:14_

Interesting, I will absolutely google it, thanks. I have no marriage to snap. I will say, though (before I've googled), that I pointed out the permissions on `.vim/` above because I've compared permissions in other areas I heavily `rg` and I can't determine why it'd work there (and not in .vim).

I'll report back.

---

_Comment by @shmup on 2018-03-20 01:16_

Eh screw it, you're at least correct, and doing `snap install rg --classic` fixes the problem.

Possibly it'd be nice to know _why_ it happened specifically there and not in other directories, and the answer is probably right in front of me. But, closing this -- thanks.

edit: lol this is a lesson in reading a ... README: you even say this:

> If you get permission errors when running ripgrep after installation, try uninstalling and then re-installing with the --classic flag.

---

_Closed by @shmup on 2018-03-20 01:16_

---

_Comment by @BurntSushi on 2018-03-20 01:24_

Yeah I don't know why. It seems to be a common problem with snap.

---
