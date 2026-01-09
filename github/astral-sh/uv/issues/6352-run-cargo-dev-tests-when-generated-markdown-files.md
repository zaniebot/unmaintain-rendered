---
number: 6352
title: "Run `cargo dev` tests when generated Markdown files change"
type: issue
state: open
author: charliermarsh
labels:
  - testing
assignees: []
created_at: 2024-08-21T16:23:33Z
updated_at: 2024-08-21T16:46:31Z
url: https://github.com/astral-sh/uv/issues/6352
synced_at: 2026-01-07T13:12:17-06:00
---

# Run `cargo dev` tests when generated Markdown files change

---

_Issue opened by @charliermarsh on 2024-08-21 16:23_

See: https://github.com/astral-sh/uv/pull/6344#issuecomment-2302489633. If you change _just_ the generated file, we don't run tests.

---

_Label `testing` added by @charliermarsh on 2024-08-21 16:23_

---

_Referenced in [astral-sh/uv#6351](../../astral-sh/uv/pulls/6351.md) on 2024-08-21 16:23_

---

_Comment by @zanieb on 2024-08-21 16:29_

Oh.. tricky. We could just be more careful about not merging changes to generated files, but we can also run these tests separately.

---

_Comment by @charliermarsh on 2024-08-21 16:44_

We could also include the reference files in the "changed files" list, it should be harmless (but tedious to maintain).

---

_Comment by @zanieb on 2024-08-21 16:46_

True, not many reference files.

---

_Referenced in [astral-sh/uv#6551](../../astral-sh/uv/pulls/6551.md) on 2024-08-23 22:17_

---
