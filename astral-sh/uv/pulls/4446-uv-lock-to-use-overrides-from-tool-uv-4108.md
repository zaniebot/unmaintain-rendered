```yaml
number: 4446
title: "uv lock to use overrides from tool.uv (#4108)"
type: pull_request
state: closed
author: idlsoft
labels: []
assignees: []
base: main
head: lock_with_overrides_fs
created_at: 2024-06-21T23:18:05Z
updated_at: 2024-06-24T16:45:03Z
url: https://github.com/astral-sh/uv/pull/4446
synced_at: 2026-01-12T16:06:14Z
```

# uv lock to use overrides from tool.uv (#4108)

---

_@idlsoft_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This will make `uv lock` read `override-dependencies` from  the `[tool.uv]` section of `pyproject.toml`.
Resolves #4108

Alternative to https://github.com/astral-sh/uv/pull/4369, an implementation more consistent with `pip compile` and `pip install`. 

## Test Plan

Unit test

---

_Marked ready for review by @idlsoft on 2024-06-21 23:32_

---

_@konstin approved on 2024-06-23 19:20_

---

_Review requested from @charliermarsh by @konstin on 2024-06-23 19:20_

---

_Comment by @charliermarsh on 2024-06-23 21:48_

I find it a bit strange that overrides are read as configuration (via the settings schema) rather than as part of the `pyproject.toml` itself, i.e., part of the workspace _data_ (similar to, like, `tool.uv.dev-dependencies`). I guess this is necessary to respect these overrides in the `pip` API in addition to `uv lock` etc.?


---

_Comment by @charliermarsh on 2024-06-23 21:53_

I feel like the approach in https://github.com/astral-sh/uv/pull/4369 is, perhaps, more correct (but only reading from the workspace root)? What do you think @konstin - which approach is more natural to you? Here we're reading the overrides as settings, but we then have the workspace root available to us in `uv lock`.

---

_Comment by @charliermarsh on 2024-06-24 16:45_

Closing in favor of https://github.com/astral-sh/uv/pull/4369. Thank you!

---

_Closed by @charliermarsh on 2024-06-24 16:45_

---
