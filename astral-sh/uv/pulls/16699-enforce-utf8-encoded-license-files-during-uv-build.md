```yaml
number: 16699
title: "Enforce UTF‑8-encoded license files during `uv build`"
type: pull_request
state: merged
author: terror
labels:
  - enhancement
assignees: []
merged: true
base: main
head: license-files-utf8
created_at: 2025-11-12T03:44:34Z
updated_at: 2025-11-13T12:50:00Z
url: https://github.com/astral-sh/uv/pull/16699
synced_at: 2026-01-12T16:12:24Z
```

# Enforce UTF‑8-encoded license files during `uv build`

---

_@terror_

I noticed this when working on https://github.com/astral-sh/uv/pull/16697.

[PEP 639](https://peps.python.org/pep-0639/#add-license-files-key) expects tools to ship license texts as UTF‑8, but previously `uv build` would quietly include any binary blob listed under `project.license-files`.

I have no clue what is going on with `rustfmt` for this file, but it seems that when I add the check, it wants to reformat a bunch of surrounding stuff.

The relevant part to look at is:

```rust
for license_file in &license_files {
    let file_path = root.join(license_file);
    let bytes = fs_err::read(&file_path)?;
    if str::from_utf8(&bytes).is_err() {
        return Err(ValidationError::LicenseFileNotUtf8(license_file.clone()).into());
    }
}
```

where we validate all collected license files before proceeding.

---

_Review requested from @konstin by @zanieb on 2025-11-12 15:00_

---

_Label `enhancement` added by @konstin on 2025-11-13 12:38_

---

_Comment by @konstin on 2025-11-13 12:39_

I've moved the license metadata parsing to its own function, it's been getting too big.

It makes sense to add since it's in the spec, but FWIW this isn't really a problem users run into, I don't think I've seen any problems from non-UTF-8 files in uv. The only part where we really need to care of non-UTF-8 inputs is Windows file paths.

---

_@konstin reviewed on 2025-11-13 12:41_

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:799 on 2025-11-13 12:41_

It's not ideal that we say the problem is `pyproject.toml`, the file itself is valid, I'll follow-up in a separate PR.

---

_Merged by @konstin on 2025-11-13 12:49_

---

_Closed by @konstin on 2025-11-13 12:50_

---
