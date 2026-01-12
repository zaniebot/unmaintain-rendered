```yaml
number: 16418
title: hope add the cache regenerate feature
type: issue
state: closed
author: smallhousebythelake
labels:
  - enhancement
assignees: []
created_at: 2025-10-23T11:11:53Z
updated_at: 2025-11-01T21:06:55Z
url: https://github.com/astral-sh/uv/issues/16418
synced_at: 2026-01-12T16:02:31Z
```

# hope add the cache regenerate feature

---

_@smallhousebythelake_

### Summary

after i commit 'uv cache clean' , the ~/.cache/uv is clean, but when i use 'uv tool install' something in the next time, i thought uv will redownload the same package which it have been used in some tools in ~/.local/share/uv/tools/ , and the good hard link functionality is invalid. i hope a new feature is add, that can regenerate cache base on the  tools or projects library in order to make the hard link functionality work again.

### Example

_No response_

---

_Label `enhancement` added by @smallhousebythelake on 2025-10-23 11:11_

---

_Comment by @charliermarsh on 2025-11-01 21:06_

I think we probably won't support this kind of thing (regenerating cache entries after cleaning), just because it seems to have limited usefulness and would be fairly complex. Thank you for writing in though.

---

_Closed by @charliermarsh on 2025-11-01 21:06_

---
