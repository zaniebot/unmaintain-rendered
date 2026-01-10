```yaml
number: 10446
title: Use Windows-specific instructions in Jupyter guide
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
  - windows
assignees: []
merged: true
base: main
head: charlie/jup
created_at: 2025-01-09T21:18:22Z
updated_at: 2025-01-09T21:47:07Z
url: https://github.com/astral-sh/uv/pull/10446
synced_at: 2026-01-10T11:44:49Z
```

# Use Windows-specific instructions in Jupyter guide

---

_Pull request opened by @charliermarsh on 2025-01-09 21:18_

## Summary

Closes https://github.com/astral-sh/uv/issues/10407.


---

_Label `documentation` added by @charliermarsh on 2025-01-09 21:18_

---

_Label `windows` added by @charliermarsh on 2025-01-09 21:18_

---

_Marked ready for review by @charliermarsh on 2025-01-09 21:18_

---

_@zanieb reviewed on 2025-01-09 21:25_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:115 on 2025-01-09 21:25_

Just wondering, why aren't we using `uv run` here?

---

_@charliermarsh reviewed on 2025-01-09 21:26_

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:115 on 2025-01-09 21:26_

It’s specifically the “non-project” instructions. We use uv run in the section above.

---

_Review requested from @zanieb by @charliermarsh on 2025-01-09 21:34_

---

_@zanieb reviewed on 2025-01-09 21:36_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:115 on 2025-01-09 21:36_

But... `uv run` should be the recommended cross-platform way to run things in virtual environments too? I'm hesitant, but 🤷‍♀️ we need to improve that experience before I feel strongly about recommending it.

---

_@charliermarsh reviewed on 2025-01-09 21:37_

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:115 on 2025-01-09 21:37_

How does that work...? `uv run --no-project`?

---

_@zanieb reviewed on 2025-01-09 21:38_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:115 on 2025-01-09 21:38_

It should "just work" 

```
❯ uv venv -p 3.13
Using CPython 3.13.1 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install jupyterlab
Resolved 90 packages in 166ms
Prepared 3 packages in 215ms
Installed 90 packages in 234ms
 + anyio==4.8.0
 + appnope==0.1.4
 + argon2-cffi==23.1.0
 + argon2-cffi-bindings==21.2.0
 + arrow==1.3.0
 + asttokens==3.0.0
 + async-lru==2.0.4
 + attrs==24.3.0
 + babel==2.16.0
 + beautifulsoup4==4.12.3
 + bleach==6.2.0
 + certifi==2024.12.14
 + cffi==1.17.1
 + charset-normalizer==3.4.1
 + comm==0.2.2
 + debugpy==1.8.11
 + decorator==5.1.1
 + defusedxml==0.7.1
 + executing==2.1.0
 + fastjsonschema==2.21.1
 + fqdn==1.5.1
 + h11==0.14.0
 + httpcore==1.0.7
 + httpx==0.28.1
 + idna==3.10
 + ipykernel==6.29.5
 + ipython==8.31.0
 + isoduration==20.11.0
 + jedi==0.19.2
 + jinja2==3.1.5
 + json5==0.10.0
 + jsonpointer==3.0.0
 + jsonschema==4.23.0
 + jsonschema-specifications==2024.10.1
 + jupyter-client==8.6.3
 + jupyter-core==5.7.2
 + jupyter-events==0.11.0
 + jupyter-lsp==2.2.5
 + jupyter-server==2.15.0
 + jupyter-server-terminals==0.5.3
 + jupyterlab==4.3.4
 + jupyterlab-pygments==0.3.0
 + jupyterlab-server==2.27.3
 + markupsafe==3.0.2
 + matplotlib-inline==0.1.7
 + mistune==3.1.0
 + nbclient==0.10.2
 + nbconvert==7.16.5
 + nbformat==5.10.4
 + nest-asyncio==1.6.0
 + notebook-shim==0.2.4
 + overrides==7.7.0
 + packaging==24.2
 + pandocfilters==1.5.1
 + parso==0.8.4
 + pexpect==4.9.0
 + platformdirs==4.3.6
 + prometheus-client==0.21.1
 + prompt-toolkit==3.0.48
 + psutil==6.1.1
 + ptyprocess==0.7.0
 + pure-eval==0.2.3
 + pycparser==2.22
 + pygments==2.19.1
 + python-dateutil==2.9.0.post0
 + python-json-logger==3.2.1
 + pyyaml==6.0.2
 + pyzmq==26.2.0
 + referencing==0.35.1
 + requests==2.32.3
 + rfc3339-validator==0.1.4
 + rfc3986-validator==0.1.1
 + rpds-py==0.22.3
 + send2trash==1.8.3
 + setuptools==75.8.0
 + six==1.17.0
 + sniffio==1.3.1
 + soupsieve==2.6
 + stack-data==0.6.3
 + terminado==0.18.1
 + tinycss2==1.4.0
 + tornado==6.4.2
 + traitlets==5.14.3
 + types-python-dateutil==2.9.0.20241206
 + uri-template==1.3.0
 + urllib3==2.3.0
 + wcwidth==0.2.13
 + webcolors==24.11.1
 + webencodings==0.5.1
 + websocket-client==1.8.0
❯ uv run jupyter lab
[I 2025-01-09 15:38:30.398 ServerApp] jupyter_lsp | extension was successfully linked.
[I 2025-01-09 15:38:30.400 ServerApp] jupyter_server_terminals | extension was successfully linked.
[I 2025-01-09 15:38:30.401 ServerApp] jupyterlab | extension was successfully linked.
```

---

_@zanieb reviewed on 2025-01-09 21:39_

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:115 on 2025-01-09 21:39_

`--no-project` is only needed when you explicitly want to disable project discovery, i.e., a `pyproject.toml` exists but should be ignored.

---

_@zanieb approved on 2025-01-09 21:40_

---

_Merged by @charliermarsh on 2025-01-09 21:43_

---

_Closed by @charliermarsh on 2025-01-09 21:43_

---

_Branch deleted on 2025-01-09 21:43_

---

_@charliermarsh reviewed on 2025-01-09 21:43_

---

_Review comment by @charliermarsh on `docs/guides/integration/jupyter.md`:115 on 2025-01-09 21:43_

Yeah, so I think it would be needed if you were in a directory with a pyproject.toml, but wanted to use the uv pip interface. Is that right?

---

_Review comment by @zanieb on `docs/guides/integration/jupyter.md`:115 on 2025-01-09 21:47_

Well, the implicit sync would be fine in that case? You'd still have these extra packages in your environment.

It's only a problem if your `pyproject.toml` isn't valid or you actively don't want it in your environment.

---

_@zanieb reviewed on 2025-01-09 21:47_

---
