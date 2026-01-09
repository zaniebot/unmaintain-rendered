---
number: 6855
title: "Feature Request [tool]: allow dumping installed packages to a file and re-installing from that file"
type: issue
state: open
author: xavdid
labels:
  - enhancement
assignees: []
created_at: 2024-08-30T07:38:45Z
updated_at: 2024-08-30T12:46:31Z
url: https://github.com/astral-sh/uv/issues/6855
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature Request [tool]: allow dumping installed packages to a file and re-installing from that file

---

_Issue opened by @xavdid on 2024-08-30 07:38_

A useful feature of `pipx` is the ability to both: 

- list all of its installed packages in JSON format
- install everything listed in a JSON file

This allows for backing up (and checking in) of an entire set of tools:

```bash
pipx list --json > my.json
# on another computer:
pipx install-all my.json
```

Specifically, I use this [in my dotfiles](https://github.com/xavdid/dotfiles/blob/master/python/pipx.json) to build the set of globally available commands I expect on new computers. It's great that it works with nested packages, too (e.g. packages `b` and `c` being installed as subsets of `a`, which `uv` already supports).

The JSON format isn't important specifically, but it's nice that it's human-readable.

## References

- https://github.com/pypa/pipx/pull/660
- https://github.com/pypa/pipx/pull/1301


---

_Label `enhancement` added by @charliermarsh on 2024-08-30 12:46_

---
