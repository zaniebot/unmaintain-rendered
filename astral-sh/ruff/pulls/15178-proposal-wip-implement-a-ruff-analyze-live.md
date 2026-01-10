```yaml
number: 15178
title: "[PROPOSAL+WIP] implement a `ruff analyze live`"
type: pull_request
state: closed
author: purajit
labels:
  - needs-decision
assignees: []
base: main
head: 20241227-analyze-live
created_at: 2024-12-29T02:59:47Z
updated_at: 2025-03-11T08:50:33Z
url: https://github.com/astral-sh/ruff/pull/15178
synced_at: 2026-01-10T19:49:01Z
```

# [PROPOSAL+WIP] implement a `ruff analyze live`

---

_Pull request opened by @purajit on 2024-12-29 02:59_

MVP implementation of https://github.com/astral-sh/ruff/issues/15198.

This is definitely not close to the final state of the PR (docs, tests, potential optimizations, Rust idiomaticity,
code refactoring at the very least);  I just wanted to see I could get a reasonable implementation of this working. 
I've been using this on our largest repo to test, and it's working really well. Feature-wise, I'd probably want to 
add more configuration  options (what to monitor, what to ignore), and also allow for xargs's replstr format to 
specify where in the command the file names should be injected (currently just doing it at the end, which should 
work in most cases, even with named args).

---

_Comment by @github-actions[bot] on 2024-12-29 03:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `needs-decision` added by @MichaReiser on 2024-12-30 08:25_

---

_Comment by @MichaReiser on 2024-12-30 08:37_

Thanks for opening this PR and suggesting this idea. It's an interesting use case, but it's not fully clear to me how it fits into `ruff analyze` and ruff overall and if there are ways we can avoid unnecessary code duplication (Ruff now has multiple entry points that support watch). 

Unfortunately, I don't have the time right now to think this through but maybe someone else on the team is excited about it and has time to help design a feature that solves the same needs and fits nicely into Ruff. 

---

_Comment by @purajit on 2024-12-30 09:00_

Thanks for the response!

I guess the difference is whether a tool like this should be built on _top_ of ruff or directly powered 
by ruff. I leaned towards the latter because it is directly based on generating the dependency graph 
repeatedly and keeping the view updated incrementally in-memory; keeping it in `analyze` would 
also allow this feature to be powered by anything else that gets added to analyze, and other ruff
internals. Of course, the same could be accomplished by a tool that uses ruff and reads the graph
output, but this is far more powerful, and I think it's a powerful canonical feature to have - honestly
one of the biggest benefits of having a dependency graph.

Another argument is that Pants currently has a [`--loop`](https://www.pantsbuild.org/dev/docs/using-pants/key-concepts/goals#running-goals) which this would be able to replace for
people coming from that world.

I do understand that this pushes the scope of ruff, I definitely expected some hesitation and discussion!

---

_Comment by @purajit on 2025-01-05 21:21_

As an FYI I have a stand-alone implementation in https://github.com/purajit/ruff-tools.

---

_Comment by @MichaReiser on 2025-03-11 08:50_

I'll close this for now. I like the idea and I think a feature like this could fit into Ruff but we may want to tackle watch support more holisticly in Ruff before adding more command specific implementations.

---

_Closed by @MichaReiser on 2025-03-11 08:50_

---
