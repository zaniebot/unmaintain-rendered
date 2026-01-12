```yaml
number: 12456
title: "`uvx --from` functionality for `uv run --script`"
type: issue
state: open
author: j99ca
labels:
  - enhancement
assignees: []
created_at: 2025-03-25T00:17:32Z
updated_at: 2025-03-25T03:08:05Z
url: https://github.com/astral-sh/uv/issues/12456
synced_at: 2026-01-12T16:01:03Z
```

# `uvx --from` functionality for `uv run --script`

---

_@j99ca_

### Summary

uvx has an awesome feature where you can specify `uvx --from` to include scripts hosted remotely and within private github/bitbucket repos as long as you have proper ssh access setup.

For example:
`uvx --from git+ssh://git@bitbucket.org/account/repo command`

It would be great if a similar ability was added for `uv run --script git+ssh://git@bitbucket.org/account/...` to allow execution of scripts formatted with [PEP 723](https://peps.python.org/pep-0723/). Requirements for PEP 723 formatted scripts may differ from the projects that contain them.

### Example
Perhaps
`uv run --from git+ssh://git@bitbucket.org/account/repo --script x/y/z.py`
Or 
`uv run --script git+ssh://git@bitbucket.org/account/repo/x/y/z.py`

---

_Label `enhancement` added by @j99ca on 2025-03-25 00:17_

---

_Renamed from "uvx --from functionality for uv run --script" to "`uvx --from` functionality for `uv run --script`" by @j99ca on 2025-03-25 00:22_

---

_Comment by @zanieb on 2025-03-25 02:08_

We already support remote scripts https://github.com/astral-sh/uv/pull/6375, though perhaps not from repositories that need to be cloned?

---

_Comment by @j99ca on 2025-03-25 03:08_

@zanieb as far as I know `uv run --script` does not offer [VCS](https://pip.pypa.io/en/stable/topics/vcs-support/) support like uvx does for tooling. I've tried a few different formats but perhaps I am missing something. Most of our "remote" scripts are behind private repos so the remote feature of `uv run` does not work for us and requires cloning the repo and then specifying the script locally.

---
