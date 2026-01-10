---
number: 13838
title: Allow using workspace member dependency groups from workspace root
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-06-04T14:50:08Z
updated_at: 2025-06-04T17:53:38Z
url: https://github.com/astral-sh/uv/issues/13838
synced_at: 2026-01-10T01:25:39Z
---

# Allow using workspace member dependency groups from workspace root

---

_Issue opened by @zanieb on 2025-06-04 14:50_

> My use-case here is:
> 
> ```toml
> [project]
> name = "my-project"
> description = "mkdocs or sphinx-based website"
> dependencies = [
>   "helper_package",
>   "mkdocs",
>   "..."
> ]
> dynamic = ["version"]
> 
> [tool.uv.sources]
> helper_package = { path = "tools/helper_package", editable = true }
> ```
> 
> (The helper package is something I'm interested in publishing to PyPI.)
> 
> It would be nice to be able to add something like:
> 
> ```toml
> [dependency-groups]
> doc = [
>   "helper_package[group=doc]"
> ]
> ```
> 
> I want the helper package to generate an `sdist` that can be independently tested and have its docs built, so I would like to track its dependencies in its own `pyproject.toml`. This would allow me to use `uv sync --group doc` whether I'm working in the repository root or in an sdist.
> 
> Right now, the best option I have is to keep a `doc` extra and use `doc = ["helper_package[doc]"]`, which I was hoping to get away from. 

 _Originally posted by @effigies in [#13540](https://github.com/astral-sh/uv/issues/13540#issuecomment-2940153737)_

---

_Referenced in [astral-sh/uv#13540](../../astral-sh/uv/issues/13540.md) on 2025-06-04 14:50_

---

_Label `enhancement` added by @zanieb on 2025-06-04 14:50_

---

_Label `needs-design` added by @zanieb on 2025-06-04 14:50_

---

_Comment by @tlambert03 on 2025-06-04 17:31_

Amazing timing! was just googling this to see if it was possible. 👍

---

_Comment by @effigies on 2025-06-04 17:53_

Thinking about this a bit more, I assume that `[dependency-groups]` can't permit anything that's not permitted in PEP735, so this would be an option:

```diff
 [project]
 name = "my-project"
 description = "mkdocs or sphinx-based website"
 dependencies = [
   "helper_package",
   "mkdocs",
   "..."
 ]
 dynamic = ["version"]

 [tool.uv.sources]
-helper_package = { path = "tools/helper_package", editable = true }
+helper_package = { path = "tools/helper_package", editable = true, map-groups={doc = 'doc'} }

 [dependency-groups]
 doc = [
   "helper_package"
 ]
```

Not married to the syntax. The logic I'm imagining would be something like:

```py
# Any field that shows up in `dependency-groups` or `tool.uv.sources.*.map-groups`
all_dependency_groups: list[str] = collect_dependency_groups(...)
for name in all_dependency_groups:
    # Create new group if mapped
    group = dependency_groups.setdefault(name, [])
    for source in tool.uv.sources:
        if name in source['map-groups']:
            group.extend(get_group(source, source['map-groups'][name]))
```


---
