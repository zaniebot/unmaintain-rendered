```yaml
number: 14749
title: "Project dependencies no longer available in JupyterLab launched with `uv run --with jupyter jupyter lab`"
type: issue
state: closed
author: bellkev
labels:
  - bug
assignees: []
created_at: 2025-07-19T22:09:11Z
updated_at: 2025-07-22T12:11:07Z
url: https://github.com/astral-sh/uv/issues/14749
synced_at: 2026-01-12T16:01:56Z
```

# Project dependencies no longer available in JupyterLab launched with `uv run --with jupyter jupyter lab`

---

_@bellkev_

### Summary

My understanding from the uv + Jupyter [integration doc](https://docs.astral.sh/uv/guides/integration/jupyter/) is that running JupyterLab without adding it to the project dependencies like `uv run --with jupyter jupyter lab` should make the project's dependencies available in the JupyterLab environment. I see that that works in 0.7.22 but not in 0.8.0. I strongly suspect that the change comes from https://github.com/astral-sh/uv/pull/14447.

As a minimal example, if I run:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
uv python install 3.10
uv init --bare --python 3.10
uv add boto3
uv run --with jupyter jupyter lab
```

Then with 0.7.22 I'm able to run `import boto3` successfully in a python cell in a notebook, but not in 0.8.0. In version 0.8.0, when I run the following in a cell:
```
import sys
print(sys.executable)
print(sys.path)
```
I get:
```
/Users/kevin/.cache/uv/archive-v0/clAeKvr1eBVfjXfG8R8TX/bin/python
['/Users/kevin/.local/share/uv/python/cpython-3.10.18-macos-aarch64-none/lib/python310.zip', '/Users/kevin/.local/share/uv/python/cpython-3.10.18-macos-aarch64-none/lib/python3.10', '/Users/kevin/.local/share/uv/python/cpython-3.10.18-macos-aarch64-none/lib/python3.10/lib-dynload', '', '/Users/kevin/.cache/uv/archive-v0/clAeKvr1eBVfjXfG8R8TX/lib/python3.10/site-packages']
```

While if I run a command like:
```
!python -c "import sys; print(sys.executable); print(sys.path)"
```

I get:
```
/Users/kevin/.cache/uv/builds-v0/.tmp2PZpfe/bin/python
['', '/Users/kevin/.local/share/uv/python/cpython-3.10.18-macos-aarch64-none/lib/python310.zip', '/Users/kevin/.local/share/uv/python/cpython-3.10.18-macos-aarch64-none/lib/python3.10', '/Users/kevin/.local/share/uv/python/cpython-3.10.18-macos-aarch64-none/lib/python3.10/lib-dynload', '/Users/kevin/.cache/uv/builds-v0/.tmp2PZpfe/lib/python3.10/site-packages', '/Users/kebel/source/uv-test/uv-repro/.venv/lib/python3.10/site-packages', '/Users/kevin/.cache/uv/archive-v0/clAeKvr1eBVfjXfG8R8TX/lib/python3.10/site-packages']
```
The latter contains the desired `.../uv-repro/.venv` project venv, I believe via a `.pth` file where the ephemeral `.cache/uv/builds-v0/.tmp2PZpfe/bin/python` is installed.

I believe the intention is for the kernel to use the same "builds-v0" ephemeral python interpreter location which has the overlay `.pth` file rather than the "archive-v0" `--with` python environment. I see that the kernelspec looks like:
```
{
 "argv": [
  "python",
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "display_name": "Python 3 (ipykernel)",
 "language": "python",
 "metadata": {
  "debugger": true
 }
}
```
The `jupyter_client` [logic](https://github.com/jupyter/jupyter_client/blob/1f36fbf4d0e2e067129552d9e69212f7bf7c0624/jupyter_client/manager.py#L309-L320) is that the "python" command is interpreted as `sys.executable`, and judging by the output of `uv run --with jupyter jupyter troubleshoot`, that is the `--with` environment (`/Users/kevin/.cache/uv/archive-v0/clAeKvr1eBVfjXfG8R8TX/bin/python` in my case). (The `!python` and similar commands have the ephemeral env in the $PATH env var though, and thus seem to use the right python environment.)

### Platform

macOS 15.5 (also tested in Amazon Linux 2023 and debian:bookworm docker image)

### Version

0.8.0

### Python version

3.10.18

---

_Label `bug` added by @bellkev on 2025-07-19 22:09_

---

_Comment by @charliermarsh on 2025-07-20 00:59_

Hmm yeah, I think this is because `jupyter` is in the `--with` environment rather than the environment that we invoke from (which has the requisite `.pth` file pointing to both the `--with` environment and the base environment).


---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-20 01:37_

---

_Comment by @AlexWaygood on 2025-07-20 18:23_

This also appears to have broken mypy invocations such as `uv run --with=mypy mypy` FYI.

(I know the way you're "supposed" to do this is by listing mypy as a dev dependency, but this is often the way I invoke mypy in practice.)

---

_Comment by @charliermarsh on 2025-07-20 18:29_

That one I assume would be fixed by rewriting the shebangs in the final environment.

---

_Comment by @zanieb on 2025-07-21 12:32_

Charlie's referring to https://github.com/astral-sh/uv/issues/13327 which was a bug in the previous implementation as well. Unfortunately that breaks Jupyter.

---

_Closed by @zanieb on 2025-07-22 12:11_

---
