---
number: 8518
title: How to config Jfrog api key in pyproject.toml
type: issue
state: closed
author: yuhao-postnl
labels:
  - question
assignees: []
created_at: 2024-10-24T08:15:55Z
updated_at: 2025-05-06T15:42:24Z
url: https://github.com/astral-sh/uv/issues/8518
synced_at: 2026-01-10T01:24:29Z
---

# How to config Jfrog api key in pyproject.toml

---

_Issue opened by @yuhao-postnl on 2024-10-24 08:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hey!!
I'm migrating from `poetry` to `uv` and wondering how to configure API keys for JFrog in the `pyproject.toml` file. Here's an example of my current configuration:
```
[project]
name = "test-dev"
version = "0.0.1"
description = "About test-dev"
authors = []
requires-python = ">=3.9,<4.0"
dependencies = [
    "sageflow>=1.7.0",
    "aiplatform>=0.4.0",
]

[tool.uv.sources]
sageflow = { index = "jfrog" }
aiplatform = { index = "jfrog" }

[[tool.uv.index]]
name = "jfrog"
url = "https://<user_name>:<key>@lpe.jfrog.io/artifactory/api/pypi/<virtual-repo>/simple"


```
For poetry, we have such command to config credentials:
```
export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring 
poetry config http-basic.jfrog <email-address> <JFROG_APIKEY>
```
Is there an equivalent way like `uv config http-basic.jforg`

I checked the doc [here](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#other-indexes), seems no further explanation on how to config jfrog. 

---

_Comment by @zanieb on 2024-10-24 12:36_

You can set the `UV_INDEX_JFROG_USERNAME` and `UV_INDEX_JFROG_PASSWORD` environment variables.

---

_Label `question` added by @zanieb on 2024-10-24 12:36_

---

_Comment by @zanieb on 2024-10-24 12:36_

(We don't recommend writing credentials to your `pyproject.toml`)

---

_Comment by @yuhao-postnl on 2024-10-24 13:27_

> You can set the `UV_INDEX_JFROG_USERNAME` and `UV_INDEX_JFROG_PASSWORD` environment variables.

Thank you for the quick reply! It may be good to mention I used a API-KEY for jfrog. I try to export env variables in my zsh file and source it. (where password I put API key) 
![image](https://github.com/user-attachments/assets/e6f672f7-2680-4a6b-8998-7d097a50b421)


And the I run
```
uv venv
source .venv/bin/activate
uv lock
```
It still pops up with the error:
```
No solution found when resolving dependencies:
  ╰─▶ Because aiplatform was not found in the package registry and your project depends on aiplatform>=0.4.0, we can conclude that your project's
      requirements are unsatisfiable.

      hint: An index URL (https://lpe.jfrog.io/artifactory/api/pypi/a-virtual/simple) could not be queried due to a lack of valid
      authentication credentials (401 Unauthorized).
```
here is my pyproject.toml, do I need to config something else in toml file:
```
[project]
name = "test-dev"
version = "0.0.1"
description = "About test-dev"
authors = []
requires-python = ">=3.9,<4.0"
dependencies = [
    "sageflow>=1.7.0",
    "aiplatform>=0.4.0",
]

[tool.uv.sources]
sageflow = { index = "jfrog" }
aiplatform = { index = "jfrog" }

[[tool.uv.index]]
name = "jfrog"
url = "https://lpe.jfrog.io/artifactory/api/pypi/a-virtual/simple"
```

---

_Closed by @yuhao-postnl on 2024-10-24 13:30_

---

_Reopened by @yuhao-postnl on 2024-10-24 13:35_

---

_Comment by @yuhao-postnl on 2024-10-24 14:30_

Seems I miss export in zsh file, close this issue now. Thanks again!
```
export UV_INDEX_JFROG_USERNAME=
export UV_INDEX_JFROG_PASSWORD
```

---

_Closed by @yuhao-postnl on 2024-10-24 14:30_

---

_Comment by @mstumpf585 on 2025-05-06 15:42_

For anyone else that comes here not wanting to use Env vars I found that you can setup Artifactory with UV using uv.toml see https://docs.astral.sh/uv/configuration/files/

I use the following for my private Artifactory: 

```
[[index]]
name = "private-registry"
url = "https://<email>/:<key here>@<arti link>/artifactory/api/pypi/noctrix-tools-pypi/simple"
```

---
