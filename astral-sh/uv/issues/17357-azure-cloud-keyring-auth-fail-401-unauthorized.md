```yaml
number: 17357
title: Azure cloud keyring auth fail - 401 Unauthorized
type: issue
state: open
author: teshanshanuka
labels:
  - bug
assignees: []
created_at: 2026-01-08T08:44:07Z
updated_at: 2026-01-08T08:45:31Z
url: https://github.com/astral-sh/uv/issues/17357
synced_at: 2026-01-10T03:11:36Z
```

# Azure cloud keyring auth fail - 401 Unauthorized

---

_Issue opened by @teshanshanuka on 2026-01-08 08:44_

### Summary

I am authenticating to a private azure pypi feed with `VssSessionToken` + `keyring`, and it does not work (anymore).

I cannot run `uv` commands in my package anymore. I had the bug happening on one PC and the other one was working fine. But now the second one also shows the same error. (At some point I was authenticating with `UV_INDEX_PRIVATE_REGISTRY_PASSWORD` in this PC, and then switched to `keyring`)

`pyproject.toml`
```
[tool.uv.sources]
pvtpkg = { index = "pvtreg" }

[[tool.uv.index]]
name = "pvtreg"
url = "https://VssSessionToken@pkgs.dev.azure.com/myorg/_packaging/Myorg/pypi/simple/"
explicit = true
```

Output of `uv sync` (See the log with `--verbose` at the end)
```
$uv sync
...
Resolved 31 packages in 0.87ms
      Built mypkg @ file:///xxx/mypkg
  × Failed to download `pvtpkg==0.1.1`
  ├─▶ Failed to fetch: `https://pkgs.dev.azure.com/myorg/_packaging/<uuid>/pypi/download/pvtpkg/0.1.1/pvtpkg-0.1.1-py3-none-any.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/myorg/_packaging/<uuid>/pypi/download/pvtpkg/0.1.1/pvtpkg-0.1.1-py3-none-any.whl)
