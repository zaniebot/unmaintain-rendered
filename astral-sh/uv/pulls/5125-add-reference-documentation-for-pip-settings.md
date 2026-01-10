```yaml
number: 5125
title: Add reference documentation for pip settings
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/api-reference-iii
created_at: 2024-07-16T19:41:14Z
updated_at: 2024-07-16T21:14:28Z
url: https://github.com/astral-sh/uv/pull/5125
synced_at: 2026-01-10T13:42:52Z
```

# Add reference documentation for pip settings

---

_Pull request opened by @charliermarsh on 2024-07-16 19:41_

## Summary

Third part of https://github.com/astral-sh/uv/issues/5093.


---

_Marked ready for review by @charliermarsh on 2024-07-16 19:41_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-16 19:41_

---

_Label `documentation` added by @charliermarsh on 2024-07-16 19:41_

---

_@zanieb reviewed on 2024-07-16 20:15_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:464 on 2024-07-16 20:15_

This is far from comprehensive, just noting for #4400 as I don't think it needs to be addressed here.

---

_@zanieb reviewed on 2024-07-16 20:15_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:472 on 2024-07-16 20:15_

"system Python environment"?

---

_@zanieb reviewed on 2024-07-16 20:16_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:503 on 2024-07-16 20:16_

I'd say "virtual or system Python environment". No strong feelings, but want to have some consistency around language for installing with system interpreters.

or just "rather than into an environment"

---

_@zanieb reviewed on 2024-07-16 20:19_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:514 on 2024-07-16 20:19_

We don't copy in an interpreter though, right? Should we say... "as if a virtual environment were present at the specified location"?

---

_Comment by @zanieb on 2024-07-16 20:34_

I only got part way through here then I think I started reviewing some duplicate content from the stack — I'll look again soon.

---

_Review requested from @zanieb by @charliermarsh on 2024-07-16 20:46_

---

_Comment by @charliermarsh on 2024-07-16 20:48_

Thanks -- rebased and updated to reflect feedback on the previous PRs. Note that this is almost all copied from the CLI with minor modifications.

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:985 on 2024-07-16 20:52_

```suggestion
    /// By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`), instead
```

---

_@zanieb reviewed on 2024-07-16 20:52_

---

_@zanieb reviewed on 2024-07-16 20:53_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:986 on 2024-07-16 20:53_

Perhaps "compilation will be performed lazily the first time a module is imported"

---

_@zanieb reviewed on 2024-07-16 20:53_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:987 on 2024-07-16 20:53_

```suggestion
    /// first start time matters, such as CLI applications and Docker containers, this option can
```

---

_@zanieb reviewed on 2024-07-16 20:54_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:990 on 2024-07-16 20:54_

"including packages that are not being installed"? Not obvious otherwise.

---

_@zanieb reviewed on 2024-07-16 20:54_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:1010 on 2024-07-16 20:54_

Some markdown renderers are picky about a blank line here.

---

_@charliermarsh reviewed on 2024-07-16 20:55_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:1010 on 2024-07-16 20:55_

Ah yeah good call. I think MkDocs is picky about that, actually.

---

_@zanieb approved on 2024-07-16 20:55_

---

_@zanieb reviewed on 2024-07-16 20:56_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:528 on 2024-07-16 20:56_

Does this URL format work well in mkdocs? (Sorry for not checking)

---

_@charliermarsh reviewed on 2024-07-16 21:06_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:528 on 2024-07-16 21:06_

Turns out it does.

---

_Merged by @charliermarsh on 2024-07-16 21:14_

---

_Closed by @charliermarsh on 2024-07-16 21:14_

---

_Branch deleted on 2024-07-16 21:14_

---
