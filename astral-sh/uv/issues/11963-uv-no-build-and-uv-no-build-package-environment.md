```yaml
number: 11963
title: "`UV_NO_BUILD` and `UV_NO_BUILD_PACKAGE` environment variables"
type: issue
state: closed
author: lengau
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2025-03-05T00:14:57Z
updated_at: 2025-03-05T04:58:20Z
url: https://github.com/astral-sh/uv/issues/11963
synced_at: 2026-01-10T03:50:31Z
```

# `UV_NO_BUILD` and `UV_NO_BUILD_PACKAGE` environment variables

---

_Issue opened by @lengau on 2025-03-05 00:14_

### Summary

Environment variables to allow specifying packages to only use the wheels of instead of building from source.

### Example

The following:

```
UV_NO_BUILD_PACKAGE='cryptography rpds-py' uv sync --no-binary
```

Would be equivalent to:

```
uv sync --no-binary --no-build-package cryptography --no-build-package rpds-py
```

---

_Label `enhancement` added by @lengau on 2025-03-05 00:14_

---

_Comment by @zanieb on 2025-03-05 00:16_

Loosely a duplicate of https://github.com/astral-sh/uv/issues/4291

---

_Label `configuration` added by @zanieb on 2025-03-05 00:16_

---

_Comment by @lengau on 2025-03-05 00:18_

Yep, very similar. The use cases I'm looking for are with `sync` and not with `uv pip`, but they're essentially the expansion of https://github.com/astral-sh/uv/pull/11399 for the `no-build` variants.

---

_Comment by @zanieb on 2025-03-05 00:20_

If you call those relationships out ahead of time you'll save me time :)

Sounds good though, these make sense to add.

---

_Comment by @lengau on 2025-03-05 01:59_

Sorry about that! I'll try to remember for the future.

---

_Closed by @zanieb on 2025-03-05 04:58_

---
