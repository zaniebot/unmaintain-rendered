```yaml
number: 6544
title: uv does not appear to parse multiple sources in pyproject.toml
type: issue
state: closed
author: jauderho
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-08-23T19:55:41Z
updated_at: 2024-08-26T14:41:56Z
url: https://github.com/astral-sh/uv/issues/6544
synced_at: 2026-01-12T15:59:05Z
```

# uv does not appear to parse multiple sources in pyproject.toml

---

_@jauderho_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv 0.3.2`

The latest releases of uv looked interesting with lock support so I decided to try converting one of my repos. I currently use pipenv to generate a `requirements.txt`

This is what I have in the Pipfile
```
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[[source]]
url = "https://www.piwheels.org/simple"
verify_ssl = true
name = "piwheels"

[packages]
ansible = "*"
ansible-lint = "*"
jmespath = "*"
pywinrm = {extras = ["kerberos", "credssp"], version = "*"}
requests-credssp = "*"
requests-kerberos = "*"
storops = "*"
wheel = "*"
#mitogen = { git = "https://github.com/mitogen-hq/mitogen.git", editable = true, ref = "bd3cfb4230a485c57a7ad63687ad15249ed341f8" }
#mitogen = { git = "https://github.com/mitogen-hq/mitogen.git", editable = true }
mitogen = "*"

[dev-packages]

[requires]
python_version = "3"
```

Based on what I could figure out, this should be a mostly equivalent `pyproject.toml`
```
[project]
name = "afiles"
version = "1.0.0"
description = "Private Ansible files"
authors = ["Jauder Ho <jauderho@users.noreply.github.com>"]
requires-python = ">=3.12"
dependencies = [
    "ansible",
    "ansible-lint",
    "jmespath",
    "mitogen",
    "pywinrm[kerberos,credssp]",
    "requests-credssp",
    "requests-kerberos",
    "storops",
    "wheel",
]

[[tool.poetry.source]]
name = "pypi"
url = "https://pypi.org/simple"
default = true

[[tool.poetry.source]]
name = "piwheels"
url = "https://www.piwheels.org/simple"

[tool.pip-tools]
generate-hashes = false
```

However, after running `uv lock` and `uv pip compile pyproject.toml -o requirements.txt`, the resulting requirements.txt file does not have what I was expecting to see.

```
-i https://pypi.org/simple
--extra-index-url https://www.piwheels.org/simple
```

Hopefully, someone can point out what I'm missing here. Thanks in advance. 

---

_Comment by @zanieb on 2024-08-23 21:10_

I believe you're looking for #171 — which we want to do but haven't yet.

---

_Label `question` added by @zanieb on 2024-08-23 21:11_

---

_Label `duplicate` added by @zanieb on 2024-08-23 21:11_

---

_Comment by @charliermarsh on 2024-08-26 14:41_

I'm gonna combine with the existing issue -- thanks!

---

_Closed by @charliermarsh on 2024-08-26 14:41_

---
