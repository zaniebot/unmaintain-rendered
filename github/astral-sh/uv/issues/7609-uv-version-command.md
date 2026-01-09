---
number: 7609
title: uv version command
type: issue
state: closed
author: mardiros
labels:
  - duplicate
assignees: []
created_at: 2024-09-21T06:32:17Z
updated_at: 2024-09-21T13:45:25Z
url: https://github.com/astral-sh/uv/issues/7609
synced_at: 2026-01-07T13:12:17-06:00
---

# uv version command

---

_Issue opened by @mardiros on 2024-09-21 06:32_

Hi uv team,

I really don't know if an issue is the right place for what I have to say, but actually,
I did not find something more appropriate at the moment.

uv provides command `uv --version` (or `-V`) for short and also a `uv version` subcommand.

that `uv version` command is redundant and does not work as expected (to me).

Since I am trying to migrate one project from poetry to uv, and a poetry early adopter,
I may be biased.

I expect that `uv version` should be here to manage the pyproject.toml version, and,
I don't find any way to deal with my package version using uv.

I would like to have something like this:

```bash
$ uv version --help
Display the version of the project or increment it for release purposes.

Usage: uv version [OPTIONS] <increment>

Options:
      --short Display only the version number

Increments:
  major      Increment major version of the project (foobar 0.1.0 -> foobar 1.0.0)
  minor      Increment minor version of the project (foobar 0.1.0 -> foobar 0.2.0)
  patch      Increment patch version of the project (foobar 0.1.0 -> foobar 0.1.1)
  premajor   Increment major version and add a pre-release identifier (foobar 0.1.0 -> foobar 1.0.0a0)
  preminor   Increment minor version and add a pre-release identifier (foobar 0.1.0 -> foobar 0.2.0a0)
  prepatch   Increment patch version and add a pre-release identifier (foobar 0.1.0 -> foobar 0.1.1a0)
```

The version manipulation can be used at scripting, for example, coming from my personal projects,
I use a [Jusfile](https://github.com/mardiros/blacksmith/blob/main/Justfile#L56) with those simple rules:

```
release major_minor_patch: test gh-pages && changelog
    poetry version {{major_minor_patch}}
    poetry install

publish:
    git commit -am "Release $(poetry version -s)"
    poetry build
    poetry publish
    git push
    git tag "$(poetry version -s)"
    git push origin "$(poetry version -s)"
```


---

_Comment by @charliermarsh on 2024-09-21 12:33_

Are you looking for #6298? Might be the same.

---

_Label `duplicate` added by @charliermarsh on 2024-09-21 12:33_

---

_Comment by @mardiros on 2024-09-21 12:54_

Completly this !

I am confused.
It seems like I'm bad at searching in the issues.

---

_Closed by @zanieb on 2024-09-21 13:45_

---
