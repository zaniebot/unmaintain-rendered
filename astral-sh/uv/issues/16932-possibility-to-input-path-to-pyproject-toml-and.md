---
number: 16932
title: Possibility to input path to pyproject.toml and uv.lock
type: issue
state: closed
author: korolenkowork
labels:
  - enhancement
assignees: []
created_at: 2025-12-02T14:51:08Z
updated_at: 2025-12-02T18:56:13Z
url: https://github.com/astral-sh/uv/issues/16932
synced_at: 2026-01-10T01:26:11Z
---

# Possibility to input path to pyproject.toml and uv.lock

---

_Issue opened by @korolenkowork on 2025-12-02 14:51_

### Summary

I had a few components in one repo that have their own dependencies and build independently. I'd like to have the possibility to choose which pyproject.toml and uv.lock I want to interact with.

### Example

_No response_

---

_Label `enhancement` added by @korolenkowork on 2025-12-02 14:51_

---

_Comment by @zanieb on 2025-12-02 14:55_

Can you share an example directory tree?

---

_Comment by @korolenkowork on 2025-12-02 18:46_

Example:
```
├───src
│   └───...
├───watcher
│   └───...
├───transcriber
│   └───...
├───shared-lib
│   └───...
├───manage.py  
│  
├───pyproject.core.toml
│   
├───pyproject.watcher.toml
│  
├───pyproject.transcriber.toml
│   
├───uv.core.lock
│   
├───uv.watcher.lock
│  
├───uv.transcriber.lock
│  
└───...
```

    


---

_Comment by @zanieb on 2025-12-02 18:53_

I don't think we'll support non-standard names for `pyproject.toml` files (nor do we intend to support arbitrary names for `uv.lock` files). Why don't you use subdirectories for those projects or place the project files in their respective directories?

---

_Closed by @korolenkowork on 2025-12-02 18:56_

---
