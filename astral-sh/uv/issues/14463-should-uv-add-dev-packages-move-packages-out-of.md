---
number: 14463
title: "Should `uv add --dev <packages>` move packages out of `dependencies`?"
type: issue
state: closed
author: stdedos
labels:
  - question
assignees: []
created_at: 2025-07-04T20:06:20Z
updated_at: 2025-07-07T21:26:56Z
url: https://github.com/astral-sh/uv/issues/14463
synced_at: 2026-01-10T01:25:45Z
---

# Should `uv add --dev <packages>` move packages out of `dependencies`?

---

_Issue opened by @stdedos on 2025-07-04 20:06_

### Question

For e.g.

```toml
dependencies = [
    "addict",
    "fabric",
    "mypy>=1.11",
    "pylint~=3.0",
    "questionary",
]

[dependency-groups]
# Empty
```

is there a reason why `uv add --dev mypy pylint` wouldn't simply / with-new-constraints move the packages in question

```toml
dependencies = [
    "addict",
    "fabric",
    "questionary",
]

[dependency-groups]
dev = [
    "mypy>=1.11",
    "pylint~=3.0",
]
```

and instead give

```toml
dependencies = [
    "addict",
    "fabric",
    "mypy>=1.11",
    "pylint~=3.0",
    "questionary",
]

[dependency-groups]
dev = [
    "mypy>=1.16.1",
    "pylint>=3.3.7",
]
```

?

I did a cursory read of https://github.com/astral-sh/uv/issues?q=sort%3Aupdated-desc%20is%3Aissue%20%22--dev%22%20add (first 3 pages) - I couldn't pattern-match my question

"I think `npm i` does this (i.e. moves the package between dependencies and devDependencies)" - but it's being a long time since I last invoked `npm` 😓 

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @stdedos on 2025-07-04 20:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-07 14:49_

---

_Comment by @charliermarsh on 2025-07-07 19:32_

Hmm, I think that wouldn't quite be right here, though I see the appeal. It can actually make sense to duplicate MyPy between the two groups, since it's possible to both (1) install the project without its dev dependencies, and (2) install only the dev dependencies but not the project.

---

_Closed by @charliermarsh on 2025-07-07 19:32_

---

_Comment by @stdedos on 2025-07-07 19:55_

> It can actually make sense to duplicate MyPy between the two groups, since it's possible to both (1) install the project without its dev dependencies, and (2) install only the dev dependencies but not the project.

I am ... getting confused how that makes sense 😅 

Do you mean that "said installations" are exclusive (i.e. you can ONLY pick one of them "at a time")?

>  (1) install the project without its dev dependencies

Ofc, and that's why mypy-by-accident-on-deps is not right to stay there

> (2) install only the dev dependencies but not the project

I use the "install project editable" flow in my development - so I don't understand this.

Would you mind elaborating why one would clone the project, NOT install its deps, but install is dev-deps? 😕 
... or am I missing something?

---

_Comment by @charliermarsh on 2025-07-07 19:58_

Yeah, it's an intentional part of the `dependency-groups` design that you can install _just_ a group and _not_ the project itself. It could apply to, e.g., dev dependencies: you might want to install a development dependency (like Ruff) which doesn't require the project itself to be useful, then run that development dependency over the project. We have the `--only-group` flag for this purpose.

---

_Comment by @stdedos on 2025-07-07 21:26_

I see, thank you for the explanation 😀

You don't need to spend more time - but for the record that still makes little sense to me.
(apart, ofc, from the "Yeah, it's an intentional part of the dependency-groups design" - which is pretty clear 😅)

Idk about Ruff, but for MyPy it doesn't make sense:
Having e.g. `pandas` (or a code+typing package) it _can be_ a requirement for MyPy to finish checking successfully - otherwise said library will "resolve" to `Any` - hence, it won't be type-checkable.

---
