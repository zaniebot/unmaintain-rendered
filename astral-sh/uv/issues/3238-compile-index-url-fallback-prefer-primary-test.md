```yaml
number: 3238
title: "`compile_index_url_fallback_prefer_primary` test doesn't seem to test anything"
type: issue
state: closed
author: mgorny
labels:
  - testing
assignees: []
created_at: 2024-04-24T14:09:52Z
updated_at: 2024-04-25T23:30:45Z
url: https://github.com/astral-sh/uv/issues/3238
synced_at: 2026-01-10T05:31:37Z
```

# `compile_index_url_fallback_prefer_primary` test doesn't seem to test anything

---

_Issue opened by @mgorny on 2024-04-24 14:09_

The following test:

https://github.com/astral-sh/uv/blob/116d47ed03695c5fb2ce8084663ff79bc87b6b54/crates/uv/tests/pip_compile.rs#L7808-L7844

indicates that it relies on the "extra" index having 3.1.2 as the newest version. However, looking at https://download.pytorch.org/whl/jinja2/, the index includes 3.1.3 as well, so the test doesn't really test anything.

I suppose it would be better to actually use a index that you control here.

---

_Label `testing` added by @zanieb on 2024-04-24 14:41_

---

_Comment by @zanieb on 2024-04-24 14:41_

Thanks!

---

_Comment by @charliermarsh on 2024-04-25 23:21_

My guess is that the index version was updated.

---

_Comment by @zanieb on 2024-04-25 23:25_

Are we not using exclude newer here?

---

_Comment by @zanieb on 2024-04-25 23:25_

Huh we explicitly turn that off, seems unnecessary.

---

_Comment by @charliermarsh on 2024-04-25 23:26_

No, it's intentional -- we can't use `--exclude-newer` with the PyTorch index.

---

_Comment by @zanieb on 2024-04-25 23:26_

Ah because they don't have timestamps? 😭 

---

_Comment by @charliermarsh on 2024-04-25 23:26_

But the solution is to not use the PyTorch index :joy:

---

_Closed by @charliermarsh on 2024-04-25 23:30_

---
