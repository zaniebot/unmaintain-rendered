```yaml
number: 12605
title: "Avoid unused async when context manager includes `TaskGroup`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tg
created_at: 2024-08-01T02:08:48Z
updated_at: 2024-08-01T12:43:54Z
url: https://github.com/astral-sh/ruff/pull/12605
synced_at: 2026-01-10T21:47:02Z
```

# Avoid unused async when context manager includes `TaskGroup`

---

_Pull request opened by @charliermarsh on 2024-08-01 02:08_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12354.


---

_Label `bug` added by @charliermarsh on 2024-08-01 02:08_

---

_Merged by @charliermarsh on 2024-08-01 02:12_

---

_Closed by @charliermarsh on 2024-08-01 02:12_

---

_Branch deleted on 2024-08-01 02:12_

---

_Comment by @github-actions[bot] on 2024-08-01 02:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@bluetech reviewed on 2024-08-01 11:03_

---

_Review comment by @bluetech on `crates/ruff_linter/src/rules/flake8_async/rules/cancel_scope_no_checkpoint.rs`:90 on 2024-08-01 11:03_

Shouldn't it hold for any async context manager? I don't think there is something unique about `asyncio.TaskGroup`.

If the `async with` is [explicitly nested](https://play.ruff.rs/351bf975-d24e-4826-a07a-9587670770db) then ruff doesn't trigger the error. So perhaps the fix should be to make

```py
async with asyncio.timeout(), foo():
    ...
```

be treated the same as

```py
async with asyncio.timeout():
    async with foo():
        ...
```

According to the [Language Reference](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement) they are semantically equivalent (this is for `with`, the [`async with` ref](https://docs.python.org/3/reference/compound_stmts.html#async-with) seems to have an omission here):

![image](https://github.com/user-attachments/assets/d9791c90-fa0a-43e9-a621-7c025782618e)


I also checked `async for` and it's detected correctly.

---

_@charliermarsh reviewed on 2024-08-01 12:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/rules/cancel_scope_no_checkpoint.rs`:90 on 2024-08-01 12:39_

I think you're right.

---

_@charliermarsh reviewed on 2024-08-01 12:43_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/rules/cancel_scope_no_checkpoint.rs`:90 on 2024-08-01 12:43_

Interested in changing it? :) We might need to accommodate cases like `async with asyncio.timeout(), asyncio.timeout()`, I guess those should still trigger even if weird.

---
