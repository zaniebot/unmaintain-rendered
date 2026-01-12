```yaml
number: 6973
title: "Feature request: allow script definition for package"
type: issue
state: open
author: marengaz
labels:
  - enhancement
  - uv tool
assignees: []
created_at: 2024-09-03T15:55:23Z
updated_at: 2024-09-03T17:21:44Z
url: https://github.com/astral-sh/uv/issues/6973
synced_at: 2026-01-12T15:59:09Z
```

# Feature request: allow script definition for package

---

_@marengaz_

hi all

i tried to install a package without any scripts defined. it told me "No executables are provided by `certifi`", but it was a surprise to see that it additionally "Deleting environment for tool `certifi`" in the debug logs. i had expected the venv to persist. perhaps that could be made more clear?

```
uv tool install certifi -v
DEBUG uv 0.4.3
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.5-macos-aarch64-none`
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/Users/ben/.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `.local/share/uv/tools`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: certifi*
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Searching for a compatible version of certifi (*)
DEBUG Selecting: certifi==2024.8.30 [compatible] (certifi-2024.8.30-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/12/90/3c9ff0512038035f59d279fddeb79f5f1eccd8859f06d6163c58798b9487/certifi-2024.8.30-py3-none-any.whl.metadata
DEBUG Tried 1 versions: certifi 1
DEBUG Split specific environment resolution took 0.001s
Resolved 1 package in 0.94ms
DEBUG Creating environment for tool `certifi`: .local/share/uv/tools/certifi
DEBUG Using request timeout of 30s
DEBUG Requirement already cached: certifi==2024.8.30
Installed 1 package in 1ms
 + certifi==2024.8.30
DEBUG Installing tool executables into: .local/bin
DEBUG Looking at `.dist-info` at: .local/share/uv/tools/certifi/lib/python3.12/site-packages/certifi-2024.8.30.dist-info
No executables are provided by `certifi`
DEBUG Looking at `.dist-info` at: .local/share/uv/tools/certifi/lib/python3.12/site-packages/certifi-2024.8.30.dist-info
DEBUG Deleting environment for tool `certifi` at .local/share/uv/tools/certifi
DEBUG Released lock at `/Users/ben/.local/share/uv/tools/.lock`
```

in the same vein, would it be possible to provide a script to uvx to be the executable?

for example:
`uv tool install certifi --script certifi=certifi`
or
`uv tool install certifi --script certifi=certifi.core:where`
or
`uv tool run certifi --script certifi=certifi`



---

_Comment by @zanieb on 2024-09-03 17:21_

If a package doesn't provide any executable scripts, yeah we'll remove the environment since there's nothing for us to expose.

It does seem reasonable to allow defining additional entry points. I'm not sure how easy it is, but it seems nice to have.

---

_Label `enhancement` added by @zanieb on 2024-09-03 17:21_

---

_Label `uv tool` added by @zanieb on 2024-09-03 17:21_

---
