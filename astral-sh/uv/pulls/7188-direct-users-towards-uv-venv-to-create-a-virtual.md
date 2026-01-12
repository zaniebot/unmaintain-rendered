```yaml
number: 7188
title: "Direct users towards `uv venv` to create a virtual environment"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/e
created_at: 2024-09-08T14:44:45Z
updated_at: 2024-09-08T22:33:35Z
url: https://github.com/astral-sh/uv/pull/7188
synced_at: 2026-01-12T16:07:43Z
```

# Direct users towards `uv venv` to create a virtual environment

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/7123.


---

_Review requested from @zanieb by @charliermarsh on 2024-09-08 14:45_

---

_Label `error messages` added by @charliermarsh on 2024-09-08 14:45_

---

_Comment by @zanieb on 2024-09-08 14:47_

Awesome thanks. Been on my list for a while.

We should have at least some test coverage for this.

---

_@zanieb reviewed on 2024-09-08 15:07_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:68 on 2024-09-08 15:07_

Could probably also have test coverage for the case `--system` with no Python installations, `TexrContext::new_with_versions([])` should be get you the proper setup.

---

_@charliermarsh reviewed on 2024-09-08 16:11_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:68 on 2024-09-08 16:11_

Done!

---

_Review requested from @zanieb by @charliermarsh on 2024-09-08 16:11_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:88 on 2024-09-08 17:08_

So there's a problem here, in that if you install with `uv python install` we actually use an externally managed marker and won't let you install into it with `--system` :)


---

_@zanieb reviewed on 2024-09-08 17:08_

---

_@charliermarsh reviewed on 2024-09-08 17:31_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:88 on 2024-09-08 17:31_

Should I just remove this part then?

---

_@zanieb reviewed on 2024-09-08 17:35_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:88 on 2024-09-08 17:35_

Maybe yeah. Is this error only surfaced in `uv pip`? If it's surfaced in the top-level commands this _would_ be relevant. I feel like we say something else there though? You might want to say "No system Python installation found" instead of this since people don't really know what a "system environment" is?

---

_@charliermarsh reviewed on 2024-09-08 22:08_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:88 on 2024-09-08 22:08_

Done. It's only in `uv pip` yeah.

---

_Review requested from @zanieb by @charliermarsh on 2024-09-08 22:08_

---

_@zanieb approved on 2024-09-08 22:12_

---

_Merged by @charliermarsh on 2024-09-08 22:33_

---

_Closed by @charliermarsh on 2024-09-08 22:33_

---

_Branch deleted on 2024-09-08 22:33_

---
