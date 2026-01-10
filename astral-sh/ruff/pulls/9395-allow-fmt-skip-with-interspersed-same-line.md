```yaml
number: 9395
title: "Allow `# fmt: skip` with interspersed same-line comments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/skip
created_at: 2024-01-05T00:17:51Z
updated_at: 2024-01-10T12:21:16Z
url: https://github.com/astral-sh/ruff/pull/9395
synced_at: 2026-01-10T22:57:09Z
```

# Allow `# fmt: skip` with interspersed same-line comments

---

_Pull request opened by @charliermarsh on 2024-01-05 00:17_

## Summary

This is similar to https://github.com/astral-sh/ruff/pull/8876, but more limited in scope:

1. It only applies to `# fmt: skip` (like Black). Like `# isort: on`, `# fmt: on` needs to be on its own line (still).
2. It only delimits on `#`, so you can do `# fmt: skip # noqa`, but not `# fmt: skip - some other content` or `# fmt: skip; noqa`.

If we want to support the `;`-delimited version, we should revisit later, since we don't support that in the linter (so `# fmt: skip; noqa` wouldn't register a `noqa`).

Closes https://github.com/astral-sh/ruff/issues/8892.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-05 00:18_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-05 00:18_

---

_Marked ready for review by @charliermarsh on 2024-01-05 00:18_

---

_Label `formatter` added by @charliermarsh on 2024-01-05 00:18_

---

_Comment by @github-actions[bot] on 2024-01-05 00:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@zanieb approved on 2024-01-05 00:39_

---

_Merged by @charliermarsh on 2024-01-05 00:39_

---

_Closed by @charliermarsh on 2024-01-05 00:39_

---

_Branch deleted on 2024-01-05 00:39_

---
