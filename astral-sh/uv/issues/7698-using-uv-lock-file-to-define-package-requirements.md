```yaml
number: 7698
title: Using uv.lock file to define package requirements?
type: issue
state: open
author: TARHiS
labels:
  - enhancement
  - projects
assignees: []
created_at: 2024-09-26T07:40:29Z
updated_at: 2024-09-29T18:37:06Z
url: https://github.com/astral-sh/uv/issues/7698
synced_at: 2026-01-12T15:59:16Z
```

# Using uv.lock file to define package requirements?

---

_@TARHiS_

When building package with `uv build`, is it possible to use uv.lock file as a requirements of a package instead of using pyproject.toml dependencies? Use case for this is to create application package that is used to deploy web servers by using private PyPI as a distribution channel.


---

_Comment by @zanieb on 2024-09-26 12:21_

This would be cool 👍

---

_Label `enhancement` added by @zanieb on 2024-09-26 12:21_

---

_Label `projects` added by @zanieb on 2024-09-26 12:21_

---

_Comment by @tfsingh on 2024-09-28 06:40_

I've been doing a good amount of digging on this issue and wanted to check if I'm thinking about this correctly.

My current understanding is that `uv` itself does not handle dependencies specified in ```pyproject.toml``` files during the build process (```pyproject.toml``` files are only scraped for  [package names/roots](https://github.com/astral-sh/uv/blob/313612dad7a6843f753f2331d38b07b896676701/crates/uv/src/commands/build.rs#L214) during ```uv build```, afaict). 

Instead, uv defers to [setup_tools as the build backend](https://github.com/astral-sh/uv/blob/313612dad7a6843f753f2331d38b07b896676701/crates/uv-build-frontend/src/lib.rs#L40) via the execution of an intermediary [Python script](https://github.com/astral-sh/uv/blob/313612dad7a6843f753f2331d38b07b896676701/crates/uv-build-frontend/src/lib.rs#L691). My understanding is that from here, `setup_tools` uses the given path, scrapes the dependencies from the corresponding `pyproject.toml`, and builds the package.

I'll be candid in that my understanding of Python packaging (while improving) is limited, but if the above is true then I think this issue might require either:

1. Injecting some custom logic into setup tools via [extensions](https://setuptools.pypa.io/en/latest/userguide/extension.html) (looks promising but requires more diligence, haven't verified this would work)
2. Custom `uv` build backend :)

Let me know if I'm thinking about this correctly and if so, I'll do some more digging into the first potential solution.

---

_Comment by @tfsingh on 2024-09-29 18:37_

cc'ing @charliermarsh for your thoughts on the above

---
