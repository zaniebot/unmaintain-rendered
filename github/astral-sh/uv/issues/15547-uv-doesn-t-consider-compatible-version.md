---
number: 15547
title: "UV doesn't consider compatible version"
type: issue
state: open
author: zivhadad24
labels:
  - bug
assignees: []
created_at: 2025-08-27T12:03:05Z
updated_at: 2025-08-27T19:23:16Z
url: https://github.com/astral-sh/uv/issues/15547
synced_at: 2026-01-07T13:12:19-06:00
---

# UV doesn't consider compatible version

---

_Issue opened by @zivhadad24 on 2025-08-27 12:03_

### Summary

Have a project that configures a few private registries as sources and uses Python 3.10 as required for the venv 
If one of the private sources will have a package that declared as dependency but not on the required Python version, UV will consider this source as the first match and not continue to search for the real package that compatible with the python version you aim to install 

Once UV finish the resolution it raise and error and failed to build the venv 

Example 
uv run --index-strategy first-index
  × No solution found when resolving dependencies:
  ╰─▶ Because crcmod==1.7 has no wheels with a matching Python version tag (e.g., `cp310`)
      and your project depends on crcmod==1.7, we can conclude that your project's
      requirements are unsatisfiable.

      hint: `crcmod` was found on
      https://gitlab.com/api/v4/projects/*********/packages/pypi/simple,
      but not at the requested version (crcmod==1.7). A compatible
      version may be available on a subsequent index (e.g.,
      https://test.jfrog.io/artifactory/api/pypi/test-pypi/simple). By
      default, uv will only consider versions that are published on the first index that
      contains a given package, to avoid dependency confusion attacks. If all indexes are
      equally trusted, use `--index-strategy unsafe-best-match` to consider all versions from
      all indexes, regardless of the order in which they were defined.

      hint: Wheels are available for `crcmod` (v1.7) with the following Python ABI tags:
      `cp39`, `pypy39_pp73`

Even when using --index-strategy unsafe-best-match it return the same error means that UV don't care about the python versions that compatible with the package it found and offer this package as the best one to use

### Platform

macOS, ubuntu22.2

### Version

uv 0.8.13

### Python version

python 3.10.18

---

_Label `bug` added by @zivhadad24 on 2025-08-27 12:03_

---

_Comment by @zanieb on 2025-08-27 15:55_

I think this is probably the same as #14836 and can be resolved by setting a required environment as described at https://github.com/astral-sh/uv/pull/14913 — we'll fix the default behavior at some point too.

---

_Comment by @zivhadad24 on 2025-08-27 17:58_

@zanieb Does it mean I need to python_full_version == 3.10.* for each package dependency? 

---

_Comment by @zanieb on 2025-08-27 18:04_

No, you'd set that in your `required-environments`. https://docs.astral.sh/uv/concepts/resolution/#required-environments

---

_Comment by @zivhadad24 on 2025-08-27 19:14_

It's working but only when --index-strategy=unsafe-best-match 
What about when we need UV to respect our private repository first? and only if our repository doesn't have compatible version it should continue to the next one

---

_Comment by @zanieb on 2025-08-27 19:23_

That's what `--index-strategy=unsafe-best-match` is for.

The other relevant issue is https://github.com/astral-sh/uv/issues/15298

---
