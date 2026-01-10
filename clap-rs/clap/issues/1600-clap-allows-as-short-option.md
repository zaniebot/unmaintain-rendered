---
number: 1600
title: "clap allows '-' as short option"
type: issue
state: closed
author: txk2048
labels:
  - E-help-wanted
  - E-easy
assignees: []
created_at: 2019-11-22T21:09:15Z
updated_at: 2020-02-03T03:12:22Z
url: https://github.com/clap-rs/clap/issues/1600
synced_at: 2026-01-10T01:26:59Z
---

# clap allows '-' as short option

---

_Issue opened by @txk2048 on 2019-11-22 21:09_

## Affected version
master branch

## Issue
.short() option method has problem where it allows you to pass the single character hyphen '-' to it

---

_Comment by @txk2048 on 2019-11-22 21:14_

I would be more than happy to submit a pull request to fix this issue if you could describe what sort of behaviour clap should display when faced with this situation

---

_Comment by @Dylan-DPC-zz on 2020-01-03 06:37_

Can you provide more details about it? thanks

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-02-01 11:18_

---

_Label `not-sure` added by @CreepySkeleton on 2020-02-01 14:29_

---

_Comment by @pksunkara on 2020-02-01 21:01_

Basically, I think the issue is that clap allows something like the following:

```rust
Arg::with_name("config")
    .short('-')
```

This should give an error.

---

_Comment by @CreepySkeleton on 2020-02-02 02:34_

Yes ,it should panic in this case

---

_Label `not-sure` removed by @CreepySkeleton on 2020-02-02 02:35_

---

_Label `C: errors` added by @CreepySkeleton on 2020-02-02 02:35_

---

_Label `C: options` added by @CreepySkeleton on 2020-02-02 02:35_

---

_Label `D: easy` added by @CreepySkeleton on 2020-02-02 02:35_

---

_Label `good first issue` added by @CreepySkeleton on 2020-02-02 02:35_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-02 02:35_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-02 02:35_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-02 02:35_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-02 02:35_

---

_Comment by @txk2048 on 2020-02-02 11:24_

I'll put in a pull request shortly to fix this

---

_Referenced in [clap-rs/clap#1657](../../clap-rs/clap/pulls/1657.md) on 2020-02-02 11:50_

---

_Comment by @txk2048 on 2020-02-02 11:51_

By the way while fixing that I noticed an issue with the documentation for that method, should I open a new issue for that?

---

_Comment by @CreepySkeleton on 2020-02-02 11:53_

If you want to fix it - just add an extra commit in this PR. Otherwise, open a separate issue

---

_Comment by @CreepySkeleton on 2020-02-03 03:12_

Closed by #1657 

---

_Closed by @CreepySkeleton on 2020-02-03 03:12_

---
