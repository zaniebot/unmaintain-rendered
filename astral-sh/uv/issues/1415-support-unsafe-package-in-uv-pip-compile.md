```yaml
number: 1415
title: "Support `--unsafe-package` in `uv pip compile`"
type: issue
state: closed
author: hauntsaninja
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-02-16T02:43:10Z
updated_at: 2024-09-20T22:29:37Z
url: https://github.com/astral-sh/uv/issues/1415
synced_at: 2026-01-12T15:58:27Z
```

# Support `--unsafe-package` in `uv pip compile`

---

_@hauntsaninja_

pip-compile has this flag that's a useful escape hatch. There's some software we use where we care about the specific build. We don't want the build from the index and we don't want it in our lock file, we want to manually manage how it gets installed.

---

_Label `enhancement` added by @charliermarsh on 2024-02-16 02:51_

---

_Label `compatibility` added by @charliermarsh on 2024-02-16 02:51_

---

_Comment by @kummerer94 on 2024-02-18 23:20_

Strong +1 for this feature.

To add another usecase: If you develop on Windows and run `uv pip compile ...`, you will sometimes have dependencies in your output file that are only necessary for Windows (like `pywin32`).

Now, I understand that cross compilation is a problem itself (see [here](https://github.com/jazzband/pip-tools?tab=readme-ov-file#should-i-commit-requirementsin-and-requirementstxt-to-source-control)) and `pip-compile` itself does not solve it. But `--unsafe-package` gets you very far without having to go through the hassle of creating different `requirements.in` files.

---

_Comment by @zanieb on 2024-02-19 00:04_

Can you share a bit more about the workflow for this?

---

_Comment by @hauntsaninja on 2024-02-19 00:30_

Sure! So one of the packages for which we care about the specific build is torch. Now we have some service that's a Python package that has `torch` in its dependencies. We run something that basically boils down to something like `pip-compile package/pyproject.toml --unsafe-package torch --generate-hashes -o requirements.txt` to produce a lock file that does not contain torch. When building the deployment, we `pip install --no-deps -r requirements.txt` and then do some custom thing to install the specific build of torch we want for that deployment.

This is not a critical feature in that we could be unblocked by writing a wrapper script that parses requirements.txt and removes the packages we don't want from it, but it is something pip-compile currently supports

(This is a thing that maybe sounds like it could be solved by using a custom index or maybe by using URL requirements, but neither of those really works for us in practice)

---

_Comment by @hmc-cs-mdrissi on 2024-02-20 23:07_

I originally contributed unsafe-package to pip-compile mostly as an escape hatch. My main use case was handing multiple editable installs. I work in monorepo with several libraries. My requirements.in looks like this,

```
-e file:lib1/
-e file:lib2/
...
```

and I pip compile that with pip-compile requirements.in --unsafe-package lib1 --unsafe-package lib2. This allows resolving dependencies of repository with hashes but without editable requirements. Afterwards we then do `pip install --no-dependencies requirements.txt && pip install --no-dependencies requirements.in`. 

pip requirements.txt does not support a dependency with hashes + editable dependency in same file. This is open [issue](https://github.com/pypa/pip/issues/4995). For my specific usage if requirements.txt file could support both hashed non-editable dependencies and editable dependencies together then I probably wouldn't use it. The feature was made as broader escape hatch though mostly because it was easier. pip-compile already had a few packages marked unsafe by default (setuptools/pip), so extending that logic to user custom unsafe was straight forward.

edit: Name unsafe only comes from it being based on default unsafe packages that existed for many years prior. A more obvious name might be --output-exclude-package (or maybe brief --exclude-package). Here exclusion applies only to output requirements file made. The package is still used as normal during resolution time.

---

_Comment by @gilfree on 2024-02-21 07:04_

If I might add a use case - some packages depend on non-python stuff. For example, cupy depends on cuda. It uses naming scheme like cupy-cuda12x. 

We pip compile our requirements, to pinned dependencies of such packages, but as we have multiple cuda versions we work with, we have a later command that checks the cuda version and installs the relevant package.

We want to have the dependencies of these packages to be pinned and be included in the generated requirements package, but we can't have these cuda-versioned packeges there.

Having a flag that says - "omit cupy from the generated file" will be great. (We can solve this by post processing the requirements file, but I think it will be safer to have this explicit)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-22 16:49_

---

_Comment by @charliermarsh on 2024-02-22 16:49_

I will add.

---

_Comment by @charliermarsh on 2024-02-22 16:50_

I'm tempted to use a different name but...

---

_Comment by @charliermarsh on 2024-02-22 17:09_

If anyone has suggestions, feel free to share, though I reserve the right to just go with `--unsafe-package` and move on :)

---

_Comment by @charliermarsh on 2024-02-22 17:10_

Actually, I'll likely _at least_ use `--unsafe-package` for compatibility.

---

_Assigned to @zanieb by @charliermarsh on 2024-02-22 19:18_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-02-22 19:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-22 19:18_

---

_Unassigned @zanieb by @charliermarsh on 2024-02-22 19:18_

---

_Comment by @nathanjmcdougall on 2024-02-22 20:10_

I think `--exclude` would be a good alias.

---

_Closed by @charliermarsh on 2024-02-23 18:47_

---

_Comment by @msharp9 on 2024-09-20 22:28_

I don't see --unsafe-package in the docs: https://docs.astral.sh/uv/reference/cli/#uv-pip-compile.  I don't get an error when I include the flag and I see the PR.

---

_Comment by @charliermarsh on 2024-09-20 22:29_

It’s “no-emit-package”.

---
