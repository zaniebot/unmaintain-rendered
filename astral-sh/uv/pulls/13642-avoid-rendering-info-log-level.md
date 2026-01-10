```yaml
number: 13642
title: Avoid rendering info log level
type: pull_request
state: merged
author: konstin
labels:
  - performance
  - tracing
assignees: []
merged: true
base: main
head: konsti/avoid-rendering-info-log-level
created_at: 2025-05-25T12:37:06Z
updated_at: 2025-05-26T13:17:18Z
url: https://github.com/astral-sh/uv/pull/13642
synced_at: 2026-01-10T11:10:42Z
```

# Avoid rendering info log level

---

_Pull request opened by @konstin on 2025-05-25 12:37_

We were previously rendering messages for the info level, carrying overhead in pubgrub which using `log::info!`. We avoid this by only configuring `LevelFilter::INFO` if the durations layer exists.

I've confirmed that the default `Subscriber::max_level_hint` goes from `INFO` to `OFF` and the profile skips `Incompatibility::display`.

---

_Label `performance` added by @konstin on 2025-05-25 12:37_

---

_Label `tracing` added by @konstin on 2025-05-25 12:37_

---

_@charliermarsh approved on 2025-05-25 16:50_

---

_Merged by @konstin on 2025-05-26 13:17_

---

_Closed by @konstin on 2025-05-26 13:17_

---

_Branch deleted on 2025-05-26 13:17_

---
