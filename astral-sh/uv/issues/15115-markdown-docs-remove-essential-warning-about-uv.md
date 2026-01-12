```yaml
number: 15115
title: "Markdown docs remove essential warning about `UV_LINK_MODE=symlink` from extended description"
type: issue
state: closed
author: konstin
labels:
  - bug
  - documentation
assignees: []
created_at: 2025-08-06T18:50:26Z
updated_at: 2025-08-07T13:57:00Z
url: https://github.com/astral-sh/uv/issues/15115
synced_at: 2026-01-12T16:02:04Z
```

# Markdown docs remove essential warning about `UV_LINK_MODE=symlink` from extended description

---

_@konstin_

`UV_LINK_MODE=symlink` has a warning attach that it's discouraged due to the risk of breaking venvs when clearing the cache. The warning is missing in the docs, where we show the description without the warning. The warning is present in `uv.schema.json`. This has hit a user: https://github.com/astral-sh/uv/issues/15104#issuecomment-3160179933. We should ensure that we don't drop extended docstrings from the docs, and that this warning is present.

Code:

https://github.com/astral-sh/uv/blob/5d37c7ecc54ce317b3b453ce1b3a8b9c9f242fa4/crates/uv-install-wheel/src/linker.rs#L18-L36

Rendered docs:

<img width="1071" height="860" alt="Image" src="https://github.com/user-attachments/assets/cf080ecd-9138-47ab-9870-d3bb872f8677" />

Possible culprit:

https://github.com/astral-sh/uv/blob/c5032aee80d5a666a45ec5e095b8c8abeffc11fb/crates/uv-macros/src/options_metadata.rs#L238



---

_Label `bug` added by @konstin on 2025-08-06 18:50_

---

_Label `documentation` added by @konstin on 2025-08-06 18:50_

---

_Comment by @zanieb on 2025-08-06 21:45_

Elsewhere we have like `opt.get_long_help().or_else(|| opt.get_help())`

---

_Comment by @zanieb on 2025-08-06 21:45_

We'll need to update the output to handle multiline formatting here, I presume?



---

_Closed by @konstin on 2025-08-07 13:57_

---
