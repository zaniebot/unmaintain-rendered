---
number: 7005
title: isort custom sections in a monorepo
type: issue
state: open
author: jaap3
labels:
  - question
assignees: []
created_at: 2023-08-30T11:43:03Z
updated_at: 2025-01-18T05:12:05Z
url: https://github.com/astral-sh/ruff/issues/7005
synced_at: 2026-01-07T13:12:15-06:00
---

# isort custom sections in a monorepo

---

_Issue opened by @jaap3 on 2023-08-30 11:43_

I'm running into the following using `ruff 0.0.286` in a monorepo. 

In the "root" configuration I define a custom section for the monorepo packages:

```toml
[tool.ruff.isort]
lines-between-types = 1
section-order = [
  "future",
  "standard-library",
  "third-party",
  "monorepo",
  "first-party",
  "local-folder",
]

[tool.ruff.isort.sections]
monorepo = [
  "my_bar",
  "my_baz",
  "my_foo",
  "my_qux",
]
```

This works. However, in a specific package's code I want that package to be sorted into the `first-party` section and not lumped into the `monorepo` section.

I tried the following in the config file for a specific package:

```toml
[tool.ruff]
extend = "../../pyproject.toml"

[tool.ruff.isort]
known-first-party = [
  "my_bar",
]
```

But that results in the following warning:

```
warning: One or more modules are part of multiple import sections, including: `my_bar`
```

Is there some way to have this root definition of a custom section, and "promote" a package to first party in the overriding config?

---

_Label `question` added by @charliermarsh on 2023-08-30 13:39_

---

_Comment by @charliermarsh on 2023-08-30 16:06_

Hmm -- thought on this a bit, and I don't think there's a way to achieve what you've described without repeating the `monorepo = [...]` definition in the extended configuration file. If you didn't care about having a separate section for the monorepo, and just wanted each package to be treated as first-party when importing from within itself, and third-party otherwise, you could rely on our `detect-same-package` heuristic which is enabled by default (https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#import-categorization-1).

---

_Comment by @jaap3 on 2023-08-31 07:20_

> If you didn't care about having a separate section for the monorepo [...]

I'm trying to replicate the current setup we have with isort (and flake8), while also leveraging the excellent monorepo support of ruff. Having a section for the monorepo packages themselves is a "must" to prevent a lot of import re-shuffling :).

> repeating the monorepo = [...] definition in the extended configuration file

This is basically what we have been doing, with the lack of configuration inheritance in isort and flake8. I had hoped I could reduce the linting configuration to a simple extend line. For now I will be repeating the monorepo definition in each project. I'm hoping there will be a way to achieve this in a DRY way in the future.

---

Is it possible to make the `known-first-party` setting take precedence over custom section configuration? It is already written that it should be "seen as an escape hatch". Which is exactly how I'm trying to use it, to escape from my own higher level configuration ;).


---

_Comment by @jaap3 on 2023-08-31 08:16_

> repeating the monorepo = [...] definition in the extended configuration file

Turns out I have to redefine **all** the custom sections (the monorepo section is not the only custom section in our case), otherwise I get a `warning: `section-order` contains unknown section [...]` :(

EDIT: Would `isort.extend-sections` be possible?

---

_Comment by @SethMMorton on 2025-01-18 05:12_

I have a similar (but not quite the same) use case. In my organization we have a common `ruff.toml` file that defines in a custom section called `internal-packages` all of the python libraries that our organization maintains so that each project gets correct sorting "for free" if they add `extend = "/path/to/common/ruff.toml"`.

The problem is when we are working on one of those libraries. Ideally, the packages in the library would be identified as first party and be sorted accordingly, but because of the upstream configuration it gets labeled as `internal-packages`. Attempting to override by labeling as `known-first-party` results in the `One or more modules are part of multiple import sections` warning and it does not get sorted correctly. We don't want to simply not include the upstream configuration because it contains other settings, and also lists all the *other* libraries that should be sorted properly.

This behavior makes sense. I am hoping for either:

- A suggested way to solve this problem with current functionality
- An enhancement to tell `ruff` to give precedence to the "most local" section in the case of duplicates (or some other enhancement that results in a similar end result)

---
