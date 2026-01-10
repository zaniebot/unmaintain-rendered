```yaml
number: 15397
title: Unideal error message on recursive dependency groups (depending on command)
type: issue
state: closed
author: henryiii
labels:
  - error messages
assignees: []
created_at: 2025-08-20T21:15:41Z
updated_at: 2025-08-22T14:30:34Z
url: https://github.com/astral-sh/uv/issues/15397
synced_at: 2026-01-10T03:23:54Z
```

# Unideal error message on recursive dependency groups (depending on command)

---

_Issue opened by @henryiii on 2025-08-20 21:15_

Not sure if this is exactly a bug, per se, just an error message that's not really helpful.

I noticed that `uv pip install --system -e.[schema] --group test` produced an unhelpful error: 
```
error: Failed to read dependency groups from: /home/runner/work/uhi/uhi/pyproject.toml
Project `uhi` has malformed dependency groups
```
But when I tried `uv run pytest` locally, I got a far more helpful error:

```
error: Project `uhi` has malformed dependency groups
  Caused by: Detected a cycle in `dependency-groups`: `dev` -> `test` -> `dev`
```
That was my mistake, I copy-pasted and forgot to change "test" into "test-core", so test depended on itself. (I think the message should have been `` `dev` -> `test` -> `test` ``, but at least it was better)

My pyproject.toml has this in dependency-groups:

```toml
test = [
  { include-group = "test" },
  "h5py; platform_python_implementation == 'CPython'",  # Doesn't support free-threaded Python currently
]
dev = [{ include-group = "test" }]
```

---

_Comment by @zanieb on 2025-08-20 23:02_

Thanks!

cc @Gankra maybe this will be obvious to you

---

_Label `error messages` added by @zanieb on 2025-08-20 23:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-22 14:03_

---

_Closed by @charliermarsh on 2025-08-22 14:30_

---
