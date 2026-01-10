---
number: 17156
title: Workspace package resolution improvement
type: issue
state: open
author: DiDeoxy
labels:
  - enhancement
assignees: []
created_at: 2025-12-17T00:31:50Z
updated_at: 2025-12-18T17:23:01Z
url: https://github.com/astral-sh/uv/issues/17156
synced_at: 2026-01-10T01:26:14Z
---

# Workspace package resolution improvement

---

_Issue opened by @DiDeoxy on 2025-12-17 00:31_

### Summary

Hello, I am running into an issue when I have a workspace where some uv sources are in git submodules that are themselves workspaces. I know that you cant have workspaces of workspace and that is fine. What I do instead is the top-level workspace points directly to the members of the submodule workspaces that I want, and for the most part this works fine, for example:

```
root_ws/
├── root_pyproject.toml
├── external/
│   └── package_d/
├── packages/
│   └── package_a/
└── external_ws/
    └── submodule_1/  (git submodule)
        ├── submodule_1_pyproject.toml
        └── packages/
            ├── package_b/
            └── package_c/
```

In root_pyproject.toml:

```toml
[tool.uv.workspace]
members = ["packages/*", "external/*", "external_ws/submodule_1/packages/*"]

[tool.uv.sources]
package_a = { workspace = true }
package_b = { workspace = true }
package_c = { workspace = true }
package_d = { workspace = true }
```

This above works and I am able to sync `root_pyproject.toml` and have all packages available.

However, an issue arrises when I have `package_d` in multiple submodules (where `package_d` is a itself a submodule containing a Python package):

```
root_ws/
├── root_pyproject.toml
├── external/
│   └── package_d/
├── packages/
│   └── package_a/
└── external_ws/
    └── submodule_1/  (git submodule)
        ├── submodule_1_pyproject.toml
        ├── external/
        │   └── package_d/
        └── packages/
            ├── package_b/
            └── package_c/
```

```toml
[tool.uv.workspace]
members = ["packages/*", "external/*", "external_ws/submodule_1/packages/*"]

[tool.uv.sources]
package_a = { workspace = true }
package_b = { workspace = true }
package_c = { workspace = true }
package_d = { workspace = true }
```

I do not include the `external_ws/submodule_1/external` directory as a member, only the packages directory so `package_d` is included once in the root workspace. However when I try to sync I get a dependency resolution error like below:

```
uv sync
  × Failed to resolve dependencies for `httpx-custom` (v0.6.1.post1.dev9)
  ╰─▶ Requirements contain conflicting URLs for package `configgi` in split `python_full_version >= '3.12'`:
      - file:///Users/me/root_ws/packages/pacakge_a (editable)
      - file:///Users/me/root_ws/external_ws/submodule_1/external/package_d (editable)
```

This is because for some reason uv is reading `external_ws/submodule_1/submodule_1_pyproject.toml` and including its settings in the resolition. The only solution I have found is to delete the `submodule_1_pyproject.toml` file after cloning the submodule, but before running `uv sync`, which is not ideal.

It would be great if I uv ignored non-root non-package pyproject.toml files when resolving dependencies in a workspace.

### Example

_No response_

---

_Label `enhancement` added by @DiDeoxy on 2025-12-17 00:31_

---

_Comment by @konstin on 2025-12-17 09:52_

What's the `tool.uv.sources` configuration in `external_ws/submodule_1/packages/`? We could have an option that ignores sources for a specific path dependency, but then it would ignore all sources in it, i.e. all of the dependency's workspace members. Otherwise I'm afraid that error message sounds correct, uv can't tell that the packages are the same and needs a different package tree.

---

_Comment by @DiDeoxy on 2025-12-17 17:01_

In `external_ws/submodule_1/submodule_1_pyproject.toml`:

```
[tool.uv.workspace]
members = ["packages/*", "external/*"]

[tool.uv.sources]
package_b = { workspace = true }
package_c = { workspace = true }
package_d = { workspace = true }
```

Which is where the issue arises since both here and `root_pyproject.toml` have different paths for `package_d`. What I don't understand is why `external_ws/submodule_1/submodule_1_pyproject.toml` is even being read? It is not referenced in `root_pyproject.toml` at all, it is ignored but uv still reads it for some reason when making `uv.lock` for `root_pyproject.toml`.

---

_Comment by @konstin on 2025-12-18 17:23_

uv search the workspace root for each package by traversing upwards, to understand workspace references in it.

It seems roughly correct that uv rejects the package being in two separate locations at the same time.

---
