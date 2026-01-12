```yaml
number: 16148
title: "`resolution=lowest[-direct]` reports dependency as missing a lower bound when it actually has one"
type: issue
state: closed
author: neutrinoceros
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-10-07T08:09:51Z
updated_at: 2025-10-07T16:49:32Z
url: https://github.com/astral-sh/uv/issues/16148
synced_at: 2026-01-12T16:02:25Z
```

# `resolution=lowest[-direct]` reports dependency as missing a lower bound when it actually has one

---

_@neutrinoceros_

### Summary

Here's an mre `pyproject.toml`:

```toml
[project]
name = "test"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
  "coverage[toml] ; python_version < '3.11'",
  "coverage >= 7.10.0",
]

[build-system]
requires = ["uv_build>=0.8.23,<0.9.0"]
build-backend = "uv_build"
```

running `uv sync` with a `lowest-direct` resolution produces a warning
```
❯ uv sync --resolution=lowest-direct
⠙ test==0.1.0
warning: The direct dependency `coverage` is unpinned. Consider setting a lower bound when using `--resolution lowest` or `--resolution lowest-direct` to avoid using outdated versions.
Resolved 3 packages in 61ms
      Built test @ file:///private/tmp/test
Prepared 2 packages in 158ms
Installed 2 packages in 2ms
 + coverage==7.10.0
 + test==0.1.0 (from file:///private/tmp/test)
```

I think in this instance the warning is misleading (incorrect ?), because there exists no scenario where `coverage` would be resolved without a lower bound, but I figure the warning is the result of a sanity check that runs on single requirements, rather than on a "merged" view (for lack of a better word). I understand this could be tricky to fix because one would need to (partially) resolve the dependency tree ahead running this sanity check, which can easily turn into a chicken VS egg problem. It's a really minor thing however, and can easily be worked around on the user side by merely duplicating the lower bound; I just thought I might raise awareness on this quirk before resorting to that.

### Platform

Darwin 24.6.0 arm64

### Version

0.8.23 (00d3aa378 2025-10-04)

### Python version

I don't think that's relevant, but I saw this on 3.10

---

_Label `bug` added by @neutrinoceros on 2025-10-07 08:09_

---

_Label `great writeup` added by @konstin on 2025-10-07 08:20_

---

_Renamed from "BUG: `resolution=lowest[-direct]` reports dependency as missing a lower bound when it actually has one" to "`resolution=lowest[-direct]` reports dependency as missing a lower bound when it actually has one" by @konstin on 2025-10-07 08:36_

---

_Comment by @konstin on 2025-10-07 08:40_

Thank you for the clear MRE! Fixing in https://github.com/astral-sh/uv/pull/16149

---

_Comment by @neutrinoceros on 2025-10-07 08:45_

wow. It'll never cease to amaze me how reactive you guys can be. Thanks a lot @konstin !

---

_Comment by @neutrinoceros on 2025-10-07 16:49_

closing as completed with #16149 merged. Thanks again !

---

_Closed by @neutrinoceros on 2025-10-07 16:49_

---
