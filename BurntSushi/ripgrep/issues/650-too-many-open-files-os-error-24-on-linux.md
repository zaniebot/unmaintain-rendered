```yaml
number: 650
title: Too many open files (os error 24) on Linux
type: issue
state: closed
author: erg
labels: []
assignees: []
created_at: 2017-10-23T16:07:26Z
updated_at: 2017-10-23T16:24:35Z
url: https://github.com/BurntSushi/ripgrep/issues/650
synced_at: 2026-01-12T16:13:22Z
```

# Too many open files (os error 24) on Linux

---

_@erg_

I have a modest amount of files (226,441) from ec2 in subdirectories and ripgrep opens too many at once on Arch Linux.

```
rg asdfasdfasdf
...
./ec2-stuff/2016/09/28/03: Too many open files (os error 24)
./ec2-stuff//2016/09/13/07: Too many open files (os error 24)
./ec2-stuff///2016/09/28/13: Too many open files (os error 24)
./ec2-stuff//2016/09/07/18: Too many open files (os error 24)
./ec2-stuff//2016/09/28/07: Too many open files (os error 24)
./ec2-stuff//2016/09/07/15: Too many open files (os error 24)
./ec2-stuff//2016/09/07/06: Too many open files (os error 24)
...
```

```
➜  ec2 find . | wc -l
226441
➜  ~ rg --version
ripgrep 0.7.0
-AVX -SIMD
➜  ~ uname -a
Linux ergbox 4.13.8-1-ARCH #1 SMP PREEMPT Wed Oct 18 11:49:44 CEST 2017 x86_64 GNU/Linux
➜  ~ cat /proc/sys/fs/file-max
13166298
➜  ~ ulimit -Hn               
4096
➜  ~ ulimit -Sn               
1024
```

---

_Comment by @okdana on 2017-10-23 16:21_

This was fixed in 0.7.1 — see #648

ETA: The [Arch package](https://www.archlinux.org/packages/community/x86_64/ripgrep/) has already been flagged as out-of-date, so it's just a matter of waiting for them to update it now

---

_Comment by @erg on 2017-10-23 16:24_

Ah. Thanks!

---

_Closed by @erg on 2017-10-23 16:24_

---
