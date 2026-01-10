---
number: 16426
title: "Introduce a uv configuration for `py.typed` file"
type: issue
state: open
author: pantheraleo-7
labels:
  - enhancement
assignees: []
created_at: 2025-10-23T16:49:26Z
updated_at: 2025-12-18T16:23:18Z
url: https://github.com/astral-sh/uv/issues/16426
synced_at: 2026-01-10T01:26:06Z
---

# Introduce a uv configuration for `py.typed` file

---

_Issue opened by @pantheraleo-7 on 2025-10-23 16:49_

### Summary

ok, so, this is too opinionated but I think this will be a cute little configuration option for uv to invent.

=========

Background:
Currently, a typed project, more specifically a 'library' project, should advertise that its typed in two ways:
- a `Typing :: Typed` trove classifier to aide in PyPI search
- a `py.typed` 'marker' file in the project to signal type checkers to use the library's type hints in downstream usage
(this is pretty basic and known, just putting it here for completeness sake)

=========

Feature Request:
Introduce a configuration option where _uv_ would add the `py.typed` file when building the project.

Rationale:
- This configuration would make the 'typed' metadata available in `pyproject.toml` (where it belongs) along with the `Typing :: Typed` classifier, to one place. (I guess the marker file was chosen so that type checkers won't have to read the whole `pyproject.toml` for just one field lol)
- This is a marker file, with this configuration enabled, its absence won't cause any confusion or breakage. It's not a source code file, so it shouldn't be PERSISTENTLY in the source code directory!
- My personal opinion/taste: having an empty file with a weird extension in the source directory for a very specific use-case which is arguably a project metadata more than anything, just doesn't sit right with me.

### Example

_No response_

---

_Label `enhancement` added by @pantheraleo-7 on 2025-10-23 16:49_

---

_Comment by @konstin on 2025-12-18 16:23_

The empty `py.typed` is a recommendation from Python itself, I'm hesitant to make a different choice in the uv build backend: https://typing.python.org/en/latest/guides/libraries.html#marking-a-package-as-providing-type-information

If you want this file to be generated, this should be a Python packaging proposal, so the configuration is shared between tools, similar to e.g. entrypoints.

---
