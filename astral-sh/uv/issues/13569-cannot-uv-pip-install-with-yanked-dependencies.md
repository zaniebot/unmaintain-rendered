---
number: 13569
title: "Cannot `uv pip install` with yanked dependencies"
type: issue
state: open
author: vangelem
labels:
  - question
assignees: []
created_at: 2025-05-21T10:05:29Z
updated_at: 2025-09-23T09:18:23Z
url: https://github.com/astral-sh/uv/issues/13569
synced_at: 2026-01-10T01:25:35Z
---

# Cannot `uv pip install` with yanked dependencies

---

_Issue opened by @vangelem on 2025-05-21 10:05_

### Question

Hi astral team,

When trying to install a package created by another team in my company (and stored on our corporate Artficatory) with the command `uv pip install package_name==5.0.1`, I get the following error message:
```bash
Using Python 3.7.9 environment at:
  × No solution found when resolving dependencies:
  ╰─▶ Because futures==3.1.1 was yanked (reason: Does not declare incompatibility with Python 3) and package_name==5.0.1 depends on futures==3.1.1, we can conclude that package_name==5.0.1 cannot be
      used.
      And because you require package_name==5.0.1, we can conclude that your requirements are unsatisfiable.
```
The similar `pip install` command works fine though, `pip` only warns about the yanked package but doesn't stop the installation, so I'm wondering how to deal with this case. Is it possible to force install yanked packages with `uv pip install`? I didn't find anything in the doc or in the tracker about it.

Cheers,
Matt

### Platform

Win11 - MSYS_NT-10.0-22631 3.4.10-87d57229.x86_64 x86_64 Msys

### Version

uv 0.7.6 (7f3e94a09 2025-05-19)

---

_Label `question` added by @vangelem on 2025-05-21 10:05_

---

_Comment by @charliermarsh on 2025-05-21 11:19_

Yeah, if you request the package explicitly, we allow it: `uv pip install futures==3.1.1`

---

_Comment by @vangelem on 2025-05-21 12:14_

Hi @charliermarsh,

Thank you for that. However, the solution isn't ideal for my use case, as the `uv pip install` command is actually automated within a more general package installation tool. In essence, users (who are non-developers) provide a `package_name` and its `version` to the GUI tool, and they expect everything to install smoothly. They aren't even aware of what the dependencies are.

Therefore, the solution would require first extracting the yanked dependencies and then explicitly appending them to the `uv pip install` command. I suppose this might be doable, but it seems like it would complicate the process. Do you think it would be possible to add something like a `--ignore-yanked-dependencies` option to simply skip those during installation?

Cheers,
Matt

---

_Comment by @notatallshaw on 2025-05-21 14:27_

FWIW, pip works here because the dependency (or transative dependency) has a pinning requirement (==).

I assume, similar to pre-releaes, that uv calculates upfront from user requirements what yanks it allows? 

---

_Comment by @rhshadrach-8451 on 2025-06-17 18:55_

My understanding is that yanking a package should not break a previous working install. Since uv fails when a transitive dependency is hard pinned but yanked, this is not the case. Perhaps my understanding is wrong?

Edit: from [PyPI](https://docs.pypi.org/project-management/yanking/):

> A yanked release is a release that is always ignored by an installer, unless it is the only release that matches a [version specifier](https://packaging.python.org/en/latest/specifications/version-specifiers/) (using either == or ===).

---

_Referenced in [astral-sh/uv#13308](../../astral-sh/uv/issues/13308.md) on 2025-07-17 14:49_

---

_Comment by @Winand on 2025-07-28 13:34_

I don't know if I have the same problem or not:

I have a package which includes an optional dependency on a yanked version of pyrfc (this it the latest/last version):
```
[project.optional-dependencies]
sap = [
    "pyrfc==3.3.1",  # https://github.com/SAP-archive/PyRFC/issues/372#issuecomment-2483383627
]
```

If I use this package from its local path (editable) there's no issues with adding `my-package[sap]`:
```
[tool.uv.sources]
my-package = { path = "../my_package", editable = true }
```

But if I publish it to a corporate index there's an error because of the yanked package _pyrfc_:
```
[[tool.uv.index]]
name = "pypi-local"
url = "https://my.corp/repository/pypi-local/simple/"
explicit = true
```

```
uv add my-package[sap] --index pypi-local=https://my.corp/repository/pypi-local/simple/

  × No solution found when resolving dependencies for split (platform_machine == 'x86_64' and sys_platform == 'linux'):
  ╰─▶ Because pyrfc==3.3.1 was yanked (reason: No longer supported, see https://github.com/SAP/PyRFC/issues/372) and my-package[sap]==0.2.0 depends on pyrfc==3.3.1, we can conclude  
      that my-package[sap]==0.2.0 cannot be used.
      And because only my-package[sap]==0.2.0 is available and your project depends on my-package[sap], we can conclude that your project's requirements are unsatisfiable.
```

---

_Comment by @zanieb on 2025-07-28 13:59_

When it's a local package, there's a direct dependency on the yanked version so it's allowed.

When it's a remote package, it's not. You need to allow `"pyrfc==3.3.1"` by adding it as a dependency to the root package you're trying to use `uv add` on.

---

_Comment by @Winand on 2025-09-23 09:18_

@zanieb i've discovered (by accident) that as of version 0.8.20 pyrfc is added successfully as a transitive dependency with `uv add`. So I don't have to add it explicitly.


---
