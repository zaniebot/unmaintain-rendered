```yaml
number: 806
title: "Improve `requires-python` incompatibility message"
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-01-05T22:20:50Z
updated_at: 2024-01-22T06:32:04Z
url: https://github.com/astral-sh/uv/issues/806
synced_at: 2026-01-12T15:58:24Z
```

# Improve `requires-python` incompatibility message

---

_@zanieb_

e.g. in the following test case

https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L2232-L2233

we say "there is no version of Python available matching >=3.10" but there may be such a Python version available we are just not resolving with it. We should adjust the language to be more correct, include a note about the current Python version, and perhaps hint at resolving with another Python version in `pip-compile` cases?

---

_Label `error messages` added by @zanieb on 2024-01-05 22:36_

---

_Closed by @zanieb on 2024-01-22 06:32_

---
