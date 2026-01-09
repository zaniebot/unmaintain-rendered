---
number: 1414
title: Trying to install a wheel that is a local file fails
type: issue
state: closed
author: CoolCat467
labels: []
assignees: []
created_at: 2024-02-16T02:32:13Z
updated_at: 2024-02-16T02:35:52Z
url: https://github.com/astral-sh/uv/issues/1414
synced_at: 2026-01-07T13:12:16-06:00
---

# Trying to install a wheel that is a local file fails

---

_Issue opened by @CoolCat467 on 2024-02-16 02:32_

I think this might be a duplicate but I'm not sure, but trying to `uv pip install` a wheel that is a local file fails because uv seems to think it should be a URL.

```console
+ uv pip install dist/trio-0.24.0+dev-py3-none-any.whl
error: Failed to parse `dist/trio-0.24.0+dev-py3-none-any.whl`
  Caused by: URL requirement must be preceded by a package name. Add the name of the package before the URL (e.g., `package_name @ https://...`).
dist/trio-0.24.0+dev-py3-none-any.whl
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

---

_Comment by @CoolCat467 on 2024-02-16 02:35_

Ok looking closer, this is duplicating #1403

---

_Closed by @CoolCat467 on 2024-02-16 02:35_

---
