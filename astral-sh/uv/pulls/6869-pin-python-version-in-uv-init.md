```yaml
number: 6869
title: "Pin `.python-version` in `uv init`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pin
created_at: 2024-08-30T13:51:32Z
updated_at: 2024-09-03T23:43:51Z
url: https://github.com/astral-sh/uv/pull/6869
synced_at: 2026-01-12T16:07:34Z
```

# Pin `.python-version` in `uv init`

---

_@charliermarsh_

## Summary

I'm not convinced that the behavior is correct as-implemented. When the user passes a `--python >=3.8` or we discover a `requires-python` from the workspace, we're currently writing that request out to `.python-version`. I would probably rather that we write the resolved patch version?

Closes https://github.com/astral-sh/uv/issues/6821.


---

_Label `enhancement` added by @charliermarsh on 2024-08-30 13:51_

---

_Marked ready for review by @charliermarsh on 2024-08-30 13:51_

---

_@charliermarsh reviewed on 2024-08-30 13:52_

---

_Review comment by @charliermarsh on `crates/uv/tests/init.rs`:1753 on 2024-08-30 13:52_

Like this is strange I think.

---

_Comment by @charliermarsh on 2024-08-30 13:55_

I think I'd prefer that we resolve the version request, and write the major, minor, patch to `.python-version`. But need feedback.

---

_@samypr100 reviewed on 2024-08-30 16:30_

---

_Review comment by @samypr100 on `crates/uv/tests/init.rs`:1753 on 2024-08-30 16:30_

Indeed

---

_Comment by @zanieb on 2024-09-03 13:57_

Yes I think it'd be weird to echo the request — as that doesn't really have an effect in a project it just matches the constraint we infer from the `requires-python` definition. Let's resolve and write major / minor as you suggested.

I think we also need an opt-out `--no-pin-python` flag for this.

---

_Comment by @charliermarsh on 2024-09-03 13:57_

Sg.

---

_Comment by @charliermarsh on 2024-09-03 13:57_

Not patch?

---

_Comment by @zanieb on 2024-09-03 17:23_

Yeah I don't think we should pin to a patch version by default, since they're expected to have the same behavior and frequently vary in availability.

---

_Comment by @samypr100 on 2024-09-03 17:30_

> Yeah I don't think we should pin to a patch version by default, since they're expected to have the same behavior and frequently vary in availability.

I think that's quite normal, a lot of my .python-version files have just `3.12` these days

---

_Review requested from @zanieb by @charliermarsh on 2024-09-03 23:30_

---

_Comment by @charliermarsh on 2024-09-03 23:30_

Added `--no-pin-python`, switched to minor version.

---

_@zanieb reviewed on 2024-09-03 23:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:227 on 2024-09-03 23:41_

Not blocking, but do we do interpreter discovery twice in this case? Seems less than ideal if so.

---

_@zanieb reviewed on 2024-09-03 23:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:227 on 2024-09-03 23:41_

Nevermind doesn't look like it. Hard to see in the diff.

---

_@zanieb approved on 2024-09-03 23:42_

---

_Merged by @charliermarsh on 2024-09-03 23:43_

---

_Closed by @charliermarsh on 2024-09-03 23:43_

---

_Branch deleted on 2024-09-03 23:43_

---
