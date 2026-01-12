```yaml
number: 16913
title: "Add help page information to the `less` pager prompt again"
type: issue
state: open
author: EliteTK
labels:
  - enhancement
assignees: []
created_at: 2025-12-01T18:36:01Z
updated_at: 2025-12-01T18:36:01Z
url: https://github.com/astral-sh/uv/issues/16913
synced_at: 2026-01-12T16:02:40Z
```

# Add help page information to the `less` pager prompt again

---

_@EliteTK_

### Summary

When using `less` to view help pages, uv should use the `LESS` variable to set the prompt (`-P`) in a fail-safe way (either works or is silently ignored) to tell the user they're viewing `uv help` output and the command for which they're viewing it.

The `LESS` variable follows an odd format and may already have things in it (user specific customization). The prompt value may need to be cleaned/escaped for this.

**Background**: Previously uv would try to set the `less` prompt to: "**uv help**: <the command you asked for help with>". However, busybox less would break when passed `-P`, breaking the paging functionality (#16015). It was also spotted that everything after and including the colon was being dropped which meant that this feature wasn't serving its intended purpose. #16908 removed `-P` to avoid triggering the busybox issue.

### Example

Running `uv help run` and scrolling down, a user would continue to see information about the current page in the prompt (bottom left corner):

<img width="1216" height="542" alt="Image" src="https://github.com/user-attachments/assets/a0e95fa6-4dea-41bc-b90f-4f709ea2f45b" />

---

_Label `enhancement` added by @EliteTK on 2025-12-01 18:36_

---
