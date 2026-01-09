---
number: 302
title: "`uv-git` should be async"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2023-11-02T23:49:51Z
updated_at: 2024-07-01T21:32:16Z
url: https://github.com/astral-sh/uv/issues/302
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv-git` should be async

---

_Issue opened by @charliermarsh on 2023-11-02 23:49_

It's hard because the `git2` structs aren't `Send`....

---

_Label `internal` added by @charliermarsh on 2023-11-03 00:15_

---

_Comment by @BurntSushi on 2023-11-07 18:06_

Wild shot from the hip here: are we sure the project itself should be using async at all? I realize it needs to interact with the network, but do we know that we're benefiting from async here?

(This may also be an ecosystem dilemma. Where the best networking libraries force you into async, but the integration points of other libraries that aren't traditional async, like `git2`, becomes trickier. One possible way to address that would be to push async to the boundaries and treat them as synchronous calls via something like [`block_on`](https://docs.rs/tokio/latest/tokio/runtime/struct.Runtime.html#method.block_on).)

---

_Comment by @charliermarsh on 2023-11-07 18:12_

We definitely need to be able to fetch metadata concurrently while we solve (i.e., when we pick a package version, kick off fetches for its dependencies while we move on to the next decision). Async seemed like the best way to enable that data flow, but I'm also relatively inexperienced with the options available to us.

---

_Comment by @charliermarsh on 2023-11-07 18:13_

I based some of the async use on decisions I saw in https://github.com/orogene/orogene.

---

_Comment by @BurntSushi on 2023-11-07 18:17_

Yeah one should definitely be able to achieve that with multi-threading. It is true that async also enables it, but arguably at a higher friction cost. The multi-threading approach can have other downsides, particularly if you wind up needing to spawn lots of threads. But "lots of threads" probably only becomes a problem at somewhat large numbers.

Anyway, it's really hard for me to have a coherent opinion on architecture given what I know (i.e., very little). I'll keep noodling on this as I continue to explore Puffin.

---

_Comment by @charliermarsh on 2023-11-07 18:18_

Please do. We're definitely still in a position where we could remove async entirely.

---

_Comment by @charliermarsh on 2023-11-07 18:18_

(I don't hold a strong opinion that we _have_ to use async, it's more that it seemed like the right fit and I haven't explored alternatives.)

---

_Comment by @zanieb on 2023-11-07 18:56_

> Wild shot from the hip here: are we sure the project itself should be using async at all?

I've raised this too and agree with the general sentiment.

>  But "lots of threads" probably only becomes a problem at somewhat large numbers.

Yeah. I'm unsure we're going to reach that scale doing package resolution.

Maybe we could estimate the number of concurrent tasks we need when solving a large project?

---

_Renamed from "`puffin-git` should be async" to "`uv-git` should be async" by @zanieb on 2024-02-15 19:51_

---

_Comment by @zanieb on 2024-07-01 21:28_

@ibraheemdev should we still do this?

---

_Comment by @zanieb on 2024-07-01 21:29_

(Presuming this is not relevant anymore)

---

_Closed by @zanieb on 2024-07-01 21:29_

---

_Comment by @ibraheemdev on 2024-07-01 21:32_

Our git code is now all process based. Use tokio processes internally isn't clearly better than a single outer `spawn_blocking` call, so I think it's fine how it is.

---
