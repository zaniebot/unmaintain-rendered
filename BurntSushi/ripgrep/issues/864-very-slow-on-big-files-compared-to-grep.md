```yaml
number: 864
title: very slow on big files compared to grep
type: issue
state: closed
author: arpi79
labels: []
assignees: []
created_at: 2018-03-20T08:08:33Z
updated_at: 2018-03-21T11:48:22Z
url: https://github.com/BurntSushi/ripgrep/issues/864
synced_at: 2026-01-12T16:13:22Z
```

# very slow on big files compared to grep

---

_@arpi79_

#### What version of ripgrep are you using?

$ rg --version
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX


#### What operating system are you using ripgrep on?

CYGWIN_NT-6.1 spdm1247 2.10.0(0.325/5/3) 2018-02-02 15:16 x86_64 Cygwin

#### Describe your question, feature request, or bug.

rg is 4x times slower then grep for a similar search.
I have a 11GB log file and just want to search for a simple text and count occurrences.

$ time rg -j 4 -a 'ScheduledTopUp.TimeStamp=1518' server.log.2018-02-14 -c
37930

real    0m46.510s
user    0m0.000s
sys     0m0.000s

$ time grep 'ScheduledTopUp.TimeStamp=1518' server.log.2018-02-14 -c
37930

real    0m13.145s
user    0m9.282s
sys     0m3.806s

Try with other settings too, but no improvements:

$ time rg -j 4 'ScheduledTopUp.TimeStamp=1518' server.log.2018-02-14 --mmap -c 
37930

real    2m21.926s
user    0m0.156s
sys     0m0.452s

$ time rg 'ScheduledTopUp.TimeStamp=1518' server.log.2018-02-14 -c 
37930

real    3m10.727s
user    0m0.234s
sys     0m0.405s






---

_Comment by @BurntSushi on 2018-03-20 09:38_

This isn't enough information to act on. Fixing performance bugs requires that it can be reproduced. Please find a way to reproduce the problem on an open dataset (or find a way to get me your dataset).

Also, I see that you are on Windows. Almost all of my benchmarking has been done on Linux. What very little benchmarking I've done in Windows suggests that performance can be greatly impacted by active virus scanners.

The high variability between your runs is also quite suspicious. Are you sure you aren't just measuring disk bandwidth? 

---

_Comment by @arpi79 on 2018-03-21 11:25_

Hi,

I will try to find a such  data set.
As you can see there were different parameters given to the rg.
If the same parameters/flags are given the same result is received.
"-j 4 -a" gives best result : ~ 50 sec
no flag the worst result: ~ 3 min

Btw. I installed rg on a debian machine:
4.14.0-2-amd64 #1 SMP Debian 4.14.7-1 (2017-12-22) x86_64 GNU/Linux

Now the times are comparable:

$ time grep 'ScheduledTopUp.TimeStamp=1518' server.log.2018-02-14 -c
37930

real    2m20.756s
user    0m19.277s
sys     0m7.317s

$ time rg -j 4 -a 'ScheduledTopUp.TimeStamp=1518' server.log.2018-02-14 -c
37930

real    2m30.704s
user    0m8.266s
sys     0m6.611s

So issue seems to be windows related.
Thank you for your time.

---

_Closed by @arpi79 on 2018-03-21 11:25_

---

_Comment by @BurntSushi on 2018-03-21 11:48_

> "-j 4 -a" gives best result : ~ 50 sec
> no flag the worst result: ~ 3 min

Something is very clearly amiss. You're searching a single file. ripgrep doesn't benefit from parallelism when searching a single file, so the fact that it's faster suggests something is going wrong. One possible explanation is that ripgrep is actually searching your entire CWD, even though that would definitely be a bug given the command you're running.

---
