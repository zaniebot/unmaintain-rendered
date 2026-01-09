---
number: 1360
title: "Request: A `uv init` command to generate a pyproject.toml"
type: issue
state: closed
author: imbev
labels:
  - enhancement
  - projects
assignees: []
created_at: 2024-02-15T21:39:39Z
updated_at: 2024-07-19T15:11:49Z
url: https://github.com/astral-sh/uv/issues/1360
synced_at: 2026-01-07T13:12:16-06:00
---

# Request: A `uv init` command to generate a pyproject.toml

---

_Issue opened by @imbev on 2024-02-15 21:39_

A downside of the "pip-tools style" interface is that there is no way to automatically generate a compatible pyproject.toml with the necessary fields. For those using the modern pyproject.toml format, it can be difficult or tedious to create an appropriate pyproject.toml file manually.

I suggest that uv adopt behavior similar to Poetry's [`poetry init`](https://python-poetry.org/docs/cli/#init), so that uv would prompt or allow the user to specify the name, description, author, python version, and build backend (or no build backend) for the project.

---

_Comment by @zanieb on 2024-02-15 21:40_

Hi! Thanks for the feedback. A `poetry init` experience is on our roadmap.

---

_Label `enhancement` added by @zanieb on 2024-02-15 21:40_

---

_Label `project-management` added by @zanieb on 2024-02-15 21:42_

---

_Comment by @DimitarVanguelov on 2024-02-18 16:11_

The overall poetry experience is quite good and ergonomic (though perhaps not always ideal). By that I mean having the other poetry commands as well, not just `poetry init`, would be great:

- `poetry new`
- `poetry add`*
- `poetry shell`
- `poetry run`
- `poetry build`
- `poetry publish`
- etc

There are of course other nice features of poetry, as well as downsides (picking Python versions seems to still be awkward) which I know `uv` is planning on addressing, but having `uv` versions of the above would be a big incentive to switch.

But perhaps the _biggest incentive_ to switch would be poetry-like management of a pyproject.toml file and lock file.

**Kudos on this effort**, will be following closely and looking forward to what it can mean for (the unmitigated train wreck that is) Python dependency/environment management.


'* I know `uv add` might be controversial since it already offers `uv pip install` which is a minimal change from just `pip install` but having `uv add` instead of `uv pip install` as a higher level API would be nice as it would save us a few keystrokes (and we're a lazy bunch) but again, it keeps close to the poetry API that a lot of us appreciate.

---

_Comment by @zanieb on 2024-02-18 17:28_

The full project management workflow provided is on our roadmap. We've put the `pip` compatibility commands in a separate namespace explicitly so we have room for things like `uv install`.

---

_Referenced in [astral-sh/uv#1837](../../astral-sh/uv/issues/1837.md) on 2024-02-22 15:01_

---

_Comment by @alelavelli on 2024-02-28 15:36_

Can you write an example of pyproject.toml for uv? Thank you

---

_Comment by @imbev on 2024-03-02 02:12_

> Can you write an example of pyproject.toml for uv? Thank you

```toml
# pyproject.toml

[project]
name = "my-project"
version = "0.1.0"
description = "my description"
authors = [ {name = "My Name", email = "myemail@example.com"}, ]
requires-python = ">= 3.11"

[build-system]
requires = ["setuptools >= 61.0"]
build-backend = "setuptools.build_meta"
```

See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/

---

_Comment by @wsidl on 2024-04-04 16:58_

Following what are now accepted PEPs, would be great to ensure any project management features adheres to [PEP-621](https://peps.python.org/pep-0621/) (which now refer to [this KB doc](https://packaging.python.org/en/latest/specifications/pyproject-toml/#pyproject-toml-spec)) and [PEP-518](https://peps.python.org/pep-0518/).

This means a project-central pattern for dependencies and other metadata (something [poetry has yet to implement](https://github.com/python-poetry/poetry/issues/7669))

---

_Comment by @zanieb on 2024-04-04 17:02_

Note we don't make use of any particular `pyproject.toml` extensions for `uv` at this time — we just follow the standards. In the future, we'll have a section for configuring `uv` and an `init` command will make more sense.

---

_Comment by @imbev on 2024-04-04 23:11_

> [project]
dependencies = [
  "httpx",
  "gidgethub[httpx]>4.0.0",
  "django>2.1; os_name != 'nt'",
  "django>2.0; os_name == 'nt'",
]

> [project.optional-dependencies]
gui = ["PyQt5"]
cli = [
  "rich",
  "click",
]

> [project]
requires-python = ">= 3.8"

https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#dependencies-and-requirements

I think the dependencies and project.optional-dependencies syntax would ideal for a future "uv add", but `requires-python` is currently relevant for `uv init`. This syntax is already supported by uv for `uv pip compile`.

---

_Referenced in [astral-sh/uv#4511](../../astral-sh/uv/issues/4511.md) on 2024-06-25 15:13_

---

_Referenced in [astral-sh/uv#4791](../../astral-sh/uv/pulls/4791.md) on 2024-07-03 22:24_

---

_Comment by @CruzanCaramele on 2024-07-14 09:26_

@zanieb thanks alot for putting and working on this on your roadmap. 
My thought or noob advice on going in the direction of poetry is that I often find myself confused if I am within poetry's shell or not whenever i run the command poetry run, its just not obvious with poetry. And frankly I question if the shell is even needed.
PDM on the other hand does not have this problem ( I believe it does not spawn a shell)

---

_Closed by @zanieb on 2024-07-19 15:11_

---

_Closed by @zanieb on 2024-07-19 15:11_

---
