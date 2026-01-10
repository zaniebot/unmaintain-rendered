---
number: 3311
title: "Feature request: Control parallelism of downloads, sdist builds, and installs"
type: issue
state: closed
author: notatallshaw
labels:
  - configuration
assignees: []
created_at: 2024-04-29T14:36:58Z
updated_at: 2024-05-18T03:14:21Z
url: https://github.com/astral-sh/uv/issues/3311
synced_at: 2026-01-10T01:23:26Z
---

# Feature request: Control parallelism of downloads, sdist builds, and installs

---

_Issue opened by @notatallshaw on 2024-04-29 14:36_

Not significantly high priority but I would like to be able to control how many parallel downloads, sdist builds, and installs uv runs, three main reasons:

1. I've had a couple of recent errors in WSL2 when running uv that the device is not ready yet (I will capture it if I see it again and make a separate issue) and it appeared to be from uv running too much against the filesystem at once
2. I may wish uv to use less resources and don't mind if it takes longer to run, e.g. on a low power device with other things running and building the Python environment isn't latency sensitive
3. I would like to be able to do more specific benchmarks compared to pip, and it's difficult to measure in a full install where uv is actually more efficient, or where it is doing things in parallel

There is an open PR (https://github.com/pypa/pip/pull/12388) on pip side that will expose the number of downloads at once via "--parallel_downloads" 


---

_Comment by @zanieb on 2024-04-29 14:39_

I think we should support this. I'd prefer environment variables to command line flags though.

---

_Label `configuration` added by @zanieb on 2024-04-29 14:39_

---

_Comment by @ibraheemdev on 2024-05-02 19:19_

Does `--parallel-downloads` set the number of threads used to download in parallel, or the maximum number of inflight concurrent downloads? We run most downloads on the main thread so we could probably only expose the latter.

---

_Comment by @notatallshaw on 2024-05-02 20:52_

> Does `--parallel-downloads` set the number of threads used to download in parallel, or the maximum number of inflight concurrent downloads? We run most downloads on the main thread so we could probably only expose the latter.

The pip PR adds the feature to use multiple threads, and the feature controls the number of threads.

But for my use case at least I was wanting to set one download at a time, I wasn't too concerned with whether things are technically parallel or concurrent.

---

_Referenced in [astral-sh/uv#3493](../../astral-sh/uv/pulls/3493.md) on 2024-05-09 18:37_

---

_Comment by @ibraheemdev on 2024-05-10 16:24_

https://github.com/astral-sh/uv/pull/3493 gives you a way to manage network and build concurrency limits. We might still want a way to limit installs, and limit the size of the blocking threadpool if you want to limit fs operations. Maybe those could be combined.

---

_Comment by @notatallshaw on 2024-05-10 16:34_

Oh wow, looks amazing. I'm quite sure this covers my use cases.

---

_Comment by @ibraheemdev on 2024-05-10 16:38_

It doesn't completely fulfill your 3rd point of benchmarking against pip. However, serializing all uv operations would be quite difficult if not impossible and I don't think something worth doing. I think the best we can do is expose concurrency limits on specific operations.

---

_Comment by @notatallshaw on 2024-05-10 16:47_

That makes sense, however, installing time is easy to seperate out from the other numbers when benchmarking, and if needs be I can always test installing one package at a time with no dependencies.

---

_Comment by @ibraheemdev on 2024-05-10 16:55_

I was more referring to other fs operations, like reading from the cache, that could still occur concurrently during downloads. However, during a cold download you should be fine. I'll look into adding an install concurrency limit in a separate PR.

---

_Comment by @ibraheemdev on 2024-05-18 03:14_

Resolved by https://github.com/astral-sh/uv/pull/3493 and https://github.com/astral-sh/uv/pull/3646.

---

_Closed by @ibraheemdev on 2024-05-18 03:14_

---
