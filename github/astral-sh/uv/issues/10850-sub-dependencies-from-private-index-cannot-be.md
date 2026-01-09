---
number: 10850
title: Sub-dependencies from private index cannot be resolved when explicit is true
type: issue
state: closed
author: olafdeleeuw
labels:
  - enhancement
assignees: []
created_at: 2025-01-22T10:37:22Z
updated_at: 2025-01-22T14:15:54Z
url: https://github.com/astral-sh/uv/issues/10850
synced_at: 2026-01-07T13:12:18-06:00
---

# Sub-dependencies from private index cannot be resolved when explicit is true

---

_Issue opened by @olafdeleeuw on 2025-01-22 10:37_

### Summary

I want to add a dependency, let's say `my-private-package` which is published in Azure Artifacts. 
The package `my-private-package` has a dependency itself which is also published in Azure Artifacts: `my-private-sub-package`.

I have the following setup in my `pyproject.toml`:

```toml
[tool.uv.sources]
my-private-package = { index = "my-private-index" }

[[tool.uv.index]]
name = "dna-nl-library"
url = "https://<subscription>.pkgs.visualstudio.com/<project>/_packaging/my-private-index/pypi/simple/"
explicit = true
```

If I run `uv add my-private-package` I get the following error:

  × No solution found when resolving dependencies for split (python_full_version == '3.11.*'):
  ╰─▶ Because my-private-sub-package was not found in the package registry and my-private-package depends on my-private-sub-package, we can conclude that my-private-package cannot be used.

Apparently, `explicit=true` says only `my-private-package` must be searched in "my-private-index". `uv` doesn't apply this index on the dependencies of `my-private-package`.

I tried adding `my-private-sub-package` to the `[tool.uv.source]` section, but this doesn't help.

The only two solutions are:

- set `explicit=False`
- add `my-private-sub-package` manually as a dependency 

It would be nice if the setting `explicit=true` holds also for the dependencies of the sources listed under `[tool.uv.source]`.

### Example

_No response_

---

_Label `enhancement` added by @olafdeleeuw on 2025-01-22 10:37_

---

_Comment by @charliermarsh on 2025-01-22 14:00_

This looks like a duplicate of #8253.

---

_Comment by @olafdeleeuw on 2025-01-22 14:14_

Yes, you’re right. Apologies, I checked the issues to see if this was already there, but I missed this one.

---

_Comment by @charliermarsh on 2025-01-22 14:15_

That's alright. I'll merge in there.

---

_Closed by @charliermarsh on 2025-01-22 14:15_

---
