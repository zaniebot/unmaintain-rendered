```yaml
number: 2129
title: Support setting empty UV_INDEX_URL
type: issue
state: closed
author: pawamoy
labels:
  - good first issue
  - compatibility
  - configuration
assignees: []
created_at: 2024-03-02T12:56:28Z
updated_at: 2024-03-03T19:57:15Z
url: https://github.com/astral-sh/uv/issues/2129
synced_at: 2026-01-12T15:58:35Z
```

# Support setting empty UV_INDEX_URL

---

_@pawamoy_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Currently, if I set `UV_INDEX_URL` to the empty string with `UV_INDEX_URL=`, `uv` complains with:

```
error: invalid value '' for '--index-url <INDEX_URL>': relative URL without a base
```

To reproduce:

```bash
mkdir repro
cd repro
uv venv --seed
. .venv/bin/activate
UV_INDEX_URL= uv pip install griffe
```

pip supports it:

```
PIP_INDEX_URL= pip install griffe
```

As I understand it, setting the index URL to an empty string through the environment variable means "no index URL specified".

My use-case: I'm using a local index to serve private (insiders) versions of packages. Sometimes, I don't want to use these private versions, but the ones from PyPI.org. So to make this flexible, my shell by default `export UV_INDEX_URL="http://localhost:31411/simple/"`, and when I want to directly use PyPI.org, I do `UV_INDEX_URL= uv pip install ...`. Better yet, I declare a `no-insiders` alias (or similar name) as `alias no-insiders='env PDM_PYPI_URL= PIP_INDEX_URL= UV_INDEX_URL='`, since uv is not the only tool I'm using, and then use it with `no-insiders uv pip install ...`, or `no-insiders any-other-command-installing-packages-with-pip-pdm-uv`.

---

_Label `compatibility` added by @charliermarsh on 2024-03-02 13:18_

---

_Label `configuration` added by @charliermarsh on 2024-03-02 13:18_

---

_Label `good first issue` added by @zanieb on 2024-03-02 14:13_

---

_Closed by @charliermarsh on 2024-03-03 19:57_

---
