```yaml
number: 13325
title: uv leaks private tokens
type: issue
state: closed
author: ChrisBr
labels:
  - bug
assignees: []
created_at: 2025-05-07T09:11:36Z
updated_at: 2025-05-07T14:29:17Z
url: https://github.com/astral-sh/uv/issues/13325
synced_at: 2026-01-12T16:01:25Z
```

# uv leaks private tokens

---

_@ChrisBr_

### Summary

When uv fails, it will leak the private registry token to STDOUT which is problematic e.g. on CI.  

```
> uv sync --inexact --frozen --no-dev --no-cache
--
  | hint: `PACKAGE` was found on https://token:SECRET_TOKEN_LEAKED@private-registry.io/basic/data/python/simple/, but not at the requested version
  | (PACKAGE>=2.2.0,<3.0.0). A compatible version may be available on a subsequent index (e.g., https://pypi.python.org/simple). By default, uv will only
  | consider versions that are published on the first index that contains a given package, to avoid dependency confusion attacks. If all indexes are equally
  | trusted, use `--index-strategy unsafe-best-match` to consider all versions from all indexes, regardless of the order in which they were defined.
```

I would expect that `https://token:SECRET_TOKEN_LEAKED@private-registry.io/basic/data/python/simple/` is `https://token:****@private-registry.io/basic/data/python/simple/` instead.



### Platform

Linux

### Version

uv-0.6.14

### Python version

3.10.14

---

_Label `bug` added by @ChrisBr on 2025-05-07 09:11_

---

_Assigned to @jtfmumm by @konstin on 2025-05-07 09:45_

---

_Comment by @jtfmumm on 2025-05-07 10:03_

Thanks for opening this. I plan to address this soon.

---

_Comment by @charliermarsh on 2025-05-07 12:49_

@jtfmumm -- Separately from obfuscation, I think we should just call `.redact` on the URLs when we format them for these hints. That should be more straightforward and solves the immediate issue.

---

_Comment by @zanieb on 2025-05-07 14:29_

This is a duplicate of https://github.com/astral-sh/uv/issues/1714

---

_Closed by @zanieb on 2025-05-07 14:29_

---
