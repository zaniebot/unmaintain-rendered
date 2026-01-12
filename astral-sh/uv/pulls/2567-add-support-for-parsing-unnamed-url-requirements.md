```yaml
number: 2567
title: Add support for parsing unnamed URL requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/bare-i
created_at: 2024-03-20T15:54:08Z
updated_at: 2024-03-21T03:29:00Z
url: https://github.com/astral-sh/uv/pull/2567
synced_at: 2026-01-12T16:05:06Z
```

# Add support for parsing unnamed URL requirements

---

_@charliermarsh_

## Summary

First piece of https://github.com/astral-sh/uv/issues/313. In order to support unnamed requirements, we need to be able to parse them in `requirements-txt`, which in turn means that we need to introduce a new type that's distinct from `pep508::Requirement`, given that these _aren't_ PEP 508-compatible requirements.

Part of: https://github.com/astral-sh/uv/issues/313.


---

_@charliermarsh reviewed on 2024-03-20 17:16_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:1253 on 2024-03-20 17:16_

I really tried, but there are so many weirdnesses in the Windows version because it's a file URL, or something. I gave up with the filters.

---

_Marked ready for review by @charliermarsh on 2024-03-20 18:16_

---

_Review requested from @konstin by @charliermarsh on 2024-03-20 18:16_

---

_Review comment by @zanieb on `crates/pep508-rs/src/lib.rs`:1061 on 2024-03-20 19:19_

Just because we don't want to support this or is it hard for some reason?

---

_@zanieb reviewed on 2024-03-20 19:19_

---

_@zanieb reviewed on 2024-03-20 19:21_

---

_Review comment by @zanieb on `crates/requirements-txt/src/lib.rs`:1049 on 2024-03-20 19:21_

```suggestion
                    "Unnamed requirements are not allowed as constraints in `{}`",
```

---

_@zanieb approved on 2024-03-20 19:25_

---

_@charliermarsh reviewed on 2024-03-20 19:30_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:1061 on 2024-03-20 19:30_

Both... I'll expand in a comment here, it's a good question.

---

_Label `enhancement` added by @charliermarsh on 2024-03-20 23:38_

---

_Merged by @charliermarsh on 2024-03-21 03:28_

---

_Closed by @charliermarsh on 2024-03-21 03:28_

---

_Branch deleted on 2024-03-21 03:29_

---
