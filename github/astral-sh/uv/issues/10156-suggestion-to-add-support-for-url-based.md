---
number: 10156
title: Suggestion to add support for URL-based requirement specifiers
type: issue
state: closed
author: oliversen
labels: []
assignees: []
created_at: 2024-12-25T16:58:53Z
updated_at: 2024-12-29T00:35:14Z
url: https://github.com/astral-sh/uv/issues/10156
synced_at: 2026-01-07T13:12:18-06:00
---

# Suggestion to add support for URL-based requirement specifiers

---

_Issue opened by @oliversen on 2024-12-25 16:58_

Example:
```
pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686
```

---

_Comment by @charliermarsh on 2024-12-26 14:34_

I believe this already works:

```
❯ uv pip install "pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686"
Resolved 1 package in 2.48s
   Built pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686
Prepared 1 package in 372ms
Installed 1 package in 2ms
 + pip==1.3.1 (from https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686)
```

---

_Closed by @charliermarsh on 2024-12-26 14:34_

---

_Comment by @oliversen on 2024-12-28 15:54_

Sorry, I didn't clarify. It doesn't work if the package version is specified:

```shell
uv pip install "pip==1.3.1 @ https://github.com/pypa/pip/archive/1.3.1.zip"
error: Failed to parse: `pip==1.3.1 @ https://github.com/pypa/pip/archive/1.3.1.zip`
  Caused by: Trailing `@ https://github.com/pypa/pip/archive/1.3.1.zip` is not allowed
pip==1.3.1 @ https://github.com/pypa/pip/archive/1.3.1.zip
   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

---

_Comment by @charliermarsh on 2024-12-29 00:35_

This syntax isn't specific to uv. It's part of a Python standard: https://peps.python.org/pep-0508/. And the standard doesn't allow specifiers in that position.

---
