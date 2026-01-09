---
number: 10771
title: unexpected items in sdist
type: issue
state: closed
author: cjw296
labels:
  - external
assignees: []
created_at: 2025-01-20T07:26:14Z
updated_at: 2025-01-21T14:13:16Z
url: https://github.com/astral-sh/uv/issues/10771
synced_at: 2026-01-07T13:12:18-06:00
---

# unexpected items in sdist

---

_Issue opened by @cjw296 on 2025-01-20 07:26_

I created [this distribution](https://pypi.org/project/paradigms/0.0.0.dev0/#paradigms-0.0.0.dev0.tar.gz) from [this commit.](https://github.com/simplistix/paradigms/commit/27be6c6dade87f71a9722ea160d90b5bb23a63dd) and have found it contains some expected content:

```bash
$ tar -tzf dist/paradigms-0.0.0.dev0.tar.gz 
paradigms-0.0.0.dev0/.python-version
paradigms-0.0.0.dev0/uv.lock
paradigms-0.0.0.dev0/.idea/.gitignore
paradigms-0.0.0.dev0/.idea/encodings.xml
paradigms-0.0.0.dev0/.idea/misc.xml
paradigms-0.0.0.dev0/.idea/modules.xml
paradigms-0.0.0.dev0/.idea/paradigms.iml
paradigms-0.0.0.dev0/.idea/vcs.xml
paradigms-0.0.0.dev0/.idea/workspace.xml
paradigms-0.0.0.dev0/.idea/inspectionProfiles/Project_Default.xml
paradigms-0.0.0.dev0/.idea/inspectionProfiles/profiles_settings.xml
paradigms-0.0.0.dev0/src/paradigms/__init__.py
paradigms-0.0.0.dev0/src/paradigms/py.typed
paradigms-0.0.0.dev0/.gitignore
paradigms-0.0.0.dev0/.hgignore
paradigms-0.0.0.dev0/README.md
paradigms-0.0.0.dev0/pyproject.toml
paradigms-0.0.0.dev0/PKG-INFO
```

Where would that `.hgignore` have come from? I don't have such a file in the project, if I did it would have from `uv init --lib`, but I don't think that lays down such a file?

The `.idea` directory shouldn't be there, I have it excluded from git in a user-wide `.gitignore`:

```bash
$ cat ~/.gitignore 
.#*
.DS_Store
*.pyc
*.tgz
*.tar.gz
.idea
/.Python
/bin
/include
/lib
```

Why is it being included?

---

_Comment by @FishAlchemist on 2025-01-20 08:46_

https://github.com/simplistix/paradigms/blob/8f96ce34eaefd5258897f9047f9670435db3fad5/pyproject.toml#L12-L14
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
I don't think it's a UV issue; I think it's a ``hatchling`` problem.

> By default, Hatch will respect the first .gitignore or .hgignore file found in your project's root directory or parent directories. 

Ref: https://hatch.pypa.io/latest/config/build/#vcs
Ref: https://github.com/pypa/hatch/issues/304

---

_Label `upstream` added by @konstin on 2025-01-20 08:46_

---

_Closed by @charliermarsh on 2025-01-20 14:26_

---

_Comment by @cjw296 on 2025-01-21 09:22_

Thanks! Is that likely to be the source of the mysterious `.hgignore` too? I've never used mercurial on the above machine, so pretty mystified!

---

_Comment by @charliermarsh on 2025-01-21 14:13_

I think so? We don't have any references to `.hgignore` in our codebase, so it doesn't _seem_ like uv would do that.

---
