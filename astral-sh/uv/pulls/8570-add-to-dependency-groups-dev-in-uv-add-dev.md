```yaml
number: 8570
title: "Add to `dependency-groups.dev` in `uv add --dev`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: tracking/735
head: charlie/dev-add
created_at: 2024-10-25T17:00:47Z
updated_at: 2024-10-25T18:06:36Z
url: https://github.com/astral-sh/uv/pull/8570
synced_at: 2026-01-12T16:08:22Z
```

# Add to `dependency-groups.dev` in `uv add --dev`

---

_@charliermarsh_

## Summary

`uv add --dev` now updates the `dependency-groups.dev` section, rather than `tool.uv.dev-dependencies` -- unless the dependency is already present in `tool.uv.dev-dependencies`.

`uv remove --dev` now removes from both `dependency-groups.dev` and `tool.uv.dev-dependencies`.

`--dev` and `--group dev` are now treated equivalently in `uv add` and `uv remove`.

---

_Review requested from @zanieb by @charliermarsh on 2024-10-25 17:02_

---

_Comment by @charliermarsh on 2024-10-25 17:03_

I do wonder if `uv add --dev` should just add to `tool.uv.dev-dependencies` if it exists but `dependency-groups.dev` does not. I.e., consolidate rather than creating a separate table. But I think either is fine for now to be honest.

---

_Label `enhancement` added by @charliermarsh on 2024-10-25 17:03_

---

_Comment by @zanieb on 2024-10-25 17:05_

> I do wonder if uv add --dev should just add to tool.uv.dev-dependencies if it exists but dependency-groups.dev does not. I.e., consolidate rather than creating a separate table

Yeah I think this would be "best" in that it avoids introducing two separate tables. I think we'll get questions otherwise. I don't think it's critical, but unless it was a pain I'd probably do it.

---

_Comment by @charliermarsh on 2024-10-25 17:06_

Na it's easy. Will fix.

---

_Comment by @charliermarsh on 2024-10-25 17:07_

But should `uv add --group dev` add to `dev-dependencies` in that case? Or create a separate table?

---

_Comment by @zanieb on 2024-10-25 17:09_

I think it's fine either way, but I would be a little surprised if it didn't make a separate table.

---

_@zanieb approved on 2024-10-25 17:10_

---

_@T-256 reviewed on 2024-10-25 17:17_

---

_Review comment by @T-256 on `crates/uv/src/commands/project/remove.rs`:282 on 2024-10-25 17:17_

```suggestion
                    "`{name}` is a `{group}` group dependency; try calling `uv remove --group {group}`",
```
This looks better into my eyes :)

---

_Merged by @charliermarsh on 2024-10-25 18:06_

---

_Closed by @charliermarsh on 2024-10-25 18:06_

---

_Branch deleted on 2024-10-25 18:06_

---
