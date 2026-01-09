---
number: 1668
title: v2-master branch is broken on 1.33
type: issue
state: closed
author: CreepySkeleton
labels: []
assignees: []
created_at: 2020-02-04T14:33:59Z
updated_at: 2020-02-05T10:34:34Z
url: https://github.com/clap-rs/clap/issues/1668
synced_at: 2026-01-07T13:12:19-06:00
---

# v2-master branch is broken on 1.33

---

_Issue opened by @CreepySkeleton on 2020-02-04 14:33_

MSRV for 2.x is 1.33, but current `v2-master` branch doesn't build with 1.33. Does it worth fixing it?

---

_Label `T: RFC / question` added by @CreepySkeleton on 2020-02-04 14:34_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-04 14:34_

---

_Comment by @Dylan-DPC-zz on 2020-02-04 14:59_

It isnt needed. Even if for some reason we need to push to 2.*, We can always start a new branch from the release 

---

_Comment by @CreepySkeleton on 2020-02-04 15:19_

I would actually like to backport at least simple bugfixes, [like that one](https://github.com/clap-rs/clap/pull/1481)

---

_Comment by @pksunkara on 2020-02-04 15:21_

I will look into the v2-master branch and fix it today so that we can allow PRs to  it.

---

_Comment by @tshepang on 2020-02-04 15:29_

Yeah, I imagine many will choose to stay on v2.x for a while yet, so may need to be kept alive for some time. I hope that's ok to say, given I'm not volunteering.

---

_Comment by @Dylan-DPC-zz on 2020-02-04 16:00_

It is fine to say. But normally when a new version is released, it implicitly implies that previous version is no longer supported. 

---

_Comment by @CreepySkeleton on 2020-02-05 10:33_

I suspect it will be a while until 3.0 is released so we need to keep supporting 2.x until then at least  

---

_Comment by @pksunkara on 2020-02-05 10:34_

So, I added the CIs and everything to v2-master which should make it easy for us to maintain it. The minimum version is now 1.36. I am closing this.

---

_Closed by @pksunkara on 2020-02-05 10:34_

---
