```yaml
number: 17421
title: Add nextest partitioning to Windows tests
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: claude/nextest-windows-ci-R34x3
created_at: 2026-01-12T19:06:03Z
updated_at: 2026-01-13T05:17:50Z
url: https://github.com/astral-sh/uv/pull/17421
synced_at: 2026-01-13T05:29:28Z
```

# Add nextest partitioning to Windows tests

---

_@zanieb_

Yet another attempt at #5714 to see if it improves CI times now that Windows test times have increased

I think this is worth it now

```
main   (1 shard):  13m 46s
branch (2 shards): 10m 31s
branch (3 shards):  7m 34s
```

total time (a.k.a., cost increase)

```
main   (1 shard):  13m 46s
branch (2 shards): 20m 18s
branch (3 shards): 21m 32s
```

As in #875, we could explore moving the build step before the sharded test runs. The build is 3m of the runtime, but I think artifact transfer and runner startup time was too expensive last time and I don't expect it to be faster. It might save a bit on CI cost, but I'm not super worried about that.

We use a hash-based strategy for test splitting, which means adding tests will never move tests across shards.

I moved _off_ of the Depot runner because the "checkout" setup is taking _way_ too long, about 120s more than the GitHub one which, once we split over multiple shards, overcomes the speed benefits of the Depot runners.

The downsides here are:

1. Increased total CI time and therefore increased cost
2. GitHub runners are more expensive than Depot runners
3. Failing tests can be split across multiple shards, requiring more GitHub UI navigation

(1) The runtime is unacceptably slow and I think the cost increase is fairly marginal
(2) We can move back to Depot runners once they resolve the specific performance issue
(3) Failing tests on Windows without failing tests on Linux are fairly rare and should often be isolated to a single shard


---

_Label `internal` added by @zanieb on 2026-01-12 19:06_

---
