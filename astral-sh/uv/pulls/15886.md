```yaml
number: 15886
title: Infer check URL from publish URL when known
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/check-url
created_at: 2025-09-16T03:11:40Z
updated_at: 2025-09-16T14:12:34Z
url: https://github.com/astral-sh/uv/pull/15886
synced_at: 2026-01-10T06:36:15Z
```

# Infer check URL from publish URL when known

---

_Pull request opened by @charliermarsh on 2025-09-16 03:11_

## Summary

If we know the publish URL-to-check URL mapping, we can just infer it.


---

_Review requested from @zanieb by @charliermarsh on 2025-09-16 03:11_

---

_Review requested from @konstin by @charliermarsh on 2025-09-16 03:11_

---

_Label `enhancement` added by @charliermarsh on 2025-09-16 03:11_

---

_Marked ready for review by @charliermarsh on 2025-09-16 03:11_

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:477 on 2025-09-16 07:35_

```suggestion
    // Reconstruct the URL with `/simple/{workspace}/{registry}`.
```

---

_@konstin reviewed on 2025-09-16 07:37_

We need to update the docstring for `--check-url`, otherwise r+.

---

_@zanieb reviewed on 2025-09-16 13:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/publish.rs`:447 on 2025-09-16 13:50_

Shouldn't we also be able to infer the check URL for PyPI?

---

_@zanieb approved on 2025-09-16 13:51_

---

_@zanieb reviewed on 2025-09-16 13:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/publish.rs`:463 on 2025-09-16 13:53_

You could use `.filter` here if you prefer, like:

```suggestion
    let workspace = segments.next().filter(|segment| !segment.is_empty())?;
```

(I really don't have a preference)

---

_@zanieb reviewed on 2025-09-16 13:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/publish.rs`:472 on 2025-09-16 13:54_

A comment might be useful on this line

---

_Merged by @charliermarsh on 2025-09-16 14:03_

---

_Closed by @charliermarsh on 2025-09-16 14:03_

---

_Branch deleted on 2025-09-16 14:03_

---

_@charliermarsh reviewed on 2025-09-16 14:04_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/publish.rs`:447 on 2025-09-16 14:04_

There's a comment in the CLI that PyPI doesn't require this per @konstin.

---

_@konstin reviewed on 2025-09-16 14:12_

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:447 on 2025-09-16 14:12_

We could do it for PyPI too, it gives nicer and quicker errors.

---
