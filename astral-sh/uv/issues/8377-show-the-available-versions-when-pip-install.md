---
number: 8377
title: "Show the available versions when `pip install package==x.y.z` is unsatisfiable due to version"
type: issue
state: open
author: pantheraleo-7
labels: []
assignees: []
created_at: 2024-10-20T06:39:33Z
updated_at: 2024-10-20T06:39:33Z
url: https://github.com/astral-sh/uv/issues/8377
synced_at: 2026-01-10T01:24:27Z
---

# Show the available versions when `pip install package==x.y.z` is unsatisfiable due to version

---

_Issue opened by @pantheraleo-7 on 2024-10-20 06:39_

`pip` displays the available versions.

```
pip3 install pyspark==4.0.0
ERROR: Could not find a version that satisfies the requirement pyspark==4.0.0 (from versions: 2.1.2, 2.1.3, 2.2.0.post0, 2.2.1, 2.2.2, 2.2.3, 2.3.0, 2.3.1, 2.3.2, 2.3.3, 2.3.4, 2.4.0, 2.4.1, 2.4.2, 2.4.3, 2.4.4, 2.4.5, 2.4.6, 2.4.7, 2.4.8, 3.0.0, 3.0.1, 3.0.2, 3.0.3, 3.1.1, 3.1.2, 3.1.3, 3.2.0, 3.2.1, 3.2.2, 3.2.3, 3.2.4, 3.3.0, 3.3.1, 3.3.2, 3.3.3, 3.3.4, 3.4.0, 3.4.1, 3.4.2, 3.4.3, 3.5.0, 3.5.1, 3.5.2, 3.5.3, 4.0.0.dev1, 4.0.0.dev2)
```

`uv` just concludes that requirements are unsatisfiable

```
uv pip install pyspark==4.0.0
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of pyspark==4.0.0 and you require pyspark==4.0.0, we can
      conclude that your requirements are unsatisfiable.
```

this can be helpful, and may eliminate the need to visit the package's pypi page or gh releases.

---
