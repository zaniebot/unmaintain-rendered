```yaml
number: 3120
title: TCH002 autofix
type: issue
state: closed
author: Olegt0rr
labels: []
assignees: []
created_at: 2023-02-22T12:16:55Z
updated_at: 2023-02-23T13:14:20Z
url: https://github.com/astral-sh/ruff/issues/3120
synced_at: 2026-01-10T11:09:46Z
```

# TCH002 autofix

---

_Issue opened by @Olegt0rr on 2023-02-22 12:16_

It would be great to implement the ability to automatically fix the TCH002 error so that the import is automatically placed in the TYPE_CHECKING block.

It's so boring to fix it manually:
1. Ensure that `TYPE_CHECKING` imported from `typing`
2. Ensure that block `if TYPE_CHECKING:` exists
3. Move import to the block
4. Wrap types with quotes


---

_Comment by @charliermarsh on 2023-02-22 19:31_

Agreed. Just gonna close this as a duplicate of #2329.

---

_Closed by @charliermarsh on 2023-02-22 19:31_

---

_Comment by @Olegt0rr on 2023-02-23 13:12_

Oh, thanks!
It's would be nice to add rule codes to issues to make it more searchable

---
