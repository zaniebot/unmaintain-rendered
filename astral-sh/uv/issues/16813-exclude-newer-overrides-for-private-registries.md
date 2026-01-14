```yaml
number: 16813
title: "exclude-newer: overrides for private registries"
type: issue
state: open
author: acdha
labels:
  - enhancement
assignees: []
created_at: 2025-11-21T21:27:02Z
updated_at: 2026-01-14T10:35:45Z
url: https://github.com/astral-sh/uv/issues/16813
synced_at: 2026-01-14T11:33:32Z
```

# exclude-newer: overrides for private registries

---

_@acdha_

### Summary

`--exclude-newer` is a very useful feature for implementing dependency cooldown policies but it requires package registries to support the JSON view with the PEP 700 `upload-time` attribute. This currently prevents usage on projects which use [Azure](https://github.com/astral-sh/uv/issues/10394) or [GitLab](https://gitlab.com/gitlab-org/gitlab/-/issues/581770) to host any dependencies.

It would be useful if there was a way to disable the exclude-newer mechanism for a single registry or package so it could be used for PyPI packages, which are usually the highest concern for the kind of attacks which a minimum age policy is designed to mitigate, while still allowing private registries to be used.

### Example

Using the documentation example for a custom index, one option would be something like this:

```toml
[tool.uv]
exclude-newer = "…"
skip-exclude-newer-for-indexes = ["pytorch"]

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
```

… or perhaps to avoid that somewhat ugly name:

```toml
[tool.uv]
exclude-newer = "…"

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
skip-exclude-newer-check = true
```

… or perhaps this would be best as a per-package override requiring you to manually opt-in for each package:

```toml
[tool.uv]
exclude-newer = "…"
exclude-newer-package = { pytorch = "ALLOW_ANY" } # This magic value can't conflict with an existing date
# or, to make it more explicit that you only want to trust one source:
exclude-newer-package = { pytorch = { allow-latest-from-registry = "pytorch" } }
```


---

_Label `enhancement` added by @acdha on 2025-11-21 21:27_

---

_Comment by @zanieb on 2025-11-21 21:33_

I think it makes more sense for `exclude-newer` to be attachable to a `tool.uv.index` definition rather than define a list of exclusions? wdyt?

---

_Comment by @acdha on 2025-11-21 22:00_

> I think it makes more sense for `exclude-newer` to be attachable to a `tool.uv.index` definition rather than define a list of exclusions? wdyt?

That’s my inclination as well - I liked the middle proposed syntax the most when I was thinking about this but wanted to at least think about alternatives since every packaging discussion I have ever been in seems to involve someone bringing up a use-case wildly outside of what I do 😁

---

_Comment by @zanieb on 2025-11-21 22:20_

I'd also encourage you to let these private registries know you want this basic, standardized functionality :D

---

_Comment by @acdha on 2025-11-21 22:23_

> I'd also encourage you to let these private registries know you want this basic, standardized functionality :D

For completeness, I opened this ticket for GitLab for that exact reason and will be escalating with our sales rep: https://gitlab.com/gitlab-org/gitlab/-/issues/581770

---

_Comment by @javiertejero on 2025-12-12 14:06_

we observed the same problem with AWS CodeArtifact, which does not support JSON API  - it would be great to have this `uv` setting  to skip validation for some additional private indexes

UPDATE: I opened a ticket with AWS support and they will consider implementing JSON API but unfortunately there is not ETA yet

---

_Comment by @sumkincpp on 2026-01-13 19:25_

As of #16854 specific packages can be excluded with following syntax -

```yaml
[tool.uv]
exclude-newer-package = { pytorch = true } 
```

It's of course really usefull for packages stored in private registries, which should not normally require version/dependency cooldown.

Good question if it really works with Gitlab, gonna test it with the next uv release!

---

_Comment by @fellhorn on 2026-01-14 10:35_

> As of [#16854](https://github.com/astral-sh/uv/pull/16854) specific packages can be excluded with following syntax -
> 
> [tool.uv]
> exclude-newer-package = { pytorch = true } 

Nit: One needs to set the value to `false` instead of `true` to disable `exclude-newer` for a specific package:

```toml
[tool.uv]
exclude-newer-package = { pytorch = false } 
```

From what I can tell with GAR it works like a charm for registries that do not provide the `upload-time` metadata.

---
