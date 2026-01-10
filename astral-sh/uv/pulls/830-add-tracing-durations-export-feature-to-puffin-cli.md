```yaml
number: 830
title: Add tracing_durations_export feature to puffin-cli
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/tracing-durations-export-main
created_at: 2024-01-08T10:14:40Z
updated_at: 2024-01-08T15:20:46Z
url: https://github.com/astral-sh/uv/pull/830
synced_at: 2026-01-10T15:44:44Z
```

# Add tracing_durations_export feature to puffin-cli

---

_Pull request opened by @konstin on 2024-01-08 10:14_

The optional `tracing-durations-export` feature allows creating parallelism plots from all puffin-cli commands without affecting production builds.

Usage:

```
virtualenv --clear -p 3.10 .venv310 && TRACING_DURATIONS_FILE=target/traces/jupyter-no-cache.ndjson RUST_LOG=puffin=info VIRTUAL_ENV=.venv310 cargo run --bin puffin --profile profiling --features tracing-durations-export -- pip-install -v --no-cache jupyter
virtualenv --clear -p 3.10 .venv310 && TRACING_DURATIONS_FILE=target/traces/jupyter.ndjson RUST_LOG=puffin=info VIRTUAL_ENV=.venv310 cargo run --bin puffin --profile profiling --features tracing-durations-export -- pip-install -v jupyter
 ```

Output, plotted in collapsed mode for readability:

Cached jupyter:

![jupyter](https://github.com/astral-sh/puffin/assets/6826232/f7e03c68-0438-4cf4-bceb-9a4a146cc506)

Uncached jupyter:

![image](https://github.com/astral-sh/puffin/assets/6826232/cfdd3383-7a9d-43d6-b8d0-201f64611596)


---

_@charliermarsh approved on 2024-01-08 14:48_

---

_Merged by @konstin on 2024-01-08 15:20_

---

_Closed by @konstin on 2024-01-08 15:20_

---

_Branch deleted on 2024-01-08 15:20_

---
