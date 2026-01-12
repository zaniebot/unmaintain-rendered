```yaml
number: 10928
title: npm style script support
type: issue
state: closed
author: allenc97
labels:
  - enhancement
assignees: []
created_at: 2025-01-24T05:53:45Z
updated_at: 2025-01-24T14:44:08Z
url: https://github.com/astral-sh/uv/issues/10928
synced_at: 2026-01-12T16:00:24Z
```

# npm style script support

---

_@allenc97_

### Summary

Currently, uv supports entrypoint scripts but strictly requires package installation, which is problematic for standard applications with no packaging intentions. It would be great to support certain features of npm's scripts, e.g defining commands that are pre-configured with certain flags, as a new subcommand.



### Example
In similar fashion to npm:
`"run_with_debug": "uv run --directory src flask --app hello run --debug" -> uv execute run_with_debug`
Assuming paths are properly configured, this would also allow the user to execute a command such as the one above from any subdirectory under the project's root.

---

_Label `enhancement` added by @allenc97 on 2025-01-24 05:53_

---

_Comment by @cthoyt on 2025-01-24 08:41_

you should check out similar issue with long discussion: https://github.com/astral-sh/uv/issues/5903

---

_Closed by @charliermarsh on 2025-01-24 14:44_

---
