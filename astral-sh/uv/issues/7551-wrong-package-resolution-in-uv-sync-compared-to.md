---
number: 7551
title: "Wrong package resolution in `uv sync` compared to `uv pip compile`"
type: issue
state: closed
author: TheRealBecks
labels:
  - question
assignees: []
created_at: 2024-09-19T14:19:04Z
updated_at: 2024-09-20T02:52:13Z
url: https://github.com/astral-sh/uv/issues/7551
synced_at: 2026-01-10T01:24:16Z
---

# Wrong package resolution in `uv sync` compared to `uv pip compile`

---

_Issue opened by @TheRealBecks on 2024-09-19 14:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I use the current version `0.4.12`, but I initially compiled my packages with `0.4.8` and upgraded and synced since then sometimes, but I had an issue with `ainsible-lint` since the introduction of `uv`, but until now I didn't know why. But now I found out that `uv` is installing a very old version of the package.

That's my `pyproject.toml`:
```
[project]
name = "koala"
description = "..."
readme = "README.md"
version = "1.0.0"
requires-python = ">=3.12.5,<3.13"
dependencies = [
  "ansible",
  "ansible-core",
  "ansible-pylibssh",
  "hvac",
  "jmespath",
  "netaddr",
  "paramiko",
]

[project.optional-dependencies]
dev = [
  "ansible-lint",
  "icecream",
  "ruff",
]
```
--> I hardcoded the Python to `>=3.12.5,<3.13`

`uv sync --extra dev --verbose` produces the following versions:
```
uv sync --extra dev --verbose
DEBUG uv 0.4.12
DEBUG Found project root: `/home/mbeckert/Entwicklung/Company/koala/branches/main`
DEBUG No workspace root found, using project root
DEBUG The virtual environment's Python version satisfies `Python >=3.12.5, <3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: koala @ file:///home/mbeckert/Entwicklung/Company/koala/branches/main
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 52 packages in 0.74ms
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: ansible==10.4.0
DEBUG Requirement already installed: ansible-compat==24.9.1
DEBUG Requirement already installed: ansible-core==2.17.4
DEBUG Requirement already installed: ansible-lint==6.8.7
DEBUG Requirement already installed: ansible-pylibssh==1.2.2
DEBUG Requirement already installed: asttokens==2.4.1
DEBUG Requirement already installed: attrs==24.2.0
DEBUG Requirement already installed: bcrypt==4.2.0
DEBUG Requirement already installed: black==24.8.0
DEBUG Requirement already installed: bracex==2.5
DEBUG Requirement already installed: certifi==2024.8.30
DEBUG Requirement already installed: cffi==1.17.1
DEBUG Requirement already installed: charset-normalizer==3.3.2
DEBUG Requirement already installed: click==8.1.7
DEBUG Requirement already installed: colorama==0.4.6
DEBUG Requirement already installed: cryptography==43.0.1
DEBUG Requirement already installed: executing==2.1.0
DEBUG Requirement already installed: filelock==3.16.1
DEBUG Requirement already installed: hvac==2.3.0
DEBUG Requirement already installed: icecream==2.1.3
DEBUG Requirement already installed: idna==3.10
DEBUG Requirement already installed: jinja2==3.1.4
DEBUG Requirement already installed: jmespath==1.0.1
DEBUG Requirement already installed: jsonschema==4.23.0
DEBUG Requirement already installed: jsonschema-specifications==2023.12.1
DEBUG Requirement already installed: markdown-it-py==3.0.0
DEBUG Requirement already installed: markupsafe==2.1.5
DEBUG Requirement already installed: mdurl==0.1.2
DEBUG Requirement already installed: mypy-extensions==1.0.0
DEBUG Requirement already installed: netaddr==1.3.0
DEBUG Requirement already installed: packaging==24.1
DEBUG Requirement already installed: paramiko==3.5.0
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Requirement already installed: platformdirs==4.3.6
DEBUG Requirement already installed: pycparser==2.22
DEBUG Requirement already installed: pygments==2.18.0
DEBUG Requirement already installed: pynacl==1.5.0
DEBUG Requirement already installed: pyyaml==6.0.2
DEBUG Requirement already installed: referencing==0.35.1
DEBUG Requirement already installed: requests==2.32.3
DEBUG Requirement already installed: resolvelib==1.0.1
DEBUG Requirement already installed: rich==13.8.1
DEBUG Requirement already installed: rpds-py==0.20.0
DEBUG Requirement already installed: ruamel-yaml==0.17.40
DEBUG Requirement already installed: ruamel-yaml-clib==0.2.8
DEBUG Requirement already installed: ruff==0.6.5
DEBUG Requirement already installed: six==1.16.0
DEBUG Requirement already installed: subprocess-tee==0.4.2
DEBUG Requirement already installed: urllib3==2.2.3
DEBUG Requirement already installed: wcmatch==9.0
DEBUG Requirement already installed: yamllint==1.35.1
Audited 51 packages in 0.13ms
```
The package resolution `ansible-lint==6.8.7` is wrong as we can see with `uv pip compile --extra dev pyproject.toml` that produces `ansible-lint==24.9.0`:
```
uv pip compile --extra dev pyproject.toml 
Resolved 53 packages in 37ms
# This file was autogenerated by uv via the following command:
#    uv pip compile --extra dev pyproject.toml
ansible==10.4.0
    # via koala (pyproject.toml)
