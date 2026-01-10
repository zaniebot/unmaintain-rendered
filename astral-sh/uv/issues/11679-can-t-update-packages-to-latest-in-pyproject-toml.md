---
number: 11679
title: "Can't update packages to latest in pyproject.toml?"
type: issue
state: closed
author: wrouesnel
labels:
  - question
assignees: []
created_at: 2025-02-20T21:06:39Z
updated_at: 2025-02-23T23:59:53Z
url: https://github.com/astral-sh/uv/issues/11679
synced_at: 2026-01-10T01:25:09Z
---

# Can't update packages to latest in pyproject.toml?

---

_Issue opened by @wrouesnel on 2025-02-20 21:06_

### Question

I've searched the documentation a fair bit and I can't find an answer to this which does what I want.

If I have a list of dependencies like:

```
    "jmespath>=1.0.1",
    "passlib>=1.7.4",
    "netaddr>=0.9.0",
    "pytoml>=0.1.21",
    "ruamel-yaml>=0.18.6",
    "click>=8.1.7",
    "dictdiffer>=0.9.0",
    "pygithub>=2.3.0",
    "pykeepass==4.0.6",
    "ansible>=10.7.0",
```

on some old project, the most common action I want is "upgrade all the packages to the highest possible version". `uv sync --upgrade` seems like it's happy to bump revision numbers, but it won't edit the pyproject.toml file so that's out.

`uv remove <package>` and `uv add <package>` achieves the desired individual effect, but can't be used across the entire dependency tree.

I explicitly *don't* want the lock file to be too different to pyproject.toml - this is application software, not a library, so I want to be specific about what versions we did install (or take advantage of python specific ranges etc.) but periodically (usually when developing a new feature) I want to just say "upgrade all the libraries to where they can be" since that's usually the time that sort of maintenance is best done.

Have I missed something? Is there a command which will do this?

### Platform

Linux 6.8.0-53-generic x86_64 GNU/Linux

### Version

uv 0.5.21

---

_Label `question` added by @wrouesnel on 2025-02-20 21:06_

---

_Comment by @Gankra on 2025-02-21 18:06_

Sounds like you want this feature:

* https://github.com/astral-sh/uv/issues/6298

---

_Comment by @stuartmaxwell on 2025-02-23 07:16_

> Sounds like you want this feature:
> 
>     * [Add a command to read and update (i.e., bump) the project version, e.g., `uv version` #6298](https://github.com/astral-sh/uv/issues/6298)

No, I don't think that is what is being asked. The issue you linked to is about bumping the *project* version, which is often done with third party tools.

What @wrouesnel is asking (and I came here to ask the same thing), is how to update the list of dependencies in the pyproject.toml file to the latest versions. The versions of the packages in the pyproject.toml file remain the same as the day you add them to your project.

The only way I can see to do this, is to run `uv remove <packagename> && uv add --upgrade <packagename>` for each package in the dependency list, which is extremely time-consuming.

---

_Comment by @charliermarsh on 2025-02-23 23:59_

Are you referring to the lower-bound set in `pyproject.toml`? Yeah, we don't have any sort of command to bump those. I don't think you need to, though? It's a lower-bound -- you can upgrade the package without bumping the lower-bound.

I believe this is a duplicate of #6794, though.

---

_Closed by @charliermarsh on 2025-02-23 23:59_

---
