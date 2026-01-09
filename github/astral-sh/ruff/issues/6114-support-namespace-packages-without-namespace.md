---
number: 6114
title: "Support namespace packages without `namespace-packages` setting"
type: issue
state: open
author: charliermarsh
labels:
  - core
assignees: []
created_at: 2023-07-27T01:59:44Z
updated_at: 2023-08-05T02:08:15Z
url: https://github.com/astral-sh/ruff/issues/6114
synced_at: 2026-01-07T13:12:15-06:00
---

# Support namespace packages without `namespace-packages` setting

---

_Issue opened by @charliermarsh on 2023-07-27 01:59_

Right now, our module discovery doesn't support namespace packages -- or rather, it requires that projects specify namespace packages explicitly via the [`namespace-packages`](https://beta.ruff.rs/docs/settings/#namespace-packages) setting.

I'd like to remove this setting, and ensure that namespace packages "just work".

One option is to take inspiration from [Mypy](https://mypy.readthedocs.io/en/stable/running_mypy.html#mapping-paths-to-modules), where they effectively mark a directory without a `__init__.py[i]` file as a namespace package if it's within a directory that contains an `__init__.py[i]` file, and provide escape hatches for explicitly specifying package bases.

Another option is to take inspiration [Pyright](https://github.com/microsoft/pyright/blob/main/docs/configuration.md#execution-environment-options), where AFAICT they allow you to specify multiple roots which are effectively your package bases.


---

_Comment by @charliermarsh on 2023-07-27 02:01_

@hauntsaninja - no pressure to respond, but I'm curious if you'd have any advice here based on your experiences in Mypy. This issue is roughly referring to the behavior Mypy has in the [`SourceFinder`](https://github.com/python/mypy/blob/b901d21194400b856a88df62a3d7db871936a50d/mypy/find_sources.py#L130). Right now, we also try to map every Python file to a (package root directory, dot-separated module path), but our logic relies on namespace packages being explicitly specified in our configuration file.

---

_Label `core` added by @charliermarsh on 2023-07-27 02:01_

---

_Comment by @charliermarsh on 2023-07-27 02:08_

It's also plausible that our current approach of requiring namespace packages to be explicitly enumerated is not _that_ bad and fine to preserve for now.

---

_Comment by @hauntsaninja on 2023-07-27 02:24_

mypy's default behaviour isn't very good for actual namespace packages... It's good for "accidental" namespace packages, where you have a large tree and people forget to litter `__init__.py` everywhere.

I'd probably recommend using something like mypy's behaviour with `--explicit-package-bases` as described in https://mypy.readthedocs.io/en/stable/running_mypy.html#mapping-file-paths-to-modules . mypy doesn't use `--explicit-package-bases` by default because it would be too breaking. Having this be cwd dependent is something that's worked reasonably well for mypy, but you may wish to think more carefully about it.

Seems potentially a little annoying to list all namespace packages out, seems preferable to list bases.

---

_Comment by @charliermarsh on 2023-07-27 02:29_

Thank you, that's helpful.

---

_Comment by @hauntsaninja on 2023-07-27 02:47_

An argument against using literally mypy's `--explicit-package-bases` behaviour is that ruff wants to support monorepos with potentially a bunch of nesting, which would cause listing all bases to be tedious. Most users of mypy I've seen are not trying to run mypy in a single invocation on a varied tree. You may wish to explore something that uses the location of pyproject.toml's instead and lets users mix and match settings a little more.

Honestly, maybe mypy should probably just use sys.path of the target interpreter to determine bases. This is very defensible behaviour and unlike ruff mypy does usually have a target interpreter that has the installs needed.

---

_Comment by @charliermarsh on 2023-07-27 02:50_

👍 Yeah monorepo support is important for Ruff. We do currently treat each `pyproject.toml` as creating a new "source root" -- it's a bit complicated but I wrote it out in detail here (not expecting you to read it, only if curious): https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#import-categorization.


---

_Referenced in [astral-sh/ruff#5920](../../astral-sh/ruff/pulls/5920.md) on 2023-07-27 10:47_

---

_Comment by @hmc-cs-mdrissi on 2023-08-05 02:08_

pyright while not python does still call python for sys.path. Beyond helping with namespace packages it also deals helps a lot of import weirdness like handling .pth files/ways sys.path can be modified. 

Pylint's namespace package detection is buggy and I haven't seen an approach that really works besides explicitness/sys.path. Here's a test [repo](https://github.com/hmc-cs-mdrissi/tricky_namespace_package_lint) that's small but is already enough to lead to issues for number of tools due to there being files with same name that are distinguished if you detect namespace packages correctly.

---

_Referenced in [astral-sh/ruff#6474](../../astral-sh/ruff/issues/6474.md) on 2023-08-10 08:27_

---

_Referenced in [astral-sh/ruff#10541](../../astral-sh/ruff/pulls/10541.md) on 2024-03-24 08:43_

---
