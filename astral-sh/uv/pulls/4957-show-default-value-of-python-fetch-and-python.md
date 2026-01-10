```yaml
number: 4957
title: "Show default value of `python-fetch` and `python-preference` in doc"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
draft: true
base: main
head: arg-default
created_at: 2024-07-10T08:13:45Z
updated_at: 2024-08-02T16:54:56Z
url: https://github.com/astral-sh/uv/pull/4957
synced_at: 2026-01-10T13:37:23Z
```

# Show default value of `python-fetch` and `python-preference` in doc

---

_Pull request opened by @j178 on 2024-07-10 08:13_

## Summary

It's unclear what the default behaviors of `--python-fetch` and `--python-preference` are. This would help clarify their usage.


---

_Converted to draft by @j178 on 2024-07-10 08:25_

---

_Comment by @j178 on 2024-07-10 08:40_

It appears that `--python-preference` is always set to `installed` for the `Project`, `Tool` and `Python` commands, and varies based on `--preview` setting for other commands.

---

_Comment by @zanieb on 2024-07-10 13:53_

Yeah we're changing the default behavior in preview and those commands are preview-only so the behavior is already changed over there.

---

_Closed by @j178 on 2024-08-02 16:54_

---

_Branch deleted on 2024-08-02 16:54_

---
