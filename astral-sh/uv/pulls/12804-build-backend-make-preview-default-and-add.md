```yaml
number: 12804
title: "Build backend: Make preview default and add configuration docs"
type: pull_request
state: merged
author: konstin
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-preview-default
created_at: 2025-04-10T12:14:39Z
updated_at: 2025-05-05T13:52:32Z
url: https://github.com/astral-sh/uv/pull/12804
synced_at: 2026-01-10T11:10:40Z
```

# Build backend: Make preview default and add configuration docs

---

_Pull request opened by @konstin on 2025-04-10 12:14_

Add configuration documentation for the build backend and make it the preview default.

The build backend should generally work with default configuration unless you want specific features such as flat layout or module renaming, there is only a dedicated configuration, but no concept or guide page for the build backend. Once the build backend is stable, we can update the guide documentation to explain that uv defaults to its own build backend, but other build backends are also supported.

The uv build backend becomes the default in preview, giving it more exposure from users and preparing it to make it the default proper. The current documentation retains warnings that the build backend is in preview.

To see current uses of `uv_build` on GitHub: https://github.com/search?q=path%3A**%2Fpyproject.toml+uv_build%3E%3D0&type=code

---

_Label `documentation` added by @konstin on 2025-04-10 12:14_

---

_Label `preview` added by @konstin on 2025-04-10 12:14_

---

_@zanieb reviewed on 2025-04-16 16:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:450 on 2025-04-16 16:22_

Should we also test `uv init --package --preview`?

I'd also expect, though it could be a future change, for `uv init --preview` to package by default.

---

_@zanieb reviewed on 2025-04-16 16:23_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:7 on 2025-04-16 16:23_

```suggestion
    The uv build backend is currently in preview and may change without warning.

    When preview mode is not enabled, uv uses [hatchling](https://pypi.org/project/hatchling/) as the default build backend.
```

---

_@zanieb reviewed on 2025-04-16 16:24_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:9 on 2025-04-16 16:24_

Maybe
```suggestion
A build backend transforms a source tree (i.e., a directory) into a source distribution or a wheel. While uv
```

---

_@zanieb reviewed on 2025-04-16 16:25_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:10 on 2025-04-16 16:25_

```suggestion
supports all build backends (as specified by PEP 517), it includes a `uv_build` backend that integrates tightly
```

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:11 on 2025-04-16 16:25_

```suggestion
with uv to improve performance and user experience.
```

---

_@zanieb reviewed on 2025-04-16 16:25_

---

_@zanieb reviewed on 2025-04-16 16:25_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:13 on 2025-04-16 16:25_

Can we link to a universal wheel definition?

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:17 on 2025-04-16 16:26_

```suggestion
To use the uv build backend in an existing project, add it to the `[build-system]` section in your `pyproject.toml`:
```

---

_@zanieb reviewed on 2025-04-16 16:26_

---

_@zanieb reviewed on 2025-04-16 16:27_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:21 on 2025-04-16 16:27_

Should we add a note admonition about the bounds here?

We'll also need to add this file to the Rooster update targets if we want to use the current version as a lower bound.

---

_@zanieb reviewed on 2025-04-16 16:27_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:25 on 2025-04-16 16:27_

```suggestion
You can also create a new project that uses the uv build backend with `uv init`:
```

---

_@zanieb reviewed on 2025-04-16 16:28_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:31 on 2025-04-16 16:28_

```suggestion
`uv_build` is a separate package from uv, optimized for portability and small binary size. `uv`
```

---

_@zanieb reviewed on 2025-04-16 16:29_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:31 on 2025-04-16 16:29_

I wonder if we should omit the backticks from `uv` here? I don't have strong feelings. I would maybe say, "The `uv` package..." if you want to keep them.

---

_@zanieb reviewed on 2025-04-16 16:35_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:38 on 2025-04-16 16:35_

```suggestion
To select which files to include in the source distribution, uv first adds the included files and
```

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:39 on 2025-04-16 16:35_

```suggestion
directories, then removes the excluded files and directories. This means that exclusions always take precedence over inclusions.
```

---

_@zanieb reviewed on 2025-04-16 16:35_

---

_@zanieb reviewed on 2025-04-16 16:36_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:50 on 2025-04-16 16:36_

Can you use "uv" instead of "we" throughout?

---

_@zanieb reviewed on 2025-04-16 16:36_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:75 on 2025-04-16 16:36_

Should this last sentence be in a `!!! important` or warning admonition or is it not critical to highlight?

---

_Review comment by @zanieb on `mkdocs.template.yml`:143 on 2025-04-16 16:38_

I know specifying "uv" might be helpful for disambiguation, but I think it's okay given we have the disclaimer at the top. I would rather avoid the redundancy / length.

```suggestion
      - Build backend: configuration/build_backend.md
```

---

_@zanieb reviewed on 2025-04-16 16:38_

---

_@zanieb reviewed on 2025-04-16 16:38_

---

_Review comment by @zanieb on `mkdocs.template.yml`:143 on 2025-04-16 16:38_

On another note, all the other files use `-` instead of `_`, can you update that?

---

_@zanieb reviewed on 2025-04-16 16:40_

---

_Review comment by @zanieb on `docs/configuration/build_backend.md`:3 on 2025-04-16 16:40_

We should also link to the build system config in here

https://docs.astral.sh/uv/concepts/projects/config/#build-systems

---

_@konstin reviewed on 2025-04-23 13:37_

---

_Review comment by @konstin on `docs/configuration/build_backend.md`:75 on 2025-04-23 13:37_

The worst case that can happen is that it is moderately slow, unless you're on an NFS have another reason for extremely high IO latency.

---

_Comment by @konstin on 2025-04-25 08:20_

I've addressed all comments, this is ready for another round.

---

_Marked ready for review by @konstin on 2025-04-25 08:21_

---

_@zanieb reviewed on 2025-05-05 13:34_

---

_Review comment by @zanieb on `pyproject.toml`:89 on 2025-05-05 13:34_

This looks wrong
```suggestion
  "docs/configuration/build-backend.md",
```

---

_@zanieb reviewed on 2025-05-05 13:34_

---

_Review comment by @zanieb on `docs/configuration/build-backend.md`:18 on 2025-05-05 13:34_

This needs to be relative

---

_@zanieb reviewed on 2025-05-05 13:34_

---

_Review comment by @zanieb on `docs/configuration/build-backend.md`:30 on 2025-05-05 13:34_

This needs to be relative

---

_@zanieb approved on 2025-05-05 13:34_

---

_Merged by @konstin on 2025-05-05 13:52_

---

_Closed by @konstin on 2025-05-05 13:52_

---

_Branch deleted on 2025-05-05 13:52_

---
