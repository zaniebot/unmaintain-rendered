```yaml
number: 13565
title: "Install PyPy to PATH as `pypy`/`pypy3`"
type: issue
state: open
author: injust
labels:
  - enhancement
assignees: []
created_at: 2025-05-21T07:05:28Z
updated_at: 2025-06-22T23:52:21Z
url: https://github.com/astral-sh/uv/issues/13565
synced_at: 2026-01-10T03:32:45Z
```

# Install PyPy to PATH as `pypy`/`pypy3`

---

_Issue opened by @injust on 2025-05-21 07:05_

### Summary

I would like to have `pypy3` in my PATH so I can run it when I know I need PyPy specifically, but also keep CPython as the default `python3` in my PATH. This doesn't seem possible with `uv python install` at the moment.

Currently, running `uv python install pypy --preview` installs it as `python3.11` and adding `--default` installs it as `python`/`python3`.

---

_Label `enhancement` added by @injust on 2025-05-21 07:05_

---

_Assigned to @zanieb by @konstin on 2025-05-21 07:11_

---

_Comment by @RazerM on 2025-06-22 23:52_

I opened a PR based on the idea that this would not be configurable — which might not be what the Astral folks have in mind.

It would be my preference though, and matches what other package managers like `brew` or `apt` do.

---
