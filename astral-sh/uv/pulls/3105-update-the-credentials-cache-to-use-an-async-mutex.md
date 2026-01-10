```yaml
number: 3105
title: Update the credentials cache to use an async mutex
type: pull_request
state: closed
author: zanieb
labels:
  - performance
  - internal
  - do-not-merge
assignees: []
base: main
head: zb/auth-async
created_at: 2024-04-17T19:12:27Z
updated_at: 2024-04-19T21:04:53Z
url: https://github.com/astral-sh/uv/pull/3105
synced_at: 2026-01-10T14:43:31Z
```

# Update the credentials cache to use an async mutex

---

_Pull request opened by @zanieb on 2024-04-17 19:12_

It seems wrong to use a sync mutex in an async context without strong justification. We're probably unnecessarily blocking the event loop.

---

_Label `performance` added by @zanieb on 2024-04-17 19:12_

---

_Label `internal` added by @zanieb on 2024-04-17 19:12_

---

_Comment by @charliermarsh on 2024-04-17 19:35_

I think this actually isn't recommended unless you need to hold the mutex across await points: https://docs.rs/tokio/latest/tokio/sync/struct.Mutex.html#which-kind-of-mutex-should-you-use

---

_Comment by @charliermarsh on 2024-04-17 19:38_

(I was surprised to learn this.)

---

_Comment by @zanieb on 2024-04-17 19:42_

Oh interesting... that... kind of makes sense but idk? It's hard to grok when you have a bunch of async requests going through the middleware at the same time. It's fine for one of the requests to block the entire async thread waiting for a mutex? Weird.

---

_Label `do-not-merge` added by @zanieb on 2024-04-17 21:31_

---

_Comment by @zanieb on 2024-04-18 04:08_

@BurntSushi do you have more knowledge here?

---

_Comment by @BurntSushi on 2024-04-18 13:01_

Not particularly. But the advice from tokio's docs makes sense. Maybe another way to conceptualize this is that I believe it's _always_ correct to use an async mutex, but it's _not_ always correct to use a std mutex. Maybe think of a std mutex as an optimization. You just really want to avoid holding it over an await boundary. And indeed, it could wind up blocking the whole thread if it's highly contended. But at that point, I'd classify it as a perf bug, async or not.

I also think the last bit of the tokio docs is crucially important:

> Additionally, when you do want shared access to an IO resource, it is often better to spawn a task to manage the IO resource, and to use message passing to communicate with that task.

In other words, I'd treat the use of an async mutex as somewhat niche. I'd try to take a more CSP (Communicating Sequential Processes) approach to the problem.

---

_Comment by @BurntSushi on 2024-04-18 13:03_

For this PR, it looks like the use of a std mutex is okay? The tip-off is that it looks like your routines aren't doing any I/O and weren't previously async (because you needed to add `async fn` in order to use an async mutex).

---

_Closed by @zanieb on 2024-04-19 21:04_

---
