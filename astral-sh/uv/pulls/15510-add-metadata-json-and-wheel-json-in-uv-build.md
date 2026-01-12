```yaml
number: 15510
title: "Add `METADATA.json` and `WHEEL.json` in uv build backend"
type: pull_request
state: open
author: konstin
labels:
  - preview
  - build-backend
assignees: []
base: main
head: konsti/json-metadata
created_at: 2025-08-25T08:35:45Z
updated_at: 2026-01-09T13:51:44Z
url: https://github.com/astral-sh/uv/pull/15510
synced_at: 2026-01-12T16:11:47Z
```

# Add `METADATA.json` and `WHEEL.json` in uv build backend

---

_@konstin_

https://discuss.python.org/t/pep-819-json-package-metadata/105558

Add an experimental JSON format for METADATA and WHEEL files in the uv build backend to have a reference what it would look like and to demonstrate that the actual change is just a few lines, how much easier it is than the existing email header format.

The change is overwhelmingly adding the preview feature as it's the first preview item in the build backend, writing the actual JSON is only a couple of lines.


---

_Label `preview` added by @konstin on 2025-08-25 08:35_

---

_Label `build-backend` added by @konstin on 2025-08-25 08:35_

---

_@konstin reviewed on 2025-08-25 08:36_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:790 on 2025-08-25 08:36_

We're now writing a trailing newline.

---

_Renamed from "Add `METADATA.json` and `WHEEL.json` to build backend" to "Add `METADATA.json` and `WHEEL.json` in uv build backend" by @konstin on 2025-08-25 08:36_

---

_Comment by @konstin on 2025-08-26 15:32_

Before merging, we need to extend this to include the full transformations from https://peps.python.org/pep-0566/#json-compatible-metadata

---

_Marked ready for review by @konstin on 2026-01-06 12:09_

---

_Comment by @konstin on 2026-01-06 12:09_

PEP 819 is up, so I'd like to merge this as a preview feature.

---

_Review requested from @EliteTK by @konstin on 2026-01-09 12:15_

---

_Review comment by @EliteTK on `crates/uv-build/src/main.rs`:50 on 2026-01-09 12:28_

Reads like the variable name itself is not valid UTF-8.

Maybe (matches the error below)

```suggestion
                format!("Invalid UTF-8 in `{}`", EnvVars::UV_PREVIEW_FEATURES)
```

Or (might not compile as is, might want `{:?}` instead of `.display()`):

```suggestion
                format!("Invalid UTF-8 in `{}`: `{}`", EnvVars::UV_PREVIEW_FEATURES, preview_features.display())
```

---

_Review comment by @EliteTK on `crates/uv-build-backend/src/lib.rs`:1826 on 2026-01-09 12:43_

We should check their contents too maybe?

---

_Review comment by @EliteTK on `crates/uv-pypi-types/src/metadata/metadata23.rs`:322 on 2026-01-09 13:35_

I'd say leave it as is, but it feels a bit odd to be using `Display` to handle serialisation for machines rather than humans.

---

_Review comment by @EliteTK on `crates/uv-pypi-types/src/metadata/metadata23.rs`:354 on 2026-01-09 13:47_

nit: I think this parsing and serialisation belongs in a `ProjectUrl` type. But I can see how that would be a bit of churn due to the overarching need for `IndexMap`.

---

_@EliteTK approved on 2026-01-09 13:51_

Fun fact:

This PR adds 88 (118) new instances of the word "preview" to uv. For a grand total of 1525 (1935) instances. (Counts in brackets are when just searching for "preview" rather than for words.)


---
