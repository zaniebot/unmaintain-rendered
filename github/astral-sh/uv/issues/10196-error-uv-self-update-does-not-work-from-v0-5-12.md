---
number: 10196
title: "[error] `uv self update` does not work from `v0.5.12` to `v0.5.13`"
type: issue
state: closed
author: lamteteeow
labels:
  - bug
assignees: []
created_at: 2024-12-27T15:31:25Z
updated_at: 2024-12-27T20:35:23Z
url: https://github.com/astral-sh/uv/issues/10196
synced_at: 2026-01-07T13:12:18-06:00
---

# [error] `uv self update` does not work from `v0.5.12` to `v0.5.13`

---

_Issue opened by @lamteteeow on 2024-12-27 15:31_

```
$ uv self update
error: error decoding response body
  Caused by: there are extra bytes after body has been decompressed
```

I am on version: `uv 0.5.12 (351d602d8 2024-12-26)`
Windows 11 23H2
Update from `v0.5.11` to `v0.5.12` went fine.

---

_Comment by @jdmartin on 2024-12-27 15:37_

Having this same issue on macOS.  I am currently using `uv 0.5.12 (351d602d8 2024-12-26)`.

```
uv self update
info: Checking for updates...
error: error decoding response body
  Caused by: there are extra bytes after body has been decompressed
```

---

_Comment by @anotherbugmaster on 2024-12-27 15:43_

Same, but with `uv pip install` on [Databricks Runtime 15.4 LTS](https://docs.databricks.com/en/release-notes/runtime/15.4lts.html#system-environment)

```
+ /databricks/python/bin/uv pip install --no-cache --no-deps -r /tmp/requirements.txt
Using Python 3.11.0rc1 environment at: /databricks/python3
error: Failed to fetch: `https://<private_domain>/pypi/<repo_name>/simple/<package_name>/`
  Caused by: error decoding response body
  Caused by: there are extra bytes after body has been decompressed
```

Downgrading to `uv==0.5.11` has resolved the issue

---

_Comment by @FishAlchemist on 2024-12-27 16:35_

Hello everyone. The inability to use ``self-update`` in version 0.5.12 is a known bug. Actually, it was mentioned and fixed in https://github.com/astral-sh/uv/issues/10191.
The issues you(@lamteteeow @jdmartin  @anotherbugmaster ) mentioned should have been fixed in version 0.5.13.

---

_Comment by @charliermarsh on 2024-12-27 16:36_

👍 This was a known issue in v0.5.12 due to a `reqwests` update. Sorry about that. I suggest re-installing with cURL (e.g., `curl -LsSf https://astral.sh/uv/install.sh | sh`). Subsequent self-updates should work without issue.

---

_Label `bug` added by @charliermarsh on 2024-12-27 16:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-27 16:36_

---

_Closed by @charliermarsh on 2024-12-27 16:36_

---

_Comment by @FishAlchemist on 2024-12-27 16:42_

@charliermarsh Given that version 0.5.12 is unable to network, would you like to proceed with yanking it from PyPI?

---

_Referenced in [astral-sh/uv#8015](../../astral-sh/uv/issues/8015.md) on 2024-12-27 17:19_

---

_Comment by @lamteteeow on 2024-12-27 20:35_

> 👍 This was a known issue in v0.5.12 due to a `reqwests` update. Sorry about that. I suggest re-installing with cURL (e.g., `curl -LsSf https://astral.sh/uv/install.sh | sh`). Subsequent self-updates should work without issue.

That solved the issue quickly! Thank you for being swift and engaged.

---

_Referenced in [astral-sh/uv#10213](../../astral-sh/uv/issues/10213.md) on 2024-12-28 09:34_

---

_Referenced in [astral-sh/uv#10295](../../astral-sh/uv/issues/10295.md) on 2025-01-04 07:24_

---
