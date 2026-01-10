```yaml
number: 2927
title: "misc: add pixi as installation way"
type: pull_request
state: closed
author: nichmor
labels: []
assignees: []
base: main
head: patch-1
created_at: 2024-04-09T05:02:08Z
updated_at: 2024-04-10T03:44:57Z
url: https://github.com/astral-sh/uv/pull/2927
synced_at: 2026-01-10T14:43:31Z
```

# misc: add pixi as installation way

---

_Pull request opened by @nichmor on 2024-04-09 05:02_

## Summary

Add [Pixi](https://pixi.sh/latest/) as one of the installation way of `uv`



---

_Comment by @charliermarsh on 2024-04-10 03:44_

Thanks for the PR! I'm actually looking to make this list smaller rather than larger, since take up such prominent real estate in the README and has a tendency to expand over time as uv gets re-packaged elsewhere. We already rejected one change to it in #2864, and I'm likely to remove Pacman for the same reason.

Happy to reconsider if we decide to add an expanded version of this once we have "real" docs.


---

_Closed by @charliermarsh on 2024-04-10 03:44_

---
