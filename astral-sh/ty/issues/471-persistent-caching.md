```yaml
number: 471
title: Persistent caching
type: issue
state: open
author: serjflint
labels:
  - feature
assignees: []
created_at: 2025-05-21T13:55:00Z
updated_at: 2025-11-18T16:10:27Z
url: https://github.com/astral-sh/ty/issues/471
synced_at: 2026-01-12T15:54:23Z
```

# Persistent caching

---

_@serjflint_

### Question

Hello! Thank you for your great work and I am very excited about ty.

We have a large codebases and we have a declarative dependency graph. We run mypy for each node and store its .mypy_cache for later use in leaf nodes. Will it be possible to use file cache in `ty` between runs for even more performance?

### Version

0.0.1a3

---

_Label `question` added by @serjflint on 2025-05-21 13:55_

---

_Comment by @AlexWaygood on 2025-05-21 14:02_

We definitely hope to add a persistent cache at some point. Figuring out the details might be tricky, though

---

_Comment by @jelle-openai on 2025-05-21 16:37_

For what it's worth, I don't know how large "large" is for you, but on our codebase of several millions of lines, ty finishes quite quickly, so there's little point in a cache. Mypy's caching mechanism is tricky and the source of a lot of bugs, so if I were you I'd avoid building caching until there's a lot more signal that it would help users.

---

_Comment by @MichaReiser on 2025-05-21 16:41_

My main motivation for a persistent cache is to reduce peak memory usage (we don't need to parse all files or source texts) and having faster start up times in editors. But agree, it's less pressing than for mypy because ty can check even very large code bases in seconds.

---

_Comment by @serjflint on 2025-05-21 17:49_

> For what it's worth, I don't know how large "large" is for you, but on our codebase of several millions of lines, ty finishes quite quickly, so there's little point in a cache. Mypy's caching mechanism is tricky and the source of a lot of bugs, so if I were you I'd avoid building caching until there's a lot more signal that it would help users.

Our codebase is tens of millions LoC. The main profit actually is that our CI jobs don't need costly access to the whole codebase at once. Checking only the affected part is much cheaper.

---

_Comment by @MichaReiser on 2025-05-21 17:59_

That might difficult to implement because ty still needs to check if the files are unchanged.

However, you can run ty on a subproject and it will only read files that are reachable from the entry points 

---

_Comment by @serjflint on 2025-05-21 18:45_

> That might difficult to implement because ty still needs to check if the files are unchanged.
> 
> However, you can run ty on a subproject and it will only read files that are reachable from the entry points 

That check would defeat the purpose of preparing the caches. If ty would believe me that the files are unchanged that would.be helpful.

Running tools on the subproject is the default mode we had before switching to the optimized one. 

---

_Renamed from "Prepared file cache for large codebases" to "Persistent caching" by @MichaReiser on 2025-07-23 09:20_

---

_Label `question` removed by @MichaReiser on 2025-07-23 09:20_

---

_Label `feature` added by @MichaReiser on 2025-07-23 09:20_

---

_Assigned to @ibraheemdev by @MichaReiser on 2025-08-15 12:11_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:41_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
