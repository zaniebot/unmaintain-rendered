---
number: 11676
title: "`uv pip list` option to show origin (PyPI, GitHub, etc)"
type: issue
state: open
author: jamesbraza
labels:
  - enhancement
assignees: []
created_at: 2025-02-20T19:46:17Z
updated_at: 2025-02-23T16:15:53Z
url: https://github.com/astral-sh/uv/issues/11676
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip list` option to show origin (PyPI, GitHub, etc)

---

_Issue opened by @jamesbraza on 2025-02-20 19:46_

### Summary

It would be useful to add some opt-in flag (e.g. `--show-origin`) to show the origin of packages in `uv pip list`.

I believe this information is stored in `direct_url.json` in package metadata.

### Example

```
> uv pip list --show-origin
Package      Version       Index    URL
------------ ------------- -------- ------------------------------------------------
accelerate   1.3.0         PyPI     https://pypi.org/simple/accelerate/
trl          0.15.0.dev0   GitHub   git+https://github.com/org/trl.git@branch-name
```

A disclaimer is I am unsure what the exact URL would be for a PyPI package.

---

_Label `enhancement` added by @jamesbraza on 2025-02-20 19:46_

---

_Comment by @insane-dreamer on 2025-02-23 16:15_

`conda list` specifies the source (conda-forge, pypi) which is handy; this would be similar to that functionality 



---
