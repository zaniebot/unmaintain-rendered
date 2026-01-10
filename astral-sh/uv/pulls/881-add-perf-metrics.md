```yaml
number: 881
title: Add perf metrics
type: pull_request
state: closed
author: bojanserafimov
labels: []
assignees: []
base: main
head: bojan/perf-metrics
created_at: 2024-01-11T01:31:02Z
updated_at: 2024-01-20T02:18:37Z
url: https://github.com/astral-sh/uv/pull/881
synced_at: 2026-01-10T15:39:02Z
```

# Add perf metrics

---

_Pull request opened by @bojanserafimov on 2024-01-11 01:31_

Add debug log lines:
```
    0.922954s DEBUG puffin::commands::pip_sync total remote size: 29.841496MB
    0.922961s DEBUG puffin::commands::pip_sync effective processing rate: 36.820007MB/s
    0.922966s DEBUG puffin::commands::pip_sync number of source distributions: 1
```

The intended use of this is if you notice suspiciously slow downloads you cansee if the effective processing rate matches your internet speed. If it doesn't, and there are 0 source distributions, something is lagging.

---

_Comment by @zanieb on 2024-01-11 02:46_

Should we use `tracing` for this instead?

---

_Comment by @bojanserafimov on 2024-01-11 14:29_

> Should we use tracing for this instead?

As in, `tracing::debug!(...)`?

---

_Comment by @zanieb on 2024-01-11 20:37_

Yeah we're already using it elsewhere. I don't have strong feelings — it just seems weird to have a dedicated environment variable for this.

---

_Marked ready for review by @bojanserafimov on 2024-01-11 21:27_

---

_@konstin reviewed on 2024-01-12 09:19_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:231 on 2024-01-12 09:19_

You can use `bytesize` here

---

_Closed by @charliermarsh on 2024-01-20 02:18_

---
