```yaml
number: 16217
title: "Add missing \"added in\" to new environment variables in reference"
type: pull_request
state: merged
author: samypr100
labels:
  - documentation
assignees: []
merged: true
base: main
head: missing_added_in
created_at: 2025-10-09T20:25:28Z
updated_at: 2025-10-13T20:44:58Z
url: https://github.com/astral-sh/uv/pull/16217
synced_at: 2026-01-12T16:12:10Z
```

# Add missing "added in" to new environment variables in reference

---

_@samypr100_

## Summary

Adds the version for environment variables added in https://github.com/astral-sh/uv/pull/16040 and https://github.com/astral-sh/uv/pull/16125. as these were in-flight before documentation versioning was added.

Adds ability to emit a compiler error when added in is missing for improved reporting to the developer.

e.g. example for the ones fixed in this PR

```shell
error: missing #[attr_added_in("x.y.z")] on `UV_UPLOAD_HTTP_TIMEOUT`
       note: env vars for an upcoming release should be annotated with `#[attr_added_in("next release")]`
   --> crates\uv-static\src\env_vars.rs:593:15
    |
593 |     pub const UV_UPLOAD_HTTP_TIMEOUT: &'static str = "UV_UPLOAD_HTTP_TIMEOUT";
    |               ^^^^^^^^^^^^^^^^^^^^^^

error: missing #[attr_added_in("x.y.z")] on `UV_WORKING_DIRECTORY`
       note: env vars for an upcoming release should be annotated with `#[attr_added_in("next release")]`
    --> crates\uv-static\src\env_vars.rs:1087:15
     |
1087 |     pub const UV_WORKING_DIRECTORY: &'static str = "UV_WORKING_DIRECTORY";
     |               ^^^^^^^^^^^^^^^^^^^^
error: could not compile `uv-static` (lib) due to 2 previous errors
```


---

_Label `documentation` added by @samypr100 on 2025-10-09 20:28_

---

_@zanieb approved on 2025-10-09 22:45_

---

_@samypr100 reviewed on 2025-10-10 00:01_

---

_Review comment by @samypr100 on `crates/uv-macros/src/lib.rs`:137 on 2025-10-10 00:01_

Note, we could've used syn:Error but I thought it'd be overkill for this simple use case.

---

_Renamed from "chore(🧹): missing added_in on new env vars" to "Missing added_in on new env vars" by @konstin on 2025-10-10 07:58_

---

_Merged by @zanieb on 2025-10-10 13:55_

---

_Closed by @zanieb on 2025-10-10 13:55_

---

_Renamed from "Missing added_in on new env vars" to "Add missing "added in" to new environment variables in reference" by @zanieb on 2025-10-10 13:56_

---

_Branch deleted on 2025-10-13 20:44_

---
