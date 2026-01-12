```yaml
number: 1948
title: Restrict SIM105 to try blocks with a body of one simple statement
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: restrict-sim105
created_at: 2023-01-18T05:19:14Z
updated_at: 2023-02-14T06:18:03Z
url: https://github.com/astral-sh/ruff/pull/1948
synced_at: 2026-01-12T15:55:07Z
```

# Restrict SIM105 to try blocks with a body of one simple statement

---

_@andersk_

If a `try` block has multiple statements, a compound statement, or control flow, rewriting it with `contextlib.suppress` would obfuscate the fact that the exception still short-circuits further statements in the block.

Fixes #1947.

---

_Merged by @charliermarsh on 2023-01-18 05:22_

---

_Closed by @charliermarsh on 2023-01-18 05:22_

---

_Branch deleted on 2023-01-18 05:23_

---

_Comment by @not-my-profile on 2023-02-14 06:10_

Could you give an example of how `contextlib.suppress` would obfuscate this?

---

_Comment by @andersk on 2023-02-14 06:15_

I gave an example in #1947.

```python
for i in range(1, 100):
    try:
        realm.string_id = prefix + str(i)
        realm.save(update_fields=["string_id"])
        break
    except IntegrityError:
        pass
else:
    raise RuntimeError(f"Unable to find a good string_id for realm {realm}")
```

---

_Comment by @not-my-profile on 2023-02-14 06:18_

Ah, my bad I didn't see the issue. Thanks.

---
