```yaml
number: 17421
title: Add nextest partitioning to Windows tests
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: claude/nextest-windows-ci-R34x3
created_at: 2026-01-12T19:06:03Z
updated_at: 2026-01-13T15:22:10Z
url: https://github.com/astral-sh/uv/pull/17421
synced_at: 2026-01-13T15:29:37Z
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

I chose three shards due to a reasonable reduction in per-shard runtime without a big increase in total runtime (compared to two shards). I think since half of the shard runtime is fixed build time (vs reducable test time), going up to more shards would be wasteful.

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

_Marked ready for review by @zanieb on 2026-01-13 14:40_

---

_@zsol approved on 2026-01-13 14:56_

---

_Review comment by @konstin on `.github/workflows/test.yml`:143 on 2026-01-13 15:18_

Do we need to set our own cache key for windows?

---

_Merged by @zanieb on 2026-01-13 15:20_

---

_Closed by @zanieb on 2026-01-13 15:20_

---

_@konstin reviewed on 2026-01-13 15:20_

---

_@zanieb reviewed on 2026-01-13 15:22_

---

_Review comment by @zanieb on `.github/workflows/test.yml`:143 on 2026-01-13 15:22_

No, the host is included still. This just avoids the unique id from each shard being appended. (I verified this by looking at the generated cache key)

---
