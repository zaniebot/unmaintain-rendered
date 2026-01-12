```yaml
number: 10836
title: "fix(cli): conflict `--locked` with `--upgrade`"
type: pull_request
state: merged
author: mkniewallner
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/conflict-locked-upgrade
created_at: 2025-01-22T01:28:09Z
updated_at: 2025-02-07T18:04:27Z
url: https://github.com/astral-sh/uv/pull/10836
synced_at: 2026-01-12T16:09:31Z
```

# fix(cli): conflict `--locked` with `--upgrade`

---

_@mkniewallner_

## Summary

Relates to #10273.

This doesn't solve what is highlighted in https://github.com/astral-sh/uv/issues/10273#issuecomment-2569515066, but I believe this is still an improvement for users not setting `upgrade = true` in `[tool.uv]`.

## Test Plan

Ran commands locally:

```shell
$ cargo run --quiet -- lock --locked --upgrade
error: the argument '--check' cannot be used with '--upgrade'

Usage: uv lock --check

For more information, try '--help'.
```

---

_@charliermarsh approved on 2025-01-22 01:30_

---

_Marked ready for review by @mkniewallner on 2025-01-22 01:38_

---

_Merged by @zanieb on 2025-01-22 03:10_

---

_Closed by @zanieb on 2025-01-22 03:10_

---

_Label `bug` added by @zanieb on 2025-01-22 03:10_

---

_Branch deleted on 2025-01-22 03:11_

---

_Comment by @zanieb on 2025-01-29 14:22_

per https://github.com/astral-sh/uv/issues/1419#issuecomment-2621372373 people were using this to check if upgrades were available e.g. `uv lock --check --upgrade` — I think this is pretty reasonable. Should we revert?

---

_Comment by @zanieb on 2025-01-29 14:23_

Perhaps we need to do a better error message at the same time?

---

_Comment by @sayandipdutta on 2025-02-07 17:51_

> per [#1419 (comment)](https://github.com/astral-sh/uv/issues/1419#issuecomment-2621372373) people were using this to check if upgrades were available e.g. `uv lock --check --upgrade` — I think this is pretty reasonable. Should we revert?

Maybe I am misunderstanding here, but was that actually possible with this command? `uv lock --upgrade` seems to update the lockfile regardless whether any upgrades were available. I support the use case, in fact, I had the same use case and that is how I discovered this [issue](https://github.com/astral-sh/uv/issues/10273), but I don't think it ever worked that way?

![image](https://github.com/user-attachments/assets/3f7efce9-f822-450e-b0ca-4ec71a598762)


---
