---
number: 11011
title: Support direct links to Github API to link dependencies in tools.uv.sources
type: issue
state: closed
author: begoon
labels:
  - enhancement
assignees: []
created_at: 2025-01-28T01:12:10Z
updated_at: 2025-01-28T14:01:58Z
url: https://github.com/astral-sh/uv/issues/11011
synced_at: 2026-01-07T13:12:18-06:00
---

# Support direct links to Github API to link dependencies in tools.uv.sources

---

_Issue opened by @begoon on 2025-01-28 01:12_

### Summary

GitHub allows to download tarballs via direct links like `https://api.github.com/repos/$OWNER/$REPO/tarball/$SHA1` with specific SHA1s.

Currently, `uv` does not recognize such links because there is no extension.

However, if `curl -L https://api.github.com/repos/$OWNER/$REPO/tarball/$SHA1 -o $REPO.tar.gz`, `uv` perfectly picks it up via `path` from the local file system.

`git+`, however, requires `git` installed, which is not always available.

### Example

_No response_

---

_Label `enhancement` added by @begoon on 2025-01-28 01:12_

---

_Comment by @charliermarsh on 2025-01-28 02:57_

This makes sense, thanks.

---

_Comment by @charliermarsh on 2025-01-28 02:58_

How is this different than, e.g.:

```
https://github.com/github/codeql/archive/aef66c462abe817e33aad91d97aa782a1e2ad2c7.tar.gz
```

---

_Comment by @begoon on 2025-01-28 11:13_

@charliermarsh, it works beautifully, thanks!

---

_Comment by @begoon on 2025-01-28 11:14_

I have closed this according to the solution from @charliermarsh above.

---

_Closed by @begoon on 2025-01-28 11:14_

---

_Comment by @charliermarsh on 2025-01-28 14:01_

Awesome, thanks @begoon!

---
