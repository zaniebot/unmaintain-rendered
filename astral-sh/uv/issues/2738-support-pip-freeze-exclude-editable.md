```yaml
number: 2738
title: "Support `pip freeze --exclude-editable`"
type: issue
state: closed
author: charlesnicholson
labels:
  - compatibility
assignees: []
created_at: 2024-03-30T17:34:59Z
updated_at: 2024-03-31T17:59:31Z
url: https://github.com/astral-sh/uv/issues/2738
synced_at: 2026-01-12T15:58:40Z
```

# Support `pip freeze --exclude-editable`

---

_@charlesnicholson_

`pip freeze --exclude-editable` emits a requirements file that contains all packages installed in the venv, without the editable installs. This can be handy when shipping a bunch of wheels as a deployment, where the editable-installed source packages in the venv get wheeled and shipped with the frozen requirements file. That way you can ship the first-party wheels with a frozen manifest of all third-party wheels at the time the first-party wheels were created, and the venv can be deterministically recreated.

`uv` on main doesn't support this at time of writing; could it?


---

_Comment by @hmc-cs-mdrissi on 2024-03-31 00:02_

This sounds very similar to [unsafe-package](https://github.com/astral-sh/uv/issues/1415) flag and maybe adding a similar flag for pip freeze would resolve this usage. One common use case for --unsafe-package is to remove editable packages by name (or any other package needed outside of frozen file).

---

_Comment by @charlesnicholson on 2024-03-31 00:23_

The value of `--exclude-editable` is that you don't have to name your packages. In our case it's much more work to collect the names of all of our python packages spread through our repo and submodules so that we can pass them as arguments to `--exclude-editable`. I'd much rather just call `VIRTUAL_ENV=my_venv uv pip freeze --exclude-editable`.

Also, the semantics feel different- we're not excluding our editable installations because they're "unsafe", we're excluding them so that we can get a perfect list of pinned versions of third-party packages that our packages depend on.

So, having `--unsafe-package` does also sound useful but it also feels like a different use case.

---

_Comment by @hmc-cs-mdrissi on 2024-03-31 01:15_

Unsafe is just historical naming. The name history has to do with originally only fixed set of packages were excluded and users could not avoid emitting others. The code evolved from that but intent of excluding them is unrelated to “safety”. The flag’s actual purpose is solely way for user to specify exclusions. I can say this with confidence as I was one who implemented the original pip-compile flag. Uv also calls it separate name that doesn’t mention unsafe.

Yes it does require you to name all of your editables once. My own use case for that flag was solely to remove editables but instead of designing flag for very specific thing it was easier to make a broader flag that had that power.

---

_Label `compatibility` added by @charliermarsh on 2024-03-31 17:49_

---

_Closed by @charliermarsh on 2024-03-31 17:59_

---
