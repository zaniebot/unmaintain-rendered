---
number: 12007
title: Allow empty dependency groups
type: issue
state: closed
author: jneuendorf-i4h
labels:
  - bug
  - enhancement
assignees: []
created_at: 2025-03-06T14:18:51Z
updated_at: 2025-03-07T10:00:35Z
url: https://github.com/astral-sh/uv/issues/12007
synced_at: 2026-01-10T01:25:14Z
---

# Allow empty dependency groups

---

_Issue opened by @jneuendorf-i4h on 2025-03-06 14:18_

### Summary

### Description

For me, it would be nice if running `uv sync --group build` with

```toml
# [project]
# ...

[dependency-groups]
build = []
```

does not result in this error

```
error: Group `build` is not defined in the project's `dependency-groups` table
```

but rather be a noop (maybe printing a warning). PEP 735 doesn't seem to prohibit the use of empty dependency groups.

### Use case

My use case is that I have multiple services that should be interchangeable. I build them using the same Dockerfile so the build process should be ideally identical. Some services require some additional packages and some don't. 

So it would be nice if I could just install the group that potentially contains the extra deps without worrying about errors.


### Versions

- uv 0.6.4 (Homebrew 2025-03-03)
- macOS 15.3.1

### Example

_No response_

---

_Label `enhancement` added by @jneuendorf-i4h on 2025-03-06 14:18_

---

_Comment by @charliermarsh on 2025-03-06 14:38_

Ah this is supposed to work.

---

_Comment by @charliermarsh on 2025-03-06 17:07_

Does it work if you recreate the lockfile? Or invalidate it in some way? (E.g., try adding some package to `build = []`, then locking, then removing it, then locking again.) I think we're just not catching that the lockfile needs to be invalidated.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-06 17:16_

---

_Label `bug` added by @charliermarsh on 2025-03-06 17:16_

---

_Referenced in [astral-sh/uv#12010](../../astral-sh/uv/pulls/12010.md) on 2025-03-06 17:33_

---

_Closed by @charliermarsh on 2025-03-06 17:44_

---

_Comment by @jneuendorf-i4h on 2025-03-07 10:00_

> Does it work if you recreate the lockfile? Or invalidate it in some way? (E.g., try adding some package to `build = []`, then locking, then removing it, then locking again.) I think we're just not catching that the lockfile needs to be invalidated.

Even though, you addressed this super quickly, I could confirm that re-locking as you suggested made it work 👍 

---
