```yaml
number: 2820
title: Deduplicate editables during install commands
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dupe
created_at: 2024-04-04T16:46:18Z
updated_at: 2024-04-04T18:47:05Z
url: https://github.com/astral-sh/uv/pull/2820
synced_at: 2026-01-12T16:05:14Z
```

# Deduplicate editables during install commands

---

_@charliermarsh_

## Summary

Closes #2819.


---

_Label `bug` added by @charliermarsh on 2024-04-04 16:46_

---

_Marked ready for review by @charliermarsh on 2024-04-04 16:46_

---

_@zanieb reviewed on 2024-04-04 16:48_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:3107 on 2024-04-04 16:48_

Can you add test coverage for an absolute path that resolves to the same as a relative path?

Should we add handling for symbolic links?

---

_@charliermarsh reviewed on 2024-04-04 16:52_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:3107 on 2024-04-04 16:52_

I'd prefer not to merge symlinks here, but I can add an absolute path test, yes.

---

_Review requested from @zanieb by @charliermarsh on 2024-04-04 17:09_

---

_@charliermarsh reviewed on 2024-04-04 17:11_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:3107 on 2024-04-04 17:11_

I mean, we _could_ merge symlinks, it's not difficult, I'm just not sure if it's correct. I guess in theory they _must_ resolve to the same package, so maybe it's totally fine...

---

_@zanieb approved on 2024-04-04 17:12_

---

_Merged by @charliermarsh on 2024-04-04 17:19_

---

_Closed by @charliermarsh on 2024-04-04 17:19_

---

_Branch deleted on 2024-04-04 17:19_

---

_Comment by @jaap3 on 2024-04-04 18:47_

Absolutely sublime🥇. Thanks for thinking about extra's as well. That's actually something we use and I forgot to mention it in the initial report.

---
