---
number: 15002
title: Check if pylock.toml satisfies requirements.txt
type: issue
state: open
author: williamAlhant
labels:
  - enhancement
assignees: []
created_at: 2025-07-31T17:28:05Z
updated_at: 2025-07-31T17:28:05Z
url: https://github.com/astral-sh/uv/issues/15002
synced_at: 2026-01-10T01:25:51Z
---

# Check if pylock.toml satisfies requirements.txt

---

_Issue opened by @williamAlhant on 2025-07-31 17:28_

### Summary

Hello,

Would it be possible, in a pip workflow, to check if a pylock.toml is up-to-date and satisfies a requirements.txt ?

### Example

`uv pip compile requirements.txt -o pylock.toml`
`uv [some-command] requirements.txt pylock.toml` -> returns successful
*add new requirement in requirements.txt*
`uv [some-command] requirements.txt pylock.toml` -> returns error if the new requirement is not satisfied by pylock.toml

---

_Label `enhancement` added by @williamAlhant on 2025-07-31 17:28_

---
