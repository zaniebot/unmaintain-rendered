```yaml
number: 3046
title: Add JSON Schema support
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/workspace-iii
created_at: 2024-04-15T23:03:41Z
updated_at: 2024-04-17T17:24:42Z
url: https://github.com/astral-sh/uv/pull/3046
synced_at: 2026-01-10T14:43:31Z
```

# Add JSON Schema support

---

_Pull request opened by @charliermarsh on 2024-04-15 23:03_

## Summary

This PR adds JSON Schema support. The setup mirrors Ruff's own.


---

_Label `configuration` added by @charliermarsh on 2024-04-15 23:03_

---

_Marked ready for review by @charliermarsh on 2024-04-15 23:03_

---

_@zanieb reviewed on 2024-04-15 23:31_

---

_Review comment by @zanieb on `.gitattributes`:3 on 2024-04-15 23:31_

ruff?

---

_@zanieb reviewed on 2024-04-15 23:31_

---

_Review comment by @zanieb on `.gitattributes`:3 on 2024-04-15 23:31_

```suggestion
uv.schema.json linguist-generated=true text=auto eol=lf
```

---

_@zanieb reviewed on 2024-04-15 23:33_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_json_schema.rs`:34 on 2024-04-15 23:33_

Should we push a newline to the end of this?

---

_@zanieb approved on 2024-04-15 23:34_

Can you update `CONTRIBUTING.md` too?

---

_Review comment by @konstin on `crates/distribution-types/src/index_url.rs`:39 on 2024-04-16 08:50_

Should this be `uri`? (there seems to be only `uri`, no `url`)

---

_Review comment by @konstin on `crates/distribution-types/src/index_url.rs`:141 on 2024-04-16 08:58_

I don't think there's a format that covers both urls and paths, i think we can't specify `format` here

---

_Review comment by @konstin on `crates/uv-configuration/src/name_specifiers.rs`:71 on 2024-04-16 09:05_

This should be `pattern: Some(r"^(:none:|:all:|([a-zA-Z0-9]|[a-zA-Z0-9][a-zA-Z0-9._-]*[a-zA-Z0-9]))$".to_string())` with a reference to https://packaging.python.org/en/latest/specifications/name-normalization/#name-format

---

_Review comment by @konstin on `crates/uv-dev/src/generate_json_schema.rs`:14 on 2024-04-16 09:07_

```suggestion
    /// Write the generated table to stdout (rather than to `uv.schema.json`).
```

---

_Review comment by @konstin on `crates/uv-resolver/src/exclude_newer.rs`:65 on 2024-04-16 09:27_

This is more restrictive than chrono but allows representing for dates and datetimes

`pattern: Some(r"^\d{4}-\d{2}-\d{2}(T\d{2}:\d{2}:\d{2}(Z|[+-]\d{2}:\d{2}))?$".to_string())`



---

_@konstin approved on 2024-04-16 09:28_

---

_@charliermarsh reviewed on 2024-04-17 00:31_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/exclude_newer.rs`:65 on 2024-04-17 00:31_

Thanks, I misunderstood "format".

---

_Merged by @charliermarsh on 2024-04-17 17:24_

---

_Closed by @charliermarsh on 2024-04-17 17:24_

---

_Branch deleted on 2024-04-17 17:24_

---
