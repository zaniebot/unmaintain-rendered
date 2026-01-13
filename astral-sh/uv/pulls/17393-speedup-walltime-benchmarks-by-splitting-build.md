```yaml
number: 17393
title: Speedup walltime benchmarks by splitting build and run jobs
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: claude/port-ruff-22126-Gz4vw
created_at: 2026-01-09T21:34:27Z
updated_at: 2026-01-13T15:49:07Z
url: https://github.com/astral-sh/uv/pull/17393
synced_at: 2026-01-13T16:27:49Z
```

# Speedup walltime benchmarks by splitting build and run jobs

---

_@zanieb_

Copies astral-sh/ruff#22126

We can use a larger _and_ less expensive runner for the build step.

Total runtime went from 18m -> 15m and most of that time is no longer on the benchmark runner.

---

_Marked ready for review by @zanieb on 2026-01-12 20:53_

---

_Label `internal` added by @zanieb on 2026-01-12 20:55_

---

_Review comment by @zsol on `.github/workflows/bench.yml`:80 on 2026-01-13 15:24_

instead of this, I'd tar everything up, upload _that_ as an artifact, and untar here. Bonus points for preserving all kinds of permissions on all of the cache, too

---

_Review comment by @zsol on `.github/workflows/bench.yml`:41 on 2026-01-13 15:26_

why change these?

---

_@zsol approved on 2026-01-13 15:26_

---

_@zanieb reviewed on 2026-01-13 15:27_

---

_Review comment by @zanieb on `.github/workflows/bench.yml`:41 on 2026-01-13 15:27_

I think it's just because we're doing an explicit build ahead of time now, but there's not really a strong reason to do it ahead of time unless we change the profile. (Claude just did this and I didn't really care enough to change it back)

---

_@zanieb reviewed on 2026-01-13 15:28_

---

_Review comment by @zanieb on `.github/workflows/bench.yml`:80 on 2026-01-13 15:28_

Hm okay. We use this pattern a lot but that does sound better.

---

_@zsol approved on 2026-01-13 15:35_

<a href="https://gitme.me/image?url=https%3A%2F%2Fmedia4.giphy.com%2Fmedia%2FNytMLKyiaIh6VH9SPm%2Fgiphy.gif&token=shipit" data-gitmeme-token="shipit"><img src="https://media4.giphy.com/media/NytMLKyiaIh6VH9SPm/giphy.gif" title="Created by gitme.me with /shipit"
        alt="shipit"/></a>

---

_Merged by @zanieb on 2026-01-13 15:49_

---

_Closed by @zanieb on 2026-01-13 15:49_

---
