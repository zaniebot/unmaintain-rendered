```yaml
number: 8153
title: Try out fopro?
type: pull_request
state: closed
author: fasterthanlime
labels: []
assignees: []
draft: true
base: main
head: fopro
created_at: 2024-10-12T21:24:06Z
updated_at: 2024-12-04T01:39:01Z
url: https://github.com/astral-sh/uv/pull/8153
synced_at: 2026-01-12T16:08:11Z
```

# Try out fopro?

---

_@fasterthanlime_

## Summary

This tries out https://github.com/bearcove/fopro when running cargo tests on linux

---

_Comment by @zanieb on 2024-10-13 19:30_

fyi I [reran the latest ubuntu test job](https://github.com/astral-sh/uv/actions/runs/11316499590/job/31470100088?pr=8153) to see what happened when there was a cache hit — looks like about a minute faster! It failed though haha :)

---

_Comment by @fasterthanlime on 2024-10-13 20:18_

> looks like about a minute faster

Unfortunately, it's not! I opened #8160 to do a fair comparison and at least on Linux with the current setup, it's not making a different in CI. (I see a difference locally but... I'd like more than that!)

---

_Comment by @zanieb on 2024-10-13 21:25_

Interesting. A few things come to mind: 

1. Linux is already the fastest, are there benefits on other platforms?
2. Is there more benefit if we reduce the runner size? We increased the runner size several times to get a quick speed improvement, but that compute isn't free!
3. Is it faster for most tests but some slow tests that aren't network bound are the bottleneck?

---

_Comment by @fasterthanlime on 2024-10-13 22:22_

> 3\. Is it faster for most tests but some slow tests that aren't network bound are the bottleneck?

Probably! Selecting `lock::` tests and forcing `-j1` shows that, y'know, fopro is not doing _nothing_:

![with fopro it runs in 77s, without it run in 89s](https://github.com/user-attachments/assets/d662aa37-20e3-4362-830a-f075d3c0873e)

...and there's still some things it doesn't cache, like git clones, HEAD requests, byte range requests, etc. — which aren't _that_ hard to add, tbh.

---

_Closed by @charliermarsh on 2024-12-04 01:39_

---
