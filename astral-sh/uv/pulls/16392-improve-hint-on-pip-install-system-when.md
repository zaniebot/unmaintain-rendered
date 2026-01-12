```yaml
number: 16392
title: "Improve hint on `pip install --system` when externally managed"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - error messages
assignees: []
merged: true
base: main
head: zb/fix-system-hint
created_at: 2025-10-21T17:06:03Z
updated_at: 2025-10-21T17:59:02Z
url: https://github.com/astral-sh/uv/pull/16392
synced_at: 2026-01-12T16:12:14Z
```

# Improve hint on `pip install --system` when externally managed

---

_@zanieb_

I had some qualms with https://github.com/astral-sh/uv/pull/16318

1. The snapshot was specific to uv's managed interpreter due to inclusion of the externally-managed output. This will break downstream distros. We either need to filter the message, or, as done here, install a managed interpreter.
2. It had a custom filter for the interpreter path, which we shouldn't need. If needed, we should fix that in the test context.
3. We already had an implicit hint to create a virtual environment. The change in styling of the new hint hint following it is confusing. It's also confusing to hint creating a virtual environment when `--system` was used.
4. There were unresolved requested changes to the language for the message / it didn't fit stylistic with our existing ones.
5. The message was also very confusing for a uv-managed interpreter, which is both a "system" Python interpreter (in that it's global) and the opposite of a "system" interpreter per UV_PYTHON_PREFERENCE.

Also problematic, but not addressed here, is that there are other commands that display an externally-managed message, e.g., `uv pip sync`, but #16318 was just limited to `pip install`. We should not just implement this in one place — I'll open a tracking issue to consolidate and reuse the logic.

---

_Label `enhancement` added by @zanieb on 2025-10-21 17:12_

---

_Label `error messages` added by @zanieb on 2025-10-21 17:12_

---

_@konstin approved on 2025-10-21 17:40_

---

_Comment by @zanieb on 2025-10-21 17:48_

cc @cmnemoi maybe a few things to learn from here

---

_Merged by @zanieb on 2025-10-21 17:59_

---

_Closed by @zanieb on 2025-10-21 17:59_

---

_Branch deleted on 2025-10-21 17:59_

---
