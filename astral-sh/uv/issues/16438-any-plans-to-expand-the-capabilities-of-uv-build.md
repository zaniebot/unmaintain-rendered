```yaml
number: 16438
title: "Any plans to expand the capabilities of `uv build` to allow more control over sdist and wheel contents?"
type: issue
state: open
author: rzuckerm
labels:
  - question
assignees: []
created_at: 2025-10-24T15:16:46Z
updated_at: 2025-10-29T18:27:54Z
url: https://github.com/astral-sh/uv/issues/16438
synced_at: 2026-01-12T16:02:31Z
```

# Any plans to expand the capabilities of `uv build` to allow more control over sdist and wheel contents?

---

_@rzuckerm_

### Question

I'm investigate convert our internal build system to use uv instead of poetry. I would very much like to use `uv_build` for the build backend instead of using something like `hatchling`. However, there are some use-cases where more advanced features of the poetry build backend are being used, which have a lot more control of the sdist and wheel contents:

- Files/directories can be included/exclude independently for the sdist and wheel (https://python-poetry.org/docs/pyproject/#exclude-and-include)
- The path within the sdist and wheel can be remapped (https://python-poetry.org/docs/pyproject/#packages)
- Extension modules can be created (https://python-poetry.org/docs/building-extension-modules/). Yes I'm aware of this issue: #10569

Any plans to expand the capabilities of `uv_build` to match the capabilities of poetry?

### Platform

Ubuntu 24.04 amd64

### Version

0.9.5

---

_Label `question` added by @rzuckerm on 2025-10-24 15:16_

---

_Comment by @konstin on 2025-10-29 18:27_

While we don't plan to keep the uv build backend fixed, there are no plans for the immediate future to support these items. For project layout and inclusion, we try to keep the configuration simple and hard to get wrong by being more restrictive about the directory structure.

Building extensions modules may be supported in the future, currently we recommend using a build backend specializing in your programming language and build tool. It's also possible to split between a native `-core` package and a pure Python library that uses the core package.

---
