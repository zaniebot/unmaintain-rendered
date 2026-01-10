```yaml
number: 12803
title: "Build backend: Add reference docs and schema"
type: pull_request
state: merged
author: konstin
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-docs
created_at: 2025-04-10T10:40:26Z
updated_at: 2025-04-21T11:45:29Z
url: https://github.com/astral-sh/uv/pull/12803
synced_at: 2026-01-10T11:10:40Z
```

# Build backend: Add reference docs and schema

---

_Pull request opened by @konstin on 2025-04-10 10:40_

Add reference documentation and schema integration for the uv build backend. The reference documentation comes with a preview note upfront.

---

_Label `documentation` added by @konstin on 2025-04-10 10:40_

---

_Label `preview` added by @konstin on 2025-04-10 10:40_

---

_@konstin reviewed on 2025-04-10 10:41_

---

_Review comment by @konstin on `crates/uv-pypi-types/src/identifier.rs`:111 on 2025-04-10 10:41_

I didn't find a good resource for proper ECMA script regex validation.

---

_Review comment by @konstin on `crates/uv-workspace/src/build_backend.rs`:33 on 2025-04-10 10:45_

We had bug reports caused by this exact problem, but unfortunately it's a somewhat popular feature (scikit-learn -> sklearn, Pillow -> PIL, etc.). It also allows users to pick their preferred package name even if it is already taken on PyPI by only renaming the installation name to something free while docs and `import <...>` can have any name.

---

_@konstin reviewed on 2025-04-10 10:45_

---

_@konstin reviewed on 2025-04-10 10:52_

---

_Review comment by @konstin on `crates/uv-workspace/src/build_backend.rs`:94 on 2025-04-10 10:52_

There isn't good documentation for this, the data feature is somewhat implementation defined.

---

_@konstin reviewed on 2025-04-10 10:57_

---

_Review comment by @konstin on `docs/reference/settings.md`:384 on 2025-04-10 10:57_

I wouldn't know why you would use them for a pure Python project, but they exist in the spec so they exist on the build backend config:

- scripts: This feature is used to ship compiled binaries (including ruff and uv), but platform specific binaries don't match with universal wheels
- headers: Afaik only used for extension modules
- data: Writes directly into the venv root, it's better to isolate data files into the module. For communicating with other packages such as for plugin interfaces, entrypoints should be used.
- purelib and platlib: No production usage known

---

_@zanieb reviewed on 2025-04-10 16:01_

---

_Review comment by @zanieb on `crates/uv-pypi-types/src/identifier.rs`:111 on 2025-04-10 16:01_

cc @Gankra perhaps you could review this part

---

_Review comment by @Gankra on `crates/uv-pypi-types/src/identifier.rs`:111 on 2025-04-10 18:16_

This syntax seems to do the validation you want. You can test this out in e.g. vscode with the "even better toml" extension with the following `uv.toml`s.

This one errors:

```toml
"$schema" = "https://raw.githubusercontent.com/astral-sh/uv/f6e2bbdec7b53a6ce380e9ec27566e88a9144eda/uv.schema.json"

[build-backend]
module-name = "0abc"
```

This one passes:

```toml
"$schema" = "https://raw.githubusercontent.com/astral-sh/uv/f6e2bbdec7b53a6ce380e9ec27566e88a9144eda/uv.schema.json"

[build-backend]
module-name = "abc123"
```

(For local testing you can use `"$schema" = "file://./uv.schema.json"`)

---

_Review comment by @Gankra on `crates/uv-workspace/src/build_backend.rs`:120 on 2025-04-10 18:48_

```suggestion
            data = { "headers": "include/headers", "scripts": "bin" }
```

---

_Review comment by @Gankra on `crates/uv-workspace/src/build_backend.rs`:145 on 2025-04-10 18:50_

NB this is a future-proofing hazard for schema users (if the schema is ever out of date it will freak out about the field being missing -- I think that's fine here, especially if we make the schema in schemastore hotlinked to our github repo).

---

_@Gankra approved on 2025-04-10 18:51_

---

_@konstin reviewed on 2025-04-11 09:22_

---

_Review comment by @konstin on `crates/uv-workspace/src/build_backend.rs`:145 on 2025-04-11 09:22_

I'm making a bet here that wheels won't get another data field (the list has been the same since 2012) and it's more likely that users mistype one of the data names (that's why it's the only type with `deny_unknown_fields` in the build backend).

---

_@zanieb approved on 2025-04-16 16:20_

---

_Merged by @konstin on 2025-04-21 10:27_

---

_Closed by @konstin on 2025-04-21 10:27_

---

_Branch deleted on 2025-04-21 10:27_

---

_Comment by @cthoyt on 2025-04-21 11:45_

thank you @konstin!!

---
