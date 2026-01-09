---
number: 14442
title: "Build backend: only warn on invalid entry point names"
type: issue
state: closed
author: Tpt
labels:
  - enhancement
  - build-backend
assignees: []
created_at: 2025-07-03T15:39:34Z
updated_at: 2025-07-24T12:12:37Z
url: https://github.com/astral-sh/uv/issues/14442
synced_at: 2026-01-07T13:12:18-06:00
---

# Build backend: only warn on invalid entry point names

---

_Issue opened by @Tpt on 2025-07-03 15:39_

### Summary

Currently UV build backend fails with `Entrypoint names must consist of letters, numbers, dots, underscores and dashes` error if the entry point name is not valid according to [the data model](https://packaging.python.org/en/latest/specifications/entry-points/#data-model).

However it seems that this validation is only a SHOULD and not a MUST <q>For new entry points, it is recommended to use only letters, numbers, underscores, dots and dashes (regex [\w.-]+).</q>.

Would it make sense to convert this error into a warning? It is blocking the migration of one of my projects to `uv_build`

### Example

```toml
[project.entry-points."foo.bar"]
"https://example/" = "package:module"
```

---

_Label `enhancement` added by @Tpt on 2025-07-03 15:39_

---

_Label `build-backend` added by @konstin on 2025-07-03 15:57_

---

_Comment by @konstin on 2025-07-03 15:58_

Why do you need an entrypoint outside the recommendation for your project?

---

_Comment by @zanieb on 2025-07-03 15:58_

Is that the entry point name in your actual project?

---

_Comment by @Tpt on 2025-07-03 16:02_

@konstin I abuse entry points to allow installation of data dependencies using Pypi. Some Python packages do not contain any code but only data that are registered and exposed ot other Python packages using entry points. The URL format is there for historical reason and I might hack a bit to make it fit the entry-point data model recommandations.

@zanieb Not exactly but it's the idea: the entry point names are URLs (mostly for historical reasons).

---

_Comment by @nejch on 2025-07-23 11:42_

@zanieb another real-world example here would be MkDocs plugins namespaced by their package to avoid conflicts (e.g. custom search plugins like the one in mkdocs-material overriding the default mkdocs search plugin).

I'm pretty sure mkdocs-material (used for docs here as well) wouldn't be able to switch to uv at the moment:
https://github.com/squidfunk/mkdocs-material/blob/master/pyproject.toml#L77-L86

I'm not sure why they chose this approach to use slashes but it's pretty established there now.
Edit: more context here https://github.com/squidfunk/mkdocs-material/issues/4581

---

_Referenced in [astral-sh/uv#14867](../../astral-sh/uv/pulls/14867.md) on 2025-07-24 09:26_

---

_Closed by @konstin on 2025-07-24 12:12_

---
