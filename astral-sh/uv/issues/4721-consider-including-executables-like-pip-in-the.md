```yaml
number: 4721
title: "Consider including executables like `pip` in the venv."
type: issue
state: closed
author: dimaqq
labels: []
assignees: []
created_at: 2024-07-02T04:34:55Z
updated_at: 2024-07-02T07:18:53Z
url: https://github.com/astral-sh/uv/issues/4721
synced_at: 2026-01-12T15:58:51Z
```

# Consider including executables like `pip` in the venv.

---

_@dimaqq_

Python's venv installs useful commands/binaries/scripts, in addition to `python` in the virtual env 

```command
🦐/code> python3.12 -m venv with-venv
🦐/code> . with-venv/bin/activate.fish
(with-venv) 🦐/code> pip --version
pip 23.2.1 from /code/with-venv/lib/python3.12/site-packages/pip (python 3.12)
```

Uv venv does not:

```command
🦐/code> uv venv -p python3.12 with-uv
Using Python 3.12.0 interpreter at: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
Creating virtualenv at: with-uv
Activate with: source with-uv/bin/activate.fish
🦐/code> . with-uv/bin/activate.fish
(with-uv) 🦐/code> pip --version
fish: Unknown command: pip
```

While I with both would also include `pydoc`, I think it's important for `uv` to at least implement the same set of core features as `venv` does.

---

_Comment by @dimaqq on 2024-07-02 05:23_

I just realised that `pip` module it not installed during `uv venv`...

I ran `python -m ensure_pip` and that didn't create the `pip` binary in virtualenv either.

Maybe it's a design choice?

---

_Comment by @konstin on 2024-07-02 07:18_

You can include pip with `uv venv --seed`

---

_Closed by @konstin on 2024-07-02 07:18_

---
