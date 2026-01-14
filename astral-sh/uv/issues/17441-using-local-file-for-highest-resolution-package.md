```yaml
number: 17441
title: Using local file for highest resolution, package index for lowest-direct
type: issue
state: open
author: khoover
labels:
  - enhancement
assignees: []
created_at: 2026-01-13T17:37:31Z
updated_at: 2026-01-14T15:25:25Z
url: https://github.com/astral-sh/uv/issues/17441
synced_at: 2026-01-14T15:39:47Z
```

# Using local file for highest resolution, package index for lowest-direct

---

_@khoover_

### Summary

When writing libraries, we often want to test using both `uv lock --resolution lowest-direct` and `uv lock --resolution highest -U`, to ensure declared minimums are accurate and that we don't break on the newest stuff. One problem is that in a monorepo environment, `--resolution highest -U` won't actually pick up the true latest version, which would actually require a file-path source. The other problem is we don't want to `override-dependencies` the package to the file path, because then `--resolution lowest-direct` won't actually use the published version we set as minimum.

What I'm looking for is some way of having `lowest-direct` pull the specified minimum version from a packaging index still, but for `highest` to use the local relative path. Either that, or something I can add to the `uv lock --resolution highest` invocation that will have the lockfile point to the local path instead of the latest published version.

Note: I suppose one alternative is making use of prerelease versions, but that still leaves a gap if two libraries get modified in the same PR; can make that prohibited, but it is an annoyance.

### Example

_No response_

---

_Label `enhancement` added by @khoover on 2026-01-13 17:37_

---

_Comment by @zanieb on 2026-01-13 21:13_

I think you just want to use `--no-sources` for the lowest-direct case and have a path (or workspace) dependency otherwise, does that make sense?

---

_Comment by @khoover on 2026-01-13 21:15_

Huh. Yeah, that would do it.

---

_Closed by @khoover on 2026-01-13 21:15_

---

_Comment by @khoover on 2026-01-14 15:22_

Actually, not fully. These libraries depend on PyTorch, where we need the custom source for getting the right wheels (although we may be fine with the default wheel that goes on PyPI, I'll try some stuff). So it's closer to "`--no-sources` for specific packages".

---

_Reopened by @khoover on 2026-01-14 15:22_

---
