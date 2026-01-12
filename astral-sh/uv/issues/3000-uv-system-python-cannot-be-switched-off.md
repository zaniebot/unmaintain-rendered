```yaml
number: 3000
title: UV_SYSTEM_PYTHON cannot be switched off
type: issue
state: closed
author: hynek
labels:
  - needs-design
assignees: []
created_at: 2024-04-11T20:45:27Z
updated_at: 2024-04-12T19:51:18Z
url: https://github.com/astral-sh/uv/issues/3000
synced_at: 2026-01-12T15:58:41Z
```

# UV_SYSTEM_PYTHON cannot be switched off

---

_@hynek_

I'm using uv as part of a GitHub Action and have noticed that when the caller sets `UV_SYSTEM_PYTHON=true`, it cannot be undone by locally setting `UV_SYSTEM_PYTHON=false` although the error indicates that false is a valid value. I always get the "system is incompatible with python":

```console
$ env UV_SYSTEM_PYTHON=false uv pip install --python .venv/bin/python ruff
error: the argument '--python <PYTHON>' cannot be used with '--system'
$ uv --version
uv 0.1.31 (7bcca28b1 2024-04-09
```

I have now solved it by unsetting the variable, but I think this should work?



---

_Comment by @charliermarsh on 2024-04-11 20:47_

Will need to look into it... This is something that Clap does automatically (https://github.com/clap-rs/clap), we just specify the mapping between argument and environment variable.

---

_Comment by @hynek on 2024-04-11 20:53_

Yeah I've noticed when looking at #2354. Since this problem will likely manifest itself elsewhere too, I hope this doesn't mean you have a clap rewrite on your plate, too. 😬

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-11 21:02_

---

_Label `needs-design` added by @charliermarsh on 2024-04-11 21:02_

---

_Comment by @charliermarsh on 2024-04-11 21:02_

Hahah don't worry I have enough on my plate to be comfortable passing on that one :)

---

_Comment by @charliermarsh on 2024-04-11 21:11_

I suspect the problem here is actually that Clap thinks both `--system` and `--python` are being passed (even though `--system` is falsey).

---

_Comment by @charliermarsh on 2024-04-11 21:11_

If you omit `--python`, do you see the right behavior? `uv` would default to `.venv` anyway, as long as `--system` is false. (We should fix the interaction here, I'm just confirming the diagnosis...)

---

_Comment by @hynek on 2024-04-11 21:14_

It does, but it has bad interactions with venv too (which is what sent me down this path):

```
nox > Running session tests-3.8(opt_deps=[])
nox > Creating virtual environment (uv) using python3.8 in .nox/tests-3-8-opt_deps
nox > Command uv venv -p python3.8 /home/runner/work/stamina/stamina/.nox/tests-3-8-opt_deps failed with exit code 2:
error: the argument '--python <PYTHON>' cannot be used with '--system'
```

---

_Comment by @charliermarsh on 2024-04-11 21:26_

Okay, I believe I know how to fix this.

---

_Closed by @charliermarsh on 2024-04-12 19:51_

---
