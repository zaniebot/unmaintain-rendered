```yaml
number: 14662
title: Add support for toggling Python bin and registry install options via env vars
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: zb/install-bin-env
created_at: 2025-07-16T15:40:25Z
updated_at: 2025-07-17T17:33:45Z
url: https://github.com/astral-sh/uv/pull/14662
synced_at: 2026-01-12T16:11:20Z
```

# Add support for toggling Python bin and registry install options via env vars

---

_@zanieb_

Adds environment variables for https://github.com/astral-sh/uv/pull/14612 and https://github.com/astral-sh/uv/pull/14614

We can't use the Clap `BoolishValueParser` here, and the reasoning is a little hard to explain. If we used `UV_PYTHON_INSTALL_NO_BIN`, as is our typical pattern, it'd work, but here we allow opt-in to hard errors with `UV_PYTHON_INSTALL_BIN=1` and I don't think we should have both `UV_PYTHON_INSTALL_BIN` and `UV_PYTHON_INSTALL_NO_BIN`.

Consequently, this pull request introduces a new `EnvironmentOptions` abstraction which allows us to express semantics that Clap cannot — which we probably want anyway because we have an increasing number of environment variables we're parsing downstream, e.g., #14544 and #14369.

---

_Label `configuration` added by @zanieb on 2025-07-16 15:40_

---

_Label `preview` added by @zanieb on 2025-07-16 15:40_

---

_Label `configuration` added by @zanieb on 2025-07-16 15:40_

---

_Label `preview` added by @zanieb on 2025-07-16 15:40_

---

_@konstin approved on 2025-07-17 17:20_

---

_Merged by @zanieb on 2025-07-17 17:33_

---

_Closed by @zanieb on 2025-07-17 17:33_

---

_Branch deleted on 2025-07-17 17:33_

---
