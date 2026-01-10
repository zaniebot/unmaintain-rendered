```yaml
number: 1322
title: Change benchmarks to use 3.12.1
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/bench-3121
created_at: 2024-02-15T18:25:19Z
updated_at: 2024-02-17T18:00:51Z
url: https://github.com/astral-sh/uv/pull/1322
synced_at: 2026-01-10T15:33:24Z
```

# Change benchmarks to use 3.12.1

---

_Pull request opened by @zanieb on 2024-02-15 18:25_

We can wait to merge this until later, but we explicitly bootstrap 3.12.1 and the benches should not pin 3.12.0

---

_Comment by @AlexWaygood on 2024-02-15 18:25_

...why not 3.12.2? ;)

---

_Comment by @zanieb on 2024-02-15 18:26_

We have 3.12.1 in our `python-versions` lock-file. We can bump everything to that separately.

---

_Label `internal` added by @zanieb on 2024-02-15 18:50_

---

_Marked ready for review by @zanieb on 2024-02-16 05:16_

---

_Comment by @zanieb on 2024-02-16 05:17_

Only delay here is that I don't want to make Charlie feel like he has to regenerate all the benches 😄 

---

_Comment by @MichaReiser on 2024-02-16 07:38_

There's a reference to the required python version in the Benchmark.md that needs updating.

---

_Comment by @zanieb on 2024-02-16 17:11_

I don't think that should be bumped until the benchmarks are regenerated

---

_Review requested from @charliermarsh by @zanieb on 2024-02-16 17:11_

---

_Merged by @zanieb on 2024-02-17 18:00_

---

_Closed by @zanieb on 2024-02-17 18:00_

---

_Branch deleted on 2024-02-17 18:00_

---
