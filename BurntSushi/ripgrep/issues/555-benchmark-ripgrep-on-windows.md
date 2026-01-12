```yaml
number: 555
title: benchmark ripgrep on Windows
type: issue
state: closed
author: latkin
labels:
  - enhancement
  - icebox
assignees: []
created_at: 2017-07-13T23:09:56Z
updated_at: 2018-01-31T02:42:51Z
url: https://github.com/BurntSushi/ripgrep/issues/555
synced_at: 2026-01-12T16:13:22Z
```

# benchmark ripgrep on Windows

---

_@latkin_

Per request from Twitter: https://twitter.com/burntsushi5/status/885481336261226496

My original statement was based on a few searches across my company's internal codebase, which I cannot share.  However I was able to repro same thing using public code, so I wrote up details & results here.

## Environment

```
// OS
OS Name:                   Microsoft Windows 10 Pro
OS Version:                10.0.15063 N/A Build 15063
System Manufacturer:       Apple Inc.
System Model:              MacBookPro11,5
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 70 Stepping 1 GenuineIntel ~2500 Mhz

// disk
Partitions : 6
DeviceID   : \\.\PHYSICALDRIVE0
Model      : APPLE SSD SM0512G
Size       : 500269754880
Caption    : APPLE SSD SM0512G

// ripgrep
ripgrep 0.5.2

// git
git version 2.9.2.windows.1
```

### Setup

Clone latest master from https://github.com/microsoft/visualfsharp (commit `b4c4d622135f8eb600cf032c6`)

All snippets below are run in powershell.

```
# 3693 *.fs files, about 20.6 MiB 
> dir *.fs -File -Recurse | Measure-Object -Sum Length

Count    : 3693
Sum      : 21691047
Property : Length
```

### Test 1

Basic search for `let` bindings in .fs files.

```
> rg '\blet ' -g *.fs | Measure-Object
Count    : 63566

> git grep '\blet ' *.fs | Measure-Object
Count    : 63564

> 1..10 |%{ Measure-Command { rg '\blet ' -g *.fs } } |% TotalMilliseconds | Measure-Object -max -min -average
Count    : 10
Average  : 262.38878
Maximum  : 312.5951
Minimum  : 249.32

> 1..10 |%{ Measure-Command { git grep '\blet ' *.fs } } |% TotalMilliseconds | Measure-Object -max -min -average
Count    : 10
Average  : 207.5198
Maximum  : 240.2744
Minimum  : 193.7944
```

### Test 2

Basic search for `async` blocks

```
> rg '\basync\s*\{' -g *.fs | Measure-Object
Count    : 301

> git grep '\basync\s*{' *.fs | Measure-Object
Count    : 301

> 1..10 |%{ Measure-Command { rg '\basync\s*\{' -g *.fs } } |% TotalMilliseconds | Measure-Object -max -min -average
Count    : 10
Average  : 156.96702
Maximum  : 171.3596
Minimum  : 151.0885

> 1..10 |%{ Measure-Command { git grep '\basync\s*{' *.fs } } |% TotalMilliseconds | Measure-Object -max -min -average
Count    : 10
Average  : 92.89577
Maximum  : 109.9558
Minimum  : 87.6595
```

---

_Comment by @ghuls on 2017-07-28 08:31_

On Ubuntu 16.04 (ext4) rg is faster:
```
$ for x in 0 1 2 3 4 5 ; do time rg '\blet ' -g *.fs > /
dev/null; done

real    0m0.068s
user    0m0.224s
sys     0m0.064s

real    0m0.045s
user    0m0.144s
sys     0m0.048s

real    0m0.035s
user    0m0.100s
sys     0m0.068s

real    0m0.044s
user    0m0.140s
sys     0m0.068s

real    0m0.038s
user    0m0.116s
sys     0m0.064s

real    0m0.036s
user    0m0.128s
sys     0m0.052s

$ for x in 0 1 2 3 4 5 ; do time git grep '\blet ' *.fs > /dev/null; done

real    0m0.109s
user    0m0.476s
sys     0m0.024s

real    0m0.080s
user    0m0.368s
sys     0m0.036s

real    0m0.080s
user    0m0.380s
sys     0m0.024s

real    0m0.084s
user    0m0.360s
sys     0m0.028s

real    0m0.085s
user    0m0.380s
sys     0m0.020s

real    0m0.092s
user    0m0.344s
sys     0m0.064s

$ for x in 0 1 2 3 4 5 ; do time rg '\basync\s*\{' -g *.
fs > /dev/null; done

real    0m0.036s
user    0m0.108s
sys     0m0.056s

real    0m0.032s
user    0m0.112s
sys     0m0.044s

real    0m0.036s
user    0m0.136s
sys     0m0.032s

real    0m0.032s
user    0m0.088s
sys     0m0.076s

real    0m0.043s
user    0m0.184s
sys     0m0.048s

real    0m0.026s
user    0m0.076s
sys     0m0.052s

$ for x in 0 1 2 3 4 5 ; do time git grep '\basync\s*{' *.fs > /dev/null; done

real    0m0.108s
user    0m0.404s
sys     0m0.032s

real    0m0.081s
user    0m0.348s
sys     0m0.036s

real    0m0.070s
user    0m0.324s
sys     0m0.048s

real    0m0.071s
user    0m0.340s
sys     0m0.016s

real    0m0.077s
user    0m0.328s
sys     0m0.028s

real    0m0.072s
user    0m0.324s
sys     0m0.056s

```

