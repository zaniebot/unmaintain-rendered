```yaml
number: 824
title: "Migrate back to `owo-colors`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/owo
created_at: 2024-01-07T02:10:21Z
updated_at: 2024-01-08T08:54:58Z
url: https://github.com/astral-sh/uv/pull/824
synced_at: 2026-01-12T16:04:13Z
```

# Migrate back to `owo-colors`

---

_@charliermarsh_

In the past, I moved us to `owo-colors` (https://github.com/astral-sh/puffin/pull/121); then, we moved back, because we ran into issues with overriding the settings to force-disable colors. But `anstream` solved those problems, so I'm moving us _back_ to `owo-colors`, since it's what `anstream` recommends, and it's already used by many of our dependencies (`miette`, `configparser`).


---

_Label `internal` added by @charliermarsh on 2024-01-07 02:13_

---

_Marked ready for review by @charliermarsh on 2024-01-07 02:13_

---

_Review requested from @konstin by @charliermarsh on 2024-01-07 02:46_

---

_@MichaReiser approved on 2024-01-08 08:51_

Should we change `ruff`` too? I like the `owo-colors` API but remember that I noticed a slight performance regression when using it

---

_@konstin approved on 2024-01-08 08:51_

---

_Merged by @konstin on 2024-01-08 08:54_

---

_Closed by @konstin on 2024-01-08 08:54_

---

_Branch deleted on 2024-01-08 08:54_

---
