---
number: 3326
title: "`uv` doesn't install the latest version of `dstack`"
type: issue
state: closed
author: peterschmidt85
labels:
  - question
  - resolver
assignees: []
created_at: 2024-04-30T17:58:07Z
updated_at: 2024-05-06T13:59:26Z
url: https://github.com/astral-sh/uv/issues/3326
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv` doesn't install the latest version of `dstack`

---

_Issue opened by @peterschmidt85 on 2024-04-30 17:58_

`uv` doesn't install the latest version of `dstack`

```
uv --version && uv pip install "dstack[all]" -U --dry-run
uv 0.1.39 (028b40741 2024-04-27)
Resolved 60 packages in 48ms
warning: The package `dstack==0.10.5` does not have an extra named `all`.
Would install 1 package
 + dstack==0.10.5
```

```
pip install "dstack[all]" -U --dry-run | grep -v 'already satisfied'
Collecting dstack[all]
  Using cached dstack-0.18.1-py3-none-any.whl.metadata (8.8 kB)
Collecting urllib3>=1.26.0 (from docker>=6.0.0->dstack[all])
  Using cached urllib3-1.26.18-py2.py3-none-any.whl.metadata (48 kB)
Using cached dstack-0.18.1-py3-none-any.whl (376 kB)
Using cached urllib3-1.26.18-py2.py3-none-any.whl (143 kB)
Would install dstack-0.18.1 urllib3-1.26.18
```



---

_Comment by @zanieb on 2024-04-30 18:07_

Hm the latest version has the pin

```
dstack[all]==0.18.1: azure-mgmt-network==23.0.0b2
```

and we say

```
DEBUG Searching for a compatible version of azure-mgmt-network (==23.0.0b2)
DEBUG No compatible version found for: azure-mgmt-network
```

I believe you need to opt-in to pre-releases here, which is a known divergence from pip. 

If this was unsolvable, we'd hint this in the error — but we do find a solution since you have no lower bound on dstack.

```
❯ uv pip install "dstack[all]>0.18" -U --dry-run
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of azure-mgmt-network==23.0.0b2 and dstack[all]==0.18.1 depends on
      azure-mgmt-network==23.0.0b2, we can conclude that dstack[all]==0.18.1 cannot be used.
      And because only the following versions of dstack[all] are available:
          dstack[all]<=0.18
          dstack[all]==0.18.1
      and you require dstack[all]>0.18, we can conclude that the requirements are unsatisfiable.

      hint: azure-mgmt-network was requested with a pre-release marker (e.g., azure-mgmt-network==23.0.0b2), but
      pre-releases weren't enabled (try: `--prerelease=allow`)
```

---

_Label `question` added by @zanieb on 2024-04-30 18:07_

---

_Label `resolver` added by @zanieb on 2024-04-30 18:08_

---

_Closed by @charliermarsh on 2024-05-06 13:59_

---
