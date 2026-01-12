```yaml
number: 8942
title: "Respect `--index-url` in `uv pip list`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/i
created_at: 2024-11-08T14:17:32Z
updated_at: 2024-11-08T14:52:35Z
url: https://github.com/astral-sh/uv/pull/8942
synced_at: 2026-01-12T16:08:34Z
```

# Respect `--index-url` in `uv pip list`

---

_@charliermarsh_

## Summary

As an oversight, these arguments weren't being respected from the CLI or elsewhere -- we always hit PyPI, ignored `--exclude-newer`, etc. It has to do with the way that the `PipOptions` are setup -- there's a global struct that we pass around everywhere and fill in with defaults, so there's no type safety to guarantee that we provide whatever it is we need to use in the command. The newer APIs are much better about this.

Closes #8927.


---

_Label `bug` added by @charliermarsh on 2024-11-08 14:17_

---

_@zanieb approved on 2024-11-08 14:38_

---

_Merged by @charliermarsh on 2024-11-08 14:52_

---

_Closed by @charliermarsh on 2024-11-08 14:52_

---

_Branch deleted on 2024-11-08 14:52_

---
