```yaml
number: 12551
title: changed where the fish completions get installed to where fish docs recommend
type: pull_request
state: merged
author: ndrew222
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2025-03-30T06:56:55Z
updated_at: 2025-03-30T15:47:05Z
url: https://github.com/astral-sh/uv/pull/12551
synced_at: 2026-01-12T16:10:19Z
```

# changed where the fish completions get installed to where fish docs recommend

---

_@ndrew222_

## Summary
I only changed the location of where the fish completions get sent, from `~/.config/fish/config.fish` to `~/.config/fish/completions/uv.fish` and `~/.config/fish/completions/uvx.fish` respectively

## Test Plan
I have tested and putting the completions in those paths works fine and complies with the fish docs. Also keeps your `config.fish` clean


### edit:
refer to https://fishshell.com/docs/current/completions.html#where-to-put-completions
> This wide search may be confusing. If you are unsure, your completions probably belong in `~/.config/fish/completions`.

---

_@charliermarsh approved on 2025-03-30 15:46_

---

_Label `documentation` added by @charliermarsh on 2025-03-30 15:46_

---

_Merged by @charliermarsh on 2025-03-30 15:47_

---

_Closed by @charliermarsh on 2025-03-30 15:47_

---

_Comment by @charliermarsh on 2025-03-30 15:47_

Makes sense, thanks!

---