Probably it is due the relative slowness of NTFS on Windows.

> http://www.phoronix.com/scan.php?page=article&item=windows-10-lxcore&num=4
> Lists some benchmarks across Ubuntu and this subsystem. It is very competitive with Native Ubuntu Linux, except when it comes to filesystem access.
> 
> Filesystem access is important for compilation, as well as the thousands of "watcher" js and ruby programs that constantly touch the filesystem, which are the stated target market.
https://wpdev.uservoice.com/forums/266908-command-prompt-console-bash-on-ubuntu-on-windo/suggestions/13386252-filesystem-slow-compared-with-native-ubuntu-system

It might not be the most honest comparison as this tests filesystem speed in **Bash on Ubuntu on Windows** instead of plain Windows.

> Enable the filesystem cache
> 
> Windows' filesystem layer is inherently different from Linux' (for which Git's filesystem access is optimized). As a workaround, Git for Windows offers a filesystem cache which accelerates operations in many cases, after an initial "warm-up". You can activate the filesystem cache per-repository:
> 
> git config core.fscache true

https://github.com/msysgit/msysgit/wiki/Diagnosing-why-Git-is-so-slow

Can you check if this git setting is enabled?
```
git config --get core.fscache
```

---

_Comment by @BurntSushi on 2017-07-28 10:41_

I still haven't had a chance to look into this, but I figured I'd respond with some things.

First and foremost, the search times here appear to be in the lowish milliseconds, which means that noise could be a serious contributing factor. If one tool takes 250ms to do something and another takes 200ms to do the same thing, then they're pretty darn close and I wouldn't really be too concerned about that.

Secondly, I've never done any benchmarks on Windows, and there are definitely things inside ripgrep that are specifically slower than on Unix systems, mostly having to do with the fact that paths in Windows are UTF-16 and all the glob matching logic requires UTF-8.

Thirdly, @ghuls idea of looking to the file system may be a good idea because `git grep` can specifically avoid walking the directory structure because of the git index. But that's totally fair; that's an advantage that `git grep` has.

I think the appropriate action item here is that someone needs to do a comprehensive benchmark analysis of ripgrep on Windows, probably compared to other tools, and figure out which things are slow. I suspect the answers will be different than on Unix.

---

_Renamed from "ripgrep consistently slower than git grep" to "benchmark ripgrep on Windows" by @BurntSushi on 2017-10-22 01:12_

---

_Label `icebox` added by @BurntSushi on 2017-10-22 01:12_

---

_Label `enhancement` added by @BurntSushi on 2017-10-22 01:13_

---

_Comment by @BurntSushi on 2017-10-22 01:14_

I'd like to change the focus of this ticket from "ripgrep is slower than git grep" to "do comprehensive benchmarks of ripgrep on Windows." I don't know if I'll ever do that myself, but I think it is a worthwhile task.

Also, the very first thing someone should do is find a meatier repository. `rg '\blet '` takes less than 50 milliseconds on my system against the `visualfsharp` repo.

---

_Comment by @BurntSushi on 2018-01-31 02:42_

I'm going to close this because I don't think there's a lot of value in tracking it. I'd still like for someone to do benchmarks with a similar rigor level that can be found in my [blog post introducing ripgrep](https://blog.burntsushi.net/ripgrep/) though!

---

_Closed by @BurntSushi on 2018-01-31 02:42_

---
