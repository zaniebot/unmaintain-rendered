```yaml
number: 7598
title: "Remove `flatten` to improve deserialization error messages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/flat
created_at: 2024-09-20T18:28:02Z
updated_at: 2024-09-20T18:54:14Z
url: https://github.com/astral-sh/uv/pull/7598
synced_at: 2026-01-12T16:07:54Z
```

# Remove `flatten` to improve deserialization error messages

---

_@charliermarsh_

## Summary

`#[serde(flatten)]` has a disastrous effect on error messages: serde no longer tells you which field errored, nor does it show it to you in the diagnostic output.

Before:

```
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 9, column 1
    |
  9 | [tool.uv]
    | ^^^^^^^^^
  invalid type: string "foo", expected a sequence
```

After:

```
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 10, column 19
     |
  10 | extra-index-url = "foo"
     |                   ^^^^^
  invalid type: string "foo", expected a sequence
```

Closes https://github.com/astral-sh/uv/issues/7113.


---

_Label `error messages` added by @charliermarsh on 2024-09-20 18:28_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-20 18:30_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-20 18:30_

---

_Comment by @zanieb on 2024-09-20 18:37_

Wow...

---

_@charliermarsh reviewed on 2024-09-20 18:37_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:39 on 2024-09-20 18:37_

I left these here because `schemars` uses them for the JSON schema.

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:1460 on 2024-09-20 18:38_

Is this meant to be commented?

---

_@zanieb reviewed on 2024-09-20 18:38_

---

_@charliermarsh reviewed on 2024-09-20 18:52_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:1460 on 2024-09-20 18:52_

Yes, to indicate where the various sections start and end.

---

_@zanieb reviewed on 2024-09-20 18:53_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:1460 on 2024-09-20 18:53_

That's sort of confusing, but okay.

---

_@zanieb approved on 2024-09-20 18:53_

---

_Merged by @charliermarsh on 2024-09-20 18:54_

---

_Closed by @charliermarsh on 2024-09-20 18:54_

---

_Branch deleted on 2024-09-20 18:54_

---
