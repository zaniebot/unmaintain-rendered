---
number: 10398
title: "Validation differences between `uv pip check` and `uv pip install --strict`"
type: issue
state: closed
author: hpelwintervold
labels:
  - question
assignees: []
created_at: 2025-01-08T16:09:39Z
updated_at: 2025-10-16T15:32:20Z
url: https://github.com/astral-sh/uv/issues/10398
synced_at: 2026-01-10T01:24:53Z
---

# Validation differences between `uv pip check` and `uv pip install --strict`

---

_Issue opened by @hpelwintervold on 2025-01-08 16:09_

**This is a question**

Hello,

I am wondering what differences there are between the validation performed in the Python environment between the commands `uv pip check` and `uv pip install --strict`. I have looked at the documentation and searched in GitHub issues for more detail but cannot find more information.

`uv pip install --strict` states:
```
Validate the Python environment after completing the installation, to detect packages with missing dependencies or other issues
```

`uv pip check` states:
```
Verify installed packages have compatible dependencies
```

My question mostly boils down to, is running `uv pip check` after `uv pip install --strict` redundant?

https://docs.astral.sh/uv/reference/cli/#uv-pip-install
https://docs.astral.sh/uv/reference/cli/#uv-pip-check

---

_Comment by @charliermarsh on 2025-01-08 19:11_

It's basically the same. Internally, both `--strict` and `check` run the same "collect diagnostics" method on the target environment. The only difference is that `--strict` will filter out any diagnostics that are linked to "unrelated" packages. Like if you did `pip install flask --strict`, it would _not_ print diagnostics related to an unrelated package like `black`.

---

_Label `question` added by @Gankra on 2025-01-08 19:15_

---

_Comment by @hpelwintervold on 2025-01-08 22:28_

Thanks, so expanding a little, assuming a virtualenv was created from a number of subsequent `uv pip install --strict` commands, it is guaranteed the environment is in a "correct" state. So running `uv pip check` would be redundant.

But if a non-strict `uv pip install` is performed which installs some corrupted package, subsequent `uv pip install --strict` are not guaranteed to catch it. So `uv pip check` would be required.

Are those statements correct?

---

_Comment by @charliermarsh on 2025-01-08 22:30_

I think that's all correct, yeah.

---

_Comment by @charliermarsh on 2025-01-08 22:31_

`uv pip check` is really fast, so probably makes sense to run it separately if you're looking to enforce those kinds of things.

---

_Comment by @hpelwintervold on 2025-01-09 00:24_

Thanks!

---

_Closed by @hpelwintervold on 2025-01-09 00:24_

---

_Comment by @AaronDMarasco on 2025-10-16 15:31_

For those trying to script some stuff - it will emit a warning, but the return value of `uv pip install --exact --no-deps --strict` is still 0/"happy" even if there is an unresolved dependency.

The `uv pip check` will explicitly return non-zero for shell scripting to catch the problem.

(Note: This is as of `uv` 0.9.2.)

---
