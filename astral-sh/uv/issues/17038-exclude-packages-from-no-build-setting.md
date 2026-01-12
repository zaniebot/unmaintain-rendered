```yaml
number: 17038
title: "Exclude packages from `no-build` setting"
type: issue
state: open
author: mkniewallner
labels:
  - enhancement
assignees: []
created_at: 2025-12-08T21:04:56Z
updated_at: 2025-12-09T22:20:52Z
url: https://github.com/astral-sh/uv/issues/17038
synced_at: 2026-01-12T16:02:43Z
```

# Exclude packages from `no-build` setting

---

_@mkniewallner_

### Summary

Sync-related commands support [`no-build`](https://docs.astral.sh/uv/reference/settings/#no-build) setting (and the equivalent [`--no-build`](https://docs.astral.sh/uv/reference/cli/#uv-sync--no-build) argument and [`UV_NO_BUILD`](https://docs.astral.sh/uv/reference/environment/#uv_no_build) environment variable), which is useful to avoid running arbitrary code during building, or could be useful when bumping to a new minor Python version, to ensure that no wheels are lost when bumping to the new version.

Sometimes though, some packages (like [uWSGI](https://pypi.org/project/uWSGI/#files)) don't provide wheels at all. It doesn't seem that there's a way to exclude some specific packages from `no-build`. Would it make sense to support that? This would allow using the option by default as a security measure, but still make it possible to exclude specific packages that knowingly don't provide wheels.

Note that there is a [`no-build-package`](https://docs.astral.sh/uv/reference/settings/#no-build-package) setting, but it requires explicitly setting each package rather than excluding specific ones, which is not convenient since adding a dependency (or having a new transitive one) would require to update the list.

### Example

```toml
[tool.uv]
no-build = true
# Not sure what would be the best name.
no-build-exclude = ["uWSGI"]
```

---

_Label `enhancement` added by @mkniewallner on 2025-12-08 21:04_

---

_Comment by @zanieb on 2025-12-08 23:00_

You can do this already with `--no-binary-package uWSGI`, though it may be nice to have a separate field which does not ignore binaries.

---

_Comment by @mkniewallner on 2025-12-09 22:20_

> You can do this already with `--no-binary-package uWSGI`

Oh indeed, I missed that somehow.

> it may be nice to have a separate field which does not ignore binaries.

I guess that this would avoid failing if wheels are not present, but instead of completely skipping them, using them if they ever get available at some point, right? If that's the case it could indeed still be useful, though less important I believe with `--no-binary-package` being available, so more like a nice-to-have.

---
