---
number: 11483
title: Support for Resolving Explicit Prerelease Transient Dependencies
type: issue
state: open
author: Ghelfi
labels:
  - enhancement
assignees: []
created_at: 2025-02-13T16:25:34Z
updated_at: 2025-02-13T16:42:04Z
url: https://github.com/astral-sh/uv/issues/11483
synced_at: 2026-01-10T01:25:06Z
---

# Support for Resolving Explicit Prerelease Transient Dependencies

---

_Issue opened by @Ghelfi on 2025-02-13 16:25_

### Summary

Hi,

Currently, `uv` accepts pre-releases for packages that exclusively publish pre-releases, as well as for first-party requirements that explicitly specify pre-release versions (from [doc](https://docs.astral.sh/uv/reference/settings/#prerelease)).

However, I encountered a scenario where a first-party dependency has a pre-release marker **and** also depends on another **pinned** pre-release requirement.
I anticipated that explicit pre-release statements would be resolved appropriately, but this is not currently supported by `uv`.

Could we consider an additional setting, `prerelease-transient`, with values `["allow", "disallow"]`? This setting would be utilized when the general `prerelease` configuration permits fetching pre-releases. It would enable explicit transient pre-releases to be resolved accurately without activating pre-releases for all dependencies.

Furthermore, could we consider allowing the prerelease setting to be configured at the individual dependency level instead of only at the global level? This enhancement would provide more granular control over dependency resolution. This would mirror the existing capability of setting indexes at the package level.

Best,
Alex

### Example

Toml for package `"my-lib"`:
```
[project]
name = "my-lib"
dependencies = [
    "my-package-1===1.0.0.post0.dev1",
    "stable-package>=2.0.0",

]
```

Toml for package `"my-package-1"`:
```
[project]
name = "my-package-1"
version = "1.0.0.post0.dev1"
dependencies = [
    "my-package-2===1.0.0.post0.dev1",
]
```
In this setting I want to run `uv lock` for `"my-lib"` and resolve the pre-releases of `"my-package-1"` and `"my-package-2"` without having pre-relases of `"stable-package"`.

---

_Label `enhancement` added by @Ghelfi on 2025-02-13 16:25_

---

_Referenced in [sigstore/protobuf-specs#288](../../sigstore/protobuf-specs/issues/288.md) on 2025-04-10 15:24_

---
