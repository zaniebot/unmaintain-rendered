```yaml
number: 3293
title: Fix span recording with install_wheel_rs
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-instrumentation-for-install-wheel-rs
created_at: 2024-04-28T18:25:22Z
updated_at: 2024-04-28T18:33:41Z
url: https://github.com/astral-sh/uv/pull/3293
synced_at: 2026-01-10T14:37:54Z
```

# Fix span recording with install_wheel_rs

---

_Pull request opened by @konstin on 2024-04-28 18:25_

We would only record spans for `uv` prefixed crates, while the rayon operations are in `install_wheel_rs`, therefore "disappearing" spans from rayon.

With these changes, we can profile the parallel installation:

![jupyter](https://github.com/astral-sh/uv/assets/6826232/bb3c626a-ba9a-443d-9727-ac1e3bf14a71)


---

_Label `internal` added by @konstin on 2024-04-28 18:25_

---

_Comment by @charliermarsh on 2024-04-28 18:28_

Should we rename the crate, or manually add our other crates to this filter? (Won't this lead to us tracing unrelated crates?)

---

_Review requested from @charliermarsh by @charliermarsh on 2024-04-28 18:28_

---

_Comment by @konstin on 2024-04-28 18:33_

I checked with `uv venv -p 3.12; TRACING_DURATIONS_FILE=target/traces/jupyter.ndjson cargo run --features tracing-durations-export pip install jupyter --no-cache-dir
` and didn't see any external spans. I'd just record everything, these filters are easy enough to change. Note that this change only affects tracing-durations-export, the cli logging uses `RUST_LOG` over it's default.

---

_Merged by @konstin on 2024-04-28 18:33_

---

_Closed by @konstin on 2024-04-28 18:33_

---

_Branch deleted on 2024-04-28 18:33_

---
