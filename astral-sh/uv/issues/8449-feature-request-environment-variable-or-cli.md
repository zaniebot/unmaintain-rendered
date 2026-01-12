```yaml
number: 8449
title: "Feature Request: Environment variable or CLI parameter to disable manylinux wheels"
type: issue
state: open
author: pythonweb2
labels:
  - wish
  - configuration
assignees: []
created_at: 2024-10-22T13:02:18Z
updated_at: 2024-11-20T00:40:06Z
url: https://github.com/astral-sh/uv/issues/8449
synced_at: 2026-01-12T15:59:26Z
```

# Feature Request: Environment variable or CLI parameter to disable manylinux wheels

---

_@pythonweb2_

UV supports PEP 600 (partially), for disablement of manylinux wheels, which is great. (https://docs.astral.sh/uv/pip/compatibility/#manylinux_compatible-enforcement).

One of the feature requests of pip years ago was to have a CLI option to also enable this behavior (see https://github.com/pypa/pip/issues/3689#issuecomment-237369552).

My request is to add that CLI option (e.g. `--no-manylinux`), or if that is too much, perhaps another option that would probably(?) be easier to implement would be a `UV_NO_MANYLINUX` environment variable that could be set to disable this.

The reason I think this would be helpful, is that when managing a project with a lockfile, there doesn't seem to be a clean way in a Dockerfile or pipeline to first install one package (e.g. no-manylinux), and then install the rest of the dependencies afterwards. It seems like `uv sync --frozen` will create then `venv`, then install the dependencies you have in the lockfile, with no option to install something in the venv before installation.

Thanks!

---

_Comment by @zanieb on 2024-10-22 16:14_

Regarding installing something before syncing, you could do something like this?

```
uv venv
uv pip install <package>
uv sync --frozen --inexact
```

---

_Comment by @pythonweb2 on 2024-10-22 16:37_

Well that is less painful of a workaround than I was expecting. I was thinking I would need to export the dependencies, then install them with `uv pip install`.

---

_Comment by @zanieb on 2024-10-22 16:59_

Well, you'll need to export it if you want the version to come from the lockfile. We're also considering adding an `--only-install-package` in the same way as we have `--no-install-package`.

---

_Comment by @pythonweb2 on 2024-10-22 17:05_

My apologies, I didn't quite understand. Are you saying that running `uv sync --frozen --inexact` won't install the packages that are in the lockfile into the project venv?

---

_Comment by @zanieb on 2024-10-22 17:07_

`uv pip install <package>` won't install the version of `package` defined in your lockfile, if any.

`uv sync --frozen --inexact` will respect your lockfile yes, it just won't remove extraneous packages.

---

_Comment by @pythonweb2 on 2024-10-22 17:10_

Ok, that is more clear now. Yes I think having something like `--only-install-package` would be nice, that way I could add the `no-manylinux` package to my dependencies, and just run `uv sync --only-install-package no-manylinux` before running the `uv sync --frozen` command.

---

_Label `wish` added by @charliermarsh on 2024-11-20 00:40_

---

_Label `configuration` added by @charliermarsh on 2024-11-20 00:40_

---
