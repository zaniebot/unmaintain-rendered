```yaml
number: 450
title: "walk.rs: queue -> stack"
type: pull_request
state: closed
author: bmalehorn
labels: []
assignees: []
base: master
head: stack
created_at: 2017-04-16T19:35:37Z
updated_at: 2017-08-23T21:51:29Z
url: https://github.com/BurntSushi/ripgrep/pull/450
synced_at: 2026-01-12T18:23:13Z
```

# walk.rs: queue -> stack

---

_@bmalehorn_

Change the pool of "files to search" from queue to a stack. This causes
ripgrep to approximate depth-first search instead of breadth-first
search. This dramatically reduces the size of the pool, since most
directories are much more "wide" than they are "deep".

As a result, ripgrep uses ~45% less peak memory:

Before:

    /usr/bin/time -f '%M' rg PM_RESUME > /dev/null
    ...
    8876

After:

    /usr/bin/time -f '%M' rg PM_RESUME > /dev/null
    16240

Throughput improves a tiny bit:

Before:

    linux_literal (pattern: PM_RESUME)
    ----------------------------------
    rg (ignore)*  0.376 +/- 0.003 (lines: 16)*

After:

    linux_literal (pattern: PM_RESUME)
    ----------------------------------
    rg (ignore)*  0.371 +/- 0.004 (lines: 16)*

Related to #152.

---

_Comment by @BurntSushi on 2017-05-08 23:41_

OK, so I finally had a chance to manually QA this. If I run `rg PM_SUSPEND` in a checkout of the Linux kernel, then the results appear much more consistent from run-to-run in current master than they do with this PR. Is this something you expected to happen?

---

_Comment by @bmalehorn on 2017-05-09 05:44_

Great, thanks for taking a look!

I've tested it on my end and can also confirm that master is more consistent. I don't really have a great explanation; it's rather tricky to diff two types of undefined behaviors.

I guess this PR is a bit open-ended: how do you defined consistency? Master is more likely to output in the same order from one run to another. But "stack" will usually group directories together. If you only run ripgrep once, which output is better?

Master:

    include/linux/ide.h
    Documentation/dev-tools/sparse.rst
    drivers/ide/ide-pm.c
    include/uapi/linux/apm_bios.h
    arch/x86/kernel/apm_32.c
    Documentation/translations/zh_CN/sparse.txt
    drivers/input/mouse/cyapa.h
    drivers/usb/mtu3/mtu3_hw_regs.h
    drivers/mtd/maps/pcmciamtd.c
    drivers/net/wireless/intersil/hostap/hostap_cs.c

Stack:

    drivers/mtd/maps/pcmciamtd.c
    drivers/ide/ide-pm.c
    drivers/net/wireless/intersil/hostap/hostap_cs.c
    drivers/usb/mtu3/mtu3_hw_regs.h
    drivers/input/mouse/cyapa.h
    Documentation/translations/zh_CN/sparse.txt
    Documentation/dev-tools/sparse.rst
    arch/x86/kernel/apm_32.c
    include/uapi/linux/apm_bios.h
    include/linux/ide.h


---

_Comment by @BurntSushi on 2017-05-09 11:16_

Right, if you qualify it with "which is better if you only run ripgrep once," then I agree that this PR is better. But I'm not sure that I buy that qualification...

---

_Comment by @hlieberman on 2017-06-12 20:12_

I did a little bit of a back-of-the-envelope test here on how different these two actually are.

I ran 1,000 greps of PM_SUSPEND of Linux through current HEAD (45e850aff769a9d3f1fc6457bd5e1390021f433c) and through the PR branch here (d4fa0155e7e2fb68de133c0cd36f0be2f92e782f), both with SIMD/AVX optimizations, on a machine with 4 CPU cores.

HEAD showed very little overlap; of the 1000 entries, there are 998 unique md5sums. The PR branch shows no overlap at all; all 1000 entries are unique. As you might expect, there is also no overlap between the two sets.

To get a rough measure of "compactness", I looked at the number of times that top level directories were listed not in a row.  (that is: "a/foo, a/bar, b/foo/, a/bar" would have a score of 3)

Master:
```
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  13.00   13.00   15.00   15.66   16.25   28.00
```
![master](https://user-images.githubusercontent.com/711517/27053006-c47e2620-4f89-11e7-8076-095aa875032e.png)

PR:
```
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   7.00    9.00   11.00   14.21   21.00   35.00 
```
![stack](https://user-images.githubusercontent.com/711517/27053008-cb75b4b6-4f89-11e7-8b56-f90b5b893c92.png)

So, the PR seems to produce more concise results on average, though the spread is a little worse.

Food for thought!

---

_Comment by @BurntSushi on 2017-08-23 21:51_

@hlieberman Thanks for that analysis! I think for now I'm going to close this PR. It's a neat idea, and we might want to revisit it, but I don't feel that there's compelling enough evidence to switch at this point.

---

_Closed by @BurntSushi on 2017-08-23 21:51_

---
