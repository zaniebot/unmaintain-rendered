---
number: 13761
title: "show extras for `uv tool list`"
type: issue
state: closed
author: gaardhus
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-06-01T15:51:38Z
updated_at: 2025-06-03T09:13:19Z
url: https://github.com/astral-sh/uv/issues/13761
synced_at: 2026-01-07T13:12:18-06:00
---

# show extras for `uv tool list`

---

_Issue opened by @gaardhus on 2025-06-01 15:51_

### Summary

I just wanted to check if I already had installed an extra with a tool, but couldn't find a way to do it with the CLI.

```bash
$ uv tool install 'harlequin[postgres]'
$ uv tool list
harlequin v2.1.2
- harlequin
```

### Example

I would expect either the extras to show per default, or to have an option to show them i.e. `--show-extras`

Which could then show something like.

```
$ uv tool install 'harlequin[postgres]'
$ uv tool list --show-extras
harlequin v2.1.2 [extras: postgres]
- harlequin
```

Let me know if I missed something, you have another suggestion for the behavior, and/or want me to implement it.

---

_Label `enhancement` added by @gaardhus on 2025-06-01 15:51_

---

_Comment by @charliermarsh on 2025-06-02 00:11_

I think it seems reasonable to support this as `--show-extras`, similar to `--show-specifiers`.

---

_Label `help wanted` added by @charliermarsh on 2025-06-02 00:11_

---

_Referenced in [astral-sh/uv#13783](../../astral-sh/uv/pulls/13783.md) on 2025-06-02 13:07_

---

_Closed by @gaardhus on 2025-06-03 09:13_

---
