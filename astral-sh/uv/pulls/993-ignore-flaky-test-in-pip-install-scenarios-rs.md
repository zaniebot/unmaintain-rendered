```yaml
number: 993
title: "Ignore flaky test in `pip_install_scenarios.rs`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/flake
created_at: 2024-01-19T02:04:50Z
updated_at: 2024-01-19T05:53:31Z
url: https://github.com/astral-sh/uv/pull/993
synced_at: 2026-01-10T15:39:03Z
```

# Ignore flaky test in `pip_install_scenarios.rs`

---

_Pull request opened by @charliermarsh on 2024-01-19 02:04_

I don't know if we want to remove the scenario, or add this to the generation script? But I'm losing a lot of time to CI failures due to this so just want to disable ASAP.

See: https://github.com/astral-sh/puffin/issues/863.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-19 02:04_

---

_Label `internal` added by @charliermarsh on 2024-01-19 02:04_

---

_Comment by @zanieb on 2024-01-19 02:33_

https://github.com/astral-sh/puffin/pull/996 is an alternative option

---

_Comment by @charliermarsh on 2024-01-19 05:53_

Closing in favor of #996.

---

_Closed by @charliermarsh on 2024-01-19 05:53_

---
