```yaml
number: 631
title: one thread can spend a long time chasing a dependency trail
type: issue
state: open
author: carljm
labels:
  - performance
assignees: []
created_at: 2025-06-11T01:20:52Z
updated_at: 2025-12-19T08:23:03Z
url: https://github.com/astral-sh/ty/issues/631
synced_at: 2026-01-10T01:53:59Z
```

# one thread can spend a long time chasing a dependency trail

---

_Issue opened by @carljm on 2025-06-11 01:20_

Observed running ty on large codebases: our current parallelism model splits work among threads at the granularity of "checking a file", where "a file" is a file the user has requested be checked.

In a case where there are many third-party dependencies, or many first-party files that were excluded from checking, the task of "check one file" can potentially encompass a very deep traversal of a large chain of dependency modules, causing one thread to become very busy for a long time while other threads sit idle.

Ideally, we would avoid this scenario. One possibility is that semantic indexing collects dependency modules and names depended on from those modules, and these also become part of a task list that we distribute to threads that would otherwise be idle.

---

_Label `performance` added by @carljm on 2025-06-11 01:20_

---

_Comment by @MichaReiser on 2025-06-11 05:23_

> Ideally, we would avoid this scenario. One possibility is that semantic indexing collects dependency modules and names depended on from those modules, and these also become part of a task list that we distribute to threads that would otherwise be idle.

We could explore this for first-party files because it's very likely that we need to analyze all of them. But doing this for third-party files is in direct conflict with our lazy approach of only doing as much work as is absolutely necessary. Parsing and indexing those dependencies might be entirely unnecessary if the first-party code doesn't use any of the transitively imported code. 

We also have the problem where many threads block on a single thread because they all have a shared dependency (or participate in one massive cycle). The problem here is that all threads are blocked and spawning of more work won't help, because all except one thread are blocked. This would require some form of yield support in salsa where blocked threads can yield to take on new work but I suspect that this could very easily result in dead-locks.

---

_Comment by @carljm on 2025-06-11 18:39_

> But doing this for third-party files is in direct conflict with our lazy approach of only doing as much work as is absolutely necessary. Parsing and indexing those dependencies might be entirely unnecessary if the first-party code doesn't use any of the transitively imported code.

Agreed, that's why I suggested collecting specific dependency modules and names actually used from those modules, when we are doing semantic indexing of the code we are going to check. I didn't suggest just blindly inferring types in all not-to-be-checked modules that exist on disk.

I guess this will still only give us one "dependency layer" of improved parallelism. Because we can't do this "collect from semantic indexing" on a not-to-be-checked file, without risking doing a bunch of work on unused dependencies. So we won't find transitive deps this way. I guess this one extra layer of parallelism is still better than the status quo -- could help a lot in some cases, less in others.

> We also have the problem where many threads block on a single thread because they all have a shared dependency (or participate in one massive cycle). The problem here is that all threads are blocked and spawning of more work won't help, because all except one thread are blocked. This would require some form of yield support in salsa where blocked threads can yield to take on new work but I suspect that this could very easily result in dead-locks.

Makes sense. I'm curious how often in practice we see the latter (all threads busy but blocked on another thread) vs the former (all threads but one idle, one thread doing a deep crawl through dependencies.)

---

_Comment by @MichaReiser on 2025-12-19 08:23_

Another thing I played with is spawning a task per scope in `check_types`. This helps a lot for projects like discord.py where a single file takes very long to type check as it enables more work-stealing. However this requires removing salsa caching from `check_file_impl` which seems bad for incrementality, because Salsa doesn't support multithreaded tasks in queries today, i

---
