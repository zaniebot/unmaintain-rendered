---
number: 14023
title: "`uv tool install --with <name> --with <name>` writes duplicate requirements to the receipt"
type: issue
state: open
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-06-13T13:30:03Z
updated_at: 2025-06-13T14:02:28Z
url: https://github.com/astral-sh/uv/issues/14023
synced_at: 2026-01-10T01:25:41Z
---

# `uv tool install --with <name> --with <name>` writes duplicate requirements to the receipt

---

_Issue opened by @zanieb on 2025-06-13 13:30_

### Summary

e.g.

```
❯ uv tool install ruff --with httpie --with httpie
Resolved 17 packages in 35ms
Installed 17 packages in 23ms
 + certifi==2025.4.26
 + charset-normalizer==3.4.2
 + defusedxml==0.7.1
 + httpie==3.2.4
 + idna==3.10
 + markdown-it-py==3.0.0
 + mdurl==0.1.2
 + multidict==6.4.4
 + pip==25.1.1
 + pygments==2.19.1
 + pysocks==1.7.1
 + requests==2.32.4
 + requests-toolbelt==1.0.0
 + rich==14.0.0
 + ruff==0.11.13
 + setuptools==80.9.0
 + urllib3==2.4.0
Installed 1 executable: ruff
❯ cat $(uv tool dir)/ruff/uv-receipt.toml
[tool]
requirements = [
    { name = "ruff" },
    { name = "httpie" },
    { name = "httpie" },
]
entrypoints = [
    { name = "ruff", install-path = "/Users/zb/.local/bin/ruff", from = "ruff" },
]
```

We also don't merge specifiers, e.g.:

```
❯ uv tool install ruff --with 'httpie<1.0' --with 'httpie<2.0'
Resolved 8 packages in 112ms
Prepared 1 package in 53ms
Uninstalled 10 packages in 63ms
Installed 1 package in 8ms
 - defusedxml==0.7.1
 - httpie==3.2.4
 + httpie==0.9.9
 - markdown-it-py==3.0.0
 - mdurl==0.1.2
 - multidict==6.4.4
 - pip==25.1.1
 - pysocks==1.7.1
 - requests-toolbelt==1.0.0
 - rich==14.0.0
 - setuptools==80.9.0
Installed 1 executable: ruff
❯ cat $(uv tool dir)/ruff/uv-receipt.toml
[tool]
requirements = [
    { name = "ruff" },
    { name = "httpie", specifier = "<1.0" },
    { name = "httpie", specifier = "<2.0" },
]
entrypoints = [
    { name = "ruff", install-path = "/Users/zb/.local/bin/ruff", from = "ruff" },
]
```

### Platform

macOS

### Version

main

### Python version

n/a

---

_Label `bug` added by @zanieb on 2025-06-13 13:30_

---

_Comment by @charliermarsh on 2025-06-13 13:55_

It looks like a faithful representation of the request, though I think either option is fine (normalizing or not).

---

_Comment by @zanieb on 2025-06-13 14:02_

I don't have strong feelings, I guess. I do find it weird though.

---