ansible-compat==24.9.1
    # via ansible-lint
ansible-core==2.17.4
    # via
    #   koala (pyproject.toml)
    #   ansible
    #   ansible-compat
    #   ansible-lint
ansible-lint==24.9.0
    # via koala (pyproject.toml)
ansible-pylibssh==1.2.2
    # via koala (pyproject.toml)
asttokens==2.4.1
    # via icecream
attrs==24.2.0
    # via
    #   jsonschema
    #   referencing
bcrypt==4.2.0
    # via paramiko
black==24.8.0
    # via ansible-lint
bracex==2.5
    # via wcmatch
certifi==2024.8.30
    # via requests
cffi==1.17.1
    # via
    #   cryptography
    #   pynacl
charset-normalizer==3.3.2
    # via requests
click==8.1.7
    # via black
colorama==0.4.6
    # via icecream
cryptography==43.0.1
    # via
    #   ansible-core
    #   paramiko
executing==2.1.0
    # via icecream
filelock==3.16.1
    # via ansible-lint
hvac==2.3.0
    # via koala (pyproject.toml)
icecream==2.1.3
    # via koala (pyproject.toml)
idna==3.10
    # via requests
importlib-metadata==8.5.0
    # via ansible-lint
jinja2==3.1.4
    # via ansible-core
jmespath==1.0.1
    # via koala (pyproject.toml)
jsonschema==4.23.0
    # via
    #   ansible-compat
    #   ansible-lint
jsonschema-specifications==2023.12.1
    # via jsonschema
markdown-it-py==3.0.0
    # via rich
markupsafe==2.1.5
    # via jinja2
mdurl==0.1.2
    # via markdown-it-py
mypy-extensions==1.0.0
    # via black
netaddr==1.3.0
    # via koala (pyproject.toml)
packaging==24.1
    # via
    #   ansible-compat
    #   ansible-core
    #   ansible-lint
    #   black
paramiko==3.5.0
    # via koala (pyproject.toml)
pathspec==0.12.1
    # via
    #   ansible-lint
    #   black
    #   yamllint
platformdirs==4.3.6
    # via black
pycparser==2.22
    # via cffi
pygments==2.18.0
    # via
    #   icecream
    #   rich
pynacl==1.5.0
    # via paramiko
pyyaml==6.0.2
    # via
    #   ansible-compat
    #   ansible-core
    #   ansible-lint
    #   yamllint
referencing==0.35.1
    # via
    #   jsonschema
    #   jsonschema-specifications
requests==2.32.3
    # via hvac
resolvelib==1.0.1
    # via ansible-core
rich==13.8.1
    # via ansible-lint
rpds-py==0.20.0
    # via
    #   jsonschema
    #   referencing
ruamel-yaml==0.18.6
    # via ansible-lint
ruamel-yaml-clib==0.2.8
    # via ruamel-yaml
ruff==0.6.5
    # via koala (pyproject.toml)
six==1.16.0
    # via asttokens
subprocess-tee==0.4.2
    # via
    #   ansible-compat
    #   ansible-lint
urllib3==2.2.3
    # via requests
wcmatch==9.0
    # via ansible-lint
yamllint==1.35.1
    # via ansible-lint
zipp==3.20.2
    # via importlib-metadata
```

Where does that version `6.8.7` come from? Is it expected that `uv sync` and `uv pip compile` produce different package versions?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-19 14:28_

---

_Comment by @charliermarsh on 2024-09-19 15:26_

The issue is that `uv sync` tries to resolve for all platforms -- it's materially different than `uv pip compile` which by default only resolves for the _current_ platform. It seems that newer versions of `ansible-lint` don't support Windows and require WSL instead, so we end up backtracking to a version that _does_ work.

You can add:

```toml
[tool.uv]
environments = ["platform_system != 'Windows'"]
```

...to only solve for non-Windows, which will give you the version you expect.

Alternatively, you can add:

```toml
[tool.uv]
environments = ["platform_system != 'Windows'", "platform_system == 'Windows'"]
```

...to solve for non-Windows, then Windows (in that order), which will also give you the version you expect on non-Windows while resolving to a different version on Windows.

---

_Label `question` added by @charliermarsh on 2024-09-19 15:26_

---

_Comment by @TheRealBecks on 2024-09-19 15:48_

That's really interesting to know, thanks for the solution!

The command `uv pip compile` shows the dependency and therefore why a specific version has been choosen. Would it also be an idea to show that a specific system lead to that version (if some system is behind all other systems)?

Edit:
Because until now I compiled several projects, but didn't saw that one system is leading to a downgraded version.

---

_Comment by @charliermarsh on 2024-09-20 02:51_

I don't know if we have a great way to communicate it after the resolution itself unfortunately.

---

_Closed by @charliermarsh on 2024-09-20 02:51_

---

_Comment by @charliermarsh on 2024-09-20 02:52_

In the future we may add a setting (#7190) that could help here, by specifying that you want the latest version on each platform rather than a version that works on all platforms.

---
