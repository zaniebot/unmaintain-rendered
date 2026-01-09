---
number: 13845
title: Termux support
type: issue
state: closed
author: geomlattice
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-06-04T17:48:31Z
updated_at: 2025-06-04T18:41:08Z
url: https://github.com/astral-sh/uv/issues/13845
synced_at: 2026-01-07T13:12:18-06:00
---

# Termux support

---

_Issue opened by @geomlattice on 2025-06-04 17:48_

### Summary

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
ERROR: there isn't a download for your platform aarch64-linux-android
```

### Example

_No response_

---

_Label `enhancement` added by @geomlattice on 2025-06-04 17:48_

---

_Comment by @geomlattice on 2025-06-04 17:51_

```bash
pip install uv
```
also fails, seems like maturin won't build or something?
```
 × Building wheel for maturin (pyproject.toml) did not run successfully.
        │ exit code: 1

```

---

_Comment by @zanieb on 2025-06-04 18:29_

Note there are other issues for Termux https://github.com/astral-sh/uv/issues?q=is%3Aissue%20%20termux

---

_Comment by @zanieb on 2025-06-04 18:29_

I think this is a duplicate of https://github.com/astral-sh/uv/issues/2408

---

_Label `duplicate` added by @zanieb on 2025-06-04 18:30_

---

_Comment by @geomlattice on 2025-06-04 18:36_

This looks helpful 

https://github.com/astral-sh/uv/issues/7373#issuecomment-2350912448

---

_Comment by @geomlattice on 2025-06-04 18:41_

Seems to work

```bash
pkg install uv
```

---

_Closed by @geomlattice on 2025-06-04 18:41_

---
