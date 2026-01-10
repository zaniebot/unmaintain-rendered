```yaml
number: 816
title: Use tracing-durations-export to visualize parallelism bottlenecks (dev commands)
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/tracing-durations-export
created_at: 2024-01-06T15:39:14Z
updated_at: 2024-01-08T15:20:39Z
url: https://github.com/astral-sh/uv/pull/816
synced_at: 2026-01-10T15:44:44Z
```

# Use tracing-durations-export to visualize parallelism bottlenecks (dev commands)

---

_Pull request opened by @konstin on 2024-01-06 15:39_

Example usage:

```
# Cached
TRACING_DURATIONS_FILE=target/traces/black.ndjson RUST_LOG=puffin=info cargo run --bin puffin-dev --profile profiling -- resolve black
TRACING_DURATIONS_FILE=target/traces/meine_stadt_transparent.ndjson RUST_LOG=puffin=info cargo run --bin puffin-dev --profile profiling -- resolve meine_stadt_transparent
TRACING_DURATIONS_FILE=target/traces/jupyter.ndjson RUST_LOG=puffin=info cargo run --bin puffin-dev --profile profiling -- resolve jupyter

# No cache
TRACING_DURATIONS_FILE=target/traces/black-no-cache.ndjson RUST_LOG=puffin=info cargo run --bin puffin-dev --profile profiling -- resolve --no-cache black
TRACING_DURATIONS_FILE=target/traces/meine_stadt_transparent-no-cache.ndjson RUST_LOG=puffin=info cargo run --bin puffin-dev --profile profiling -- resolve --no-cache meine_stadt_transparent
TRACING_DURATIONS_FILE=target/traces/jupyter-no-cache.ndjson RUST_LOG=puffin=info cargo run --bin puffin-dev --profile profiling -- resolve --no-cache jupyter
```

Uncached black output example:

![black-no-cache](https://github.com/astral-sh/puffin/assets/6826232/38497b89-7214-453b-9456-c9d9cbf7d2d5)



---

_Renamed from "Use tracing-durations-export to visualize parallelism bottlenecks" to "Use tracing-durations-export to visualize parallelism bottlenecks (dev commands)" by @konstin on 2024-01-08 10:08_

---

_Marked ready for review by @konstin on 2024-01-08 10:11_

---

_Comment by @konstin on 2024-01-08 10:12_

Notably, this PR modifies puffin-dev only.

---

_@charliermarsh approved on 2024-01-08 14:49_

---

_Merged by @konstin on 2024-01-08 15:20_

---

_Closed by @konstin on 2024-01-08 15:20_

---

_Branch deleted on 2024-01-08 15:20_

---
