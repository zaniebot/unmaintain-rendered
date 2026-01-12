```yaml
number: 16697
title: "Error when a `project.license-files` glob matches nothing"
type: pull_request
state: merged
author: terror
labels:
  - enhancement
assignees: []
merged: true
base: main
head: license-files-error
created_at: 2025-11-12T02:09:51Z
updated_at: 2025-11-14T10:02:04Z
url: https://github.com/astral-sh/uv/pull/16697
synced_at: 2026-01-12T16:12:23Z
```

# Error when a `project.license-files` glob matches nothing

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16693

[`PEP 639`](https://peps.python.org/pep-0639/#add-license-files-key) requires build tools to error if any user-specified `project.license-files` glob fails to match a file, but uv currently allows the build to succeed and produces empty `.dist-info/licenses/` directories.

This PR enforces the spec by tracking matches for each glob during metadata generation, raising a clear
validation error when one is unmatched.

---

_Converted to draft by @terror on 2025-11-12 02:26_

---

_Marked ready for review by @terror on 2025-11-12 02:44_

---

_@terror reviewed on 2025-11-12 03:05_

---

_Review comment by @terror on `crates/uv-build-backend/src/metadata.rs`:414 on 2025-11-12 03:05_

Should we worry about symlinks to non-files? This will probably fail at some point downstream, so something to maybe consider 🤔 

---

_Review requested from @konstin by @zanieb on 2025-11-12 15:00_

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:376 on 2025-11-13 09:00_

Why aren't we using `PortableGlobParser::Pep639` here?

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:381 on 2025-11-13 09:05_

(This was already wrong before)

```suggestion
                        field: "project.license-files".to_string(),
```

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:779 on 2025-11-13 09:07_

Can you add a second glob that matches something to test that the errors are on a per-glob basis?

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:357 on 2025-11-13 09:08_

This field is redundant with `license_globs`

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:414 on 2025-11-13 09:13_

This seems fine to not handle specifically

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:372 on 2025-11-13 09:13_

This variable needs a comment why we're doing this

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:445 on 2025-11-13 09:14_

nit: Just `filter` is simpler than `then_some`

---

_@konstin reviewed on 2025-11-13 09:15_

---

_Review comment by @terror on `crates/uv-build-backend/src/metadata.rs`:376 on 2025-11-13 17:08_

Each one of those globs went through the PEP 639 parser, we're just calling `compile_matcher` on them here.

---

_@terror reviewed on 2025-11-13 17:08_

---

_@terror reviewed on 2025-11-13 17:19_

---

_Review comment by @terror on `crates/uv-build-backend/src/metadata.rs`:445 on 2025-11-13 17:19_

I broke this out into separate `find` and `map` calls to make this more clear, using `filter` here doesn't make sense to me 🤔 

---

_@konstin reviewed on 2025-11-14 08:12_

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:445 on 2025-11-14 08:12_

You don't need to map explicitly, you can use a `Some((_, pattern))` pattern

---

_@terror reviewed on 2025-11-14 08:15_

---

_Review comment by @terror on `crates/uv-build-backend/src/metadata.rs`:445 on 2025-11-14 08:15_

Ah right, just tweaked this

---

_Renamed from "Fail build when `project.license-files` globs match nothing" to "Error when a `project.license-files` glob matches nothing" by @konstin on 2025-11-14 10:01_

---

_Label `enhancement` added by @konstin on 2025-11-14 10:01_

---

_@konstin approved on 2025-11-14 10:01_

Thanks!

---

_Merged by @konstin on 2025-11-14 10:02_

---

_Closed by @konstin on 2025-11-14 10:02_

---
