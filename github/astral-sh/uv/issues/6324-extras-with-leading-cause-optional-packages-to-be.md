---
number: 6324
title: "Extras with leading \"-\" cause optional packages to be installed."
type: issue
state: closed
author: AKuederle
labels:
  - bug
assignees: []
created_at: 2024-08-21T13:03:33Z
updated_at: 2024-08-21T13:47:35Z
url: https://github.com/astral-sh/uv/issues/6324
synced_at: 2026-01-07T13:12:17-06:00
---

# Extras with leading "-" cause optional packages to be installed.

---

_Issue opened by @AKuederle on 2024-08-21 13:03_

I tried installing one of my own packages with uv, which has, admittedly, a little weird structure when it comes to extras.

Namely it has two extras which are named with a leading "-". See excerpt from the package Metadata below.

When installing the package with uv (`uv add tpcp`), both packages (`torch` and `tensorflow-cpu`) that are part of these extras will be installed.
When using just pip or poetry to install the package, the two packages are correctly skipped.

Packages that are part of extras (`optuna` and `attrs`) that are part of extras without leading "-" are correctly skipped by uv

Relevant part from Package metadata:
```
Provides-Extra: -tensorflow
Provides-Extra: -torch-cpu
Provides-Extra: attrs
Provides-Extra: optuna
Requires-Dist: attrs (>=22.1.0) ; extra == "attrs"
Requires-Dist: joblib (>=1.3)
Requires-Dist: numpy (>=1.0)
Requires-Dist: optuna (>=2.10) ; extra == "optuna"
Requires-Dist: pandas (>=1.3)
Requires-Dist: scikit-learn (>=1.2.0)
Requires-Dist: tensorflow-cpu (>=2.16.0) ; extra == "-tensorflow"
Requires-Dist: torch (>=1.6.0) ; extra == "-torch-cpu"
Requires-Dist: tqdm (>=4.62.3)
Requires-Dist: typing-extensions (>=4.1.1)
```

Link to poetry managed pyproject.toml file use to create the package: https://github.com/mad-lab-fau/tpcp/blob/main/pyproject.toml

Based on the debug log during installation, it seems like uv simply assumes that torch and tensorflow-cpu are regular dependencies.

```
DEBUG Selecting: tpcp==1.0.0 [preference] (tpcp-1.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/42/0b/4256fb0025215fac17ef358a8e1b0fe092cb09a02e1bf4ef52bf40a1cb6f/tpcp-1.0.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for tpcp==1.0.0: joblib>=1.3
DEBUG Adding transitive dependency for tpcp==1.0.0: numpy>=1.0
DEBUG Adding transitive dependency for tpcp==1.0.0: pandas>=1.3
DEBUG Adding transitive dependency for tpcp==1.0.0: scikit-learn>=1.2.0
DEBUG Adding transitive dependency for tpcp==1.0.0: tensorflow-cpu>=2.16.0
DEBUG Adding transitive dependency for tpcp==1.0.0: torch>=1.6.0
DEBUG Adding transitive dependency for tpcp==1.0.0: tqdm>=4.62.3
DEBUG Adding transitive dependency for tpcp==1.0.0: typing-extensions>=4.1.1
```

Hence, I would assume it is an issue with parsing the Metadata file.

```
>>> uv --version
uv 0.3.0
```


---

_Comment by @charliermarsh on 2024-08-21 13:16_

I _think_ this is a bug, I'll take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-21 13:16_

---

_Label `bug` added by @charliermarsh on 2024-08-21 13:16_

---

_Comment by @AKuederle on 2024-08-21 13:31_

Just realized, that this is likely related: https://github.com/astral-sh/uv/issues/6279
Missed it when looking through the issues

---

_Comment by @charliermarsh on 2024-08-21 13:44_

Just confirming that this a bug on our end.

---

_Comment by @charliermarsh on 2024-08-21 13:47_

I'm going to merge into https://github.com/astral-sh/uv/issues/6279 to stop duplicating my comments.

---

_Closed by @charliermarsh on 2024-08-21 13:47_

---

_Referenced in [astral-sh/uv#6395](../../astral-sh/uv/pulls/6395.md) on 2024-08-23 10:30_

---
