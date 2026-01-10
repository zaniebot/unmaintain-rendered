```yaml
number: 8543
title: "Add `tool.uv.sources` to the \"Settings\" reference"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-10-24T20:42:30Z
updated_at: 2024-10-24T23:17:51Z
url: https://github.com/astral-sh/uv/pull/8543
synced_at: 2026-01-10T12:54:11Z
```

# Add `tool.uv.sources` to the "Settings" reference

---

_Pull request opened by @charliermarsh on 2024-10-24 20:42_

## Summary

Closes https://github.com/astral-sh/uv/issues/8540.


---

_Label `documentation` added by @charliermarsh on 2024-10-24 20:42_

---

_@charliermarsh reviewed on 2024-10-24 20:51_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:161 on 2024-10-24 20:51_

@zanieb -- I guess ideally this would be a relative path to the Markdown. But then it will _stay_ as a path in the JSON schema, which seems suboptimal. Wdyt?

---

_@zanieb reviewed on 2024-10-24 20:58_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:161 on 2024-10-24 20:58_

You could steal this pattern we use in the CLI reference

https://github.com/astral-sh/uv/blob/715f28fd398aae1a6347deb312e3d0a27161696b/crates/uv-dev/src/generate_cli_reference.rs#L16-L28

Not pretty but we could use it to patch either the JSON schema or the Markdown.

---

_@zanieb approved on 2024-10-24 20:58_

---

_@charliermarsh reviewed on 2024-10-24 21:25_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:161 on 2024-10-24 21:25_

Done, thanks! I also tweaked the code to enforce that we see at least one match.

---

_Merged by @charliermarsh on 2024-10-24 23:17_

---

_Closed by @charliermarsh on 2024-10-24 23:17_

---

_Branch deleted on 2024-10-24 23:17_

---