```

I do not know what goes under the hood here, but it is weird to me that it tries not the given url but a slightly different one with UUIDs. 

The token is set in keyring and works with `--extra-index-url`.

```
uv pip install pvtpkg==0.1.1 --extra-index-url="https://$(keyring get $PVTREG_URL VssSessionToken)@pkgs.dev.azure.com/..."
```

**Possible related issue:**: [17144](https://github.com/astral-sh/uv/issues/17144)

---
```
$uv sync --verbose
DEBUG uv 0.9.21 (0dc9556ad 2025-12-30)
DEBUG Acquired shared lock for `/home/myuser/.cache/uv`
DEBUG Found project root: `/home/mypkg`
DEBUG Found workspace root: `/home/mypkg`
DEBUG Adding root workspace member: `/home/mypkg`
DEBUG Adding discovered workspace member: `/home/mypkg/.infra`
DEBUG Acquired exclusive lock for `/home/mypkg`
DEBUG Reading Python requests from version file at `/home/mypkg/.python-version`
DEBUG Using Python request `3.8` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.8`
DEBUG Released lock at `/tmp/uv-7c5426fb63c06d71.lock`
DEBUG Acquired exclusive lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: infra @ file:///home/mypkg/.infra
DEBUG Found workspace root: `/home/mypkg`
DEBUG Found static `pyproject.toml` for: pl-sysctl @ file:///home/mypkg
DEBUG Found workspace root: `/home/mypkg`
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 88 packages in 9ms
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: git-cliff==2.11.0
DEBUG Registry requirement already cached: pyright==1.1.407
DEBUG Registry requirement already cached: pytest==8.3.5
DEBUG Registry requirement already cached: pytest-cov==5.0.0
DEBUG Registry requirement already cached: pytest-dependency==0.6.0
DEBUG Registry requirement already cached: pytest-mock==3.14.1
DEBUG Requirement already installed: typing-extensions==4.13.2
DEBUG Identified uncached distribution: mypkg==0.1.3rc1
DEBUG Identified uncached distribution: mypkg-client==0.1.3rc3
DEBUG Identified uncached distribution: mypkg-proto==0.1.3rc1
DEBUG Identified uncached distribution: pvtpkg==0.1.1
DEBUG Registry requirement already cached: nodeenv==1.10.0
DEBUG Registry requirement already cached: exceptiongroup==1.3.1
DEBUG Registry requirement already cached: iniconfig==2.1.0
DEBUG Requirement already installed: packaging==25.0
DEBUG Registry requirement already cached: pluggy==1.5.0
DEBUG Registry requirement already cached: tomli==2.3.0
DEBUG Registry requirement already cached: coverage==7.6.1
DEBUG Requirement already installed: setuptools==75.3.2
DEBUG Requirement already installed: cryptography==46.0.3
DEBUG Requirement already installed: cffi==1.17.1
DEBUG Requirement already installed: pycparser==2.23
DEBUG Unnecessary package: markupsafe==2.1.5
DEBUG Unnecessary package: bcrypt==5.0.0
DEBUG Unnecessary package: certifi==2026.1.4
DEBUG Unnecessary package: charset-normalizer==3.4.4
DEBUG Unnecessary package: click==8.1.8
DEBUG Unnecessary package: distro==1.9.0
DEBUG Unnecessary package: gevent==24.2.1
DEBUG Unnecessary package: graphlib-backport==1.1.0
DEBUG Unnecessary package: greenlet==3.1.1
DEBUG Unnecessary package: idna==3.11
DEBUG Unnecessary package: importlib-metadata==8.5.0
DEBUG Unnecessary package: jinja2==3.1.6
DEBUG Unnecessary package: paramiko==3.5.1
DEBUG Unnecessary package: pyinfra==3.2
DEBUG Unnecessary package: pynacl==1.6.2
DEBUG Unnecessary package: pyspnego==0.11.2
DEBUG Unnecessary package: python-dateutil==2.9.0.post0
DEBUG Unnecessary package: pywinrm==0.5.0
DEBUG Unnecessary package: requests==2.32.4
DEBUG Unnecessary package: requests-ntlm==1.3.0
DEBUG Unnecessary package: six==1.17.0
DEBUG Unnecessary package: typeguard==4.4.0
DEBUG Unnecessary package: urllib3==2.2.3
DEBUG Unnecessary package: xmltodict==0.15.0
DEBUG Unnecessary package: zipp==3.20.2
DEBUG Unnecessary package: zope-event==5.0
DEBUG Unnecessary package: zope-interface==7.2
DEBUG Found stale response for: https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/mypkg/0.1.3rc1/mypkg-0.1.3rc1-py3-none-any.whl
DEBUG Sending revalidation request for: https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/mypkg/0.1.3rc1/mypkg-0.1.3rc1-py3-none-any.whl
DEBUG Found stale response for: https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/mypkg-client/0.1.3rc3/mypkg_client-0.1.3rc3-py3-none-any.whl
DEBUG Sending revalidation request for: https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/mypkg-client/0.1.3rc3/mypkg_client-0.1.3rc3-py3-none-any.whl
DEBUG Found stale response for: https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/mypkg-proto/0.1.3rc1/mypkg_proto-0.1.3rc1-py3-none-any.whl
DEBUG Sending revalidation request for: https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/mypkg-proto/0.1.3rc1/mypkg_proto-0.1.3rc1-py3-none-any.whl
DEBUG Found stale response for: https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/pvtpkg/0.1.1/pvtpkg-0.1.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/pvtpkg/0.1.1/pvtpkg-0.1.1-py3-none-any.whl
DEBUG No netrc file found
DEBUG Acquired exclusive lock for `credentials store`
DEBUG Released lock at `/home/myuser/.local/share/uv/credentials/credentials.toml.lock`
DEBUG No credentials file found at /home/myuser/.local/share/uv/credentials/credentials.toml
DEBUG Checking keyring for credentials for full URL VssSessionToken@https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/mypkg-client/0.1.3rc3/mypkg_client-0.1.3rc3-py3-none-any.whl
  × Failed to download `pvtpkg==0.1.1`
  ├─▶ Failed to fetch: `https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/pvtpkg/0.1.1/pvtpkg-0.1.1-py3-none-any.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/myorg/_packaging/b43d494e-6bce-1bcf-49d6-b53d6400c096/pypi/download/pvtpkg/0.1.1/pvtpkg-0.1.1-py3-none-any.whl)
  help: `pvtpkg` (v0.1.1) was included because `pl-sysctl` (v0.1.0) depends on `pvtpkg`
DEBUG Released lock at `/home/mypkg/.venv/.lock`
DEBUG Released lock at `/home/myuser/.cache/uv/.lock`
```

### Platform

Linux 6.18.3-arch1-1 x86_64 GNU/Linux

### Version

uv 0.9.21 (0dc9556ad 2025-12-30)

### Python version

Python 3.8.20

---

_Label `bug` added by @teshanshanuka on 2026-01-08 08:44_

---
