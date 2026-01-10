---
number: 13443
title: uv 0.7.x and 0.6.x have different behaviors running the same command
type: issue
state: closed
author: andy971022
labels:
  - bug
assignees: []
created_at: 2025-05-14T05:17:05Z
updated_at: 2025-05-16T08:10:38Z
url: https://github.com/astral-sh/uv/issues/13443
synced_at: 2026-01-10T01:25:33Z
---

# uv 0.7.x and 0.6.x have different behaviors running the same command

---

_Issue opened by @andy971022 on 2025-05-14 05:17_

### Summary

## 0.6.x
No error

## 0.7.x
``` bash
ENV UV_KEYRING_PROVIDER subprocess
ENV UV_EXTRA_INDEX_URL our_private_index_url
ENV PATH /root/.local/bin:$PATH

INFO[0036] Running: [/bin/sh -c uv tool install keyring --with keyrings.google-artifactregistry-auth     && uv sync --group group_1 --group  group_2 --extra-index-url $UV_EXTRA_INDEX_URL] 
  × No solution found when resolving dependencies:
  ╰─▶ Because keyring was not found in the package registry and you require
      keyring, we can conclude that your requirements are unsatisfiable.

      hint: An index URL
      (our private index URL)
      could not be queried due to a lack of valid authentication credentials
      (401 Unauthorized).
```

### Platform

python:3.10-slim-bookworm

### Version

uv 0.6.x, uv 0.7.x

### Python version

python 3.10

---

_Label `bug` added by @andy971022 on 2025-05-14 05:17_

---

_Assigned to @jtfmumm by @konstin on 2025-05-14 07:26_

---

_Comment by @jtfmumm on 2025-05-14 07:32_

Would you mind sharing the trace (`-vv`) logs when you run this (for both the 0.6 success and 0.7 failure cases)? 

How are you providing your credentials for the private index and have you checked if they are valid? Prior to 0.7, uv would ignore 401s and fall through to the default index when searching for a package. But because this creates dependency confusion vulnerabilities, uv no longer falls through to the default index when it hits a 401.

---

_Comment by @andy971022 on 2025-05-14 10:10_

Thanks @jtfmumm! Let me share the error trace first

> How are you providing your credentials for the private index and have you checked if they are valid?

This runs by a machine loaded with a service account's credential, which will be configured by `uv tool install keyring --with keyrings.google-artifactregistry-auth`. They should be valid because 0.6.x works without problem.

> uv would ignore 401s and fall through to the default index when searching for a package. But because this creates dependency confusion vulnerabilities, uv no longer falls through to the default index when it hits a 401.

Does it mean that `uv` will look into the private index for dependencies of the private packages?


## uv 0.7.3

From the log it looks like it is looking into the private index for `keyring` which is a public package.

``` bash
DEBUG uv 0.7.3

DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`

TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`

TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`

TRACE stdout output from `ldd --version`: "ld.so (Debian GLIBC 2.36) stable release version 2.36.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.36 in stdout of `ldd --version`

TRACE Searching PATH for executables: python, python3
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Found possible Python executable: /usr/local/bin/python
TRACE Querying interpreter executable at /usr/local/bin/python
DEBUG Found `cpython-3.10.17-linux-x86_64-gnu` at `/usr/local/bin/python` (first executable in the search path)

TRACE Checking lock for `/root/.local/share/uv/tools` at `/root/.local/share/uv/tools/.lock`
DEBUG Acquired lock for `/root/.local/share/uv/tools`
DEBUG Checking for Python environment at `/root/.local/share/uv/tools/keyring`

TRACE Caching credentials for `<PRIVATE_REPO_URL>/simple`
TRACE Caching credentials for `<PRIVATE_REPO_URL>`

DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.17
DEBUG Solving with target Python version: >=3.10.17

TRACE Assigned packages:  
TRACE Chose package for decision: root. remaining choices:  
DEBUG Adding direct dependency: keyring*
DEBUG Adding direct dependency: keyrings-<PRIVATE_PACKAGE>-auth*

TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: keyring. remaining choices: keyrings-<PRIVATE_PACKAGE>-auth

TRACE Fetching metadata for keyring from `<PRIVATE_REPO_URL>/simple/keyring/`
TRACE Fetching metadata for keyrings-<PRIVATE_PACKAGE>-auth from `<PRIVATE_REPO_URL>/simple/keyrings-<PRIVATE_PACKAGE>-auth/`

DEBUG No cache entry for: `<PRIVATE_REPO_URL>/simple/keyring/`
TRACE Sending fresh GET request for `<PRIVATE_REPO_URL>/simple/keyring/`
TRACE Handling request for `<PRIVATE_REPO_URL>/simple/keyring/` with authentication policy auto
TRACE Request for `<PRIVATE_REPO_URL>/simple/keyring/` is missing a password, looking for credentials

TRACE No password in cache for URL `<PRIVATE_REPO_URL>/simple`
TRACE No password in cache for URL `<PRIVATE_REPO_URL>/simple/keyring/`
DEBUG No netrc file found

DEBUG Checking keyring for credentials for index URL oauth2accesstoken@`<PRIVATE_REPO_URL>/simple`
TRACE Checking keyring for URL `<PRIVATE_REPO_URL>/simple`
WARN Failure running `keyring` command: No such file or directory (os error 2)

TRACE Checking keyring for host `<PRIVATE_HOST>`
WARN Failure running `keyring` command: No such file or directory (os error 2)

TRACE No password in cache for realm oauth2accesstoken@`<PRIVATE_HOST>`

DEBUG No cache entry for: `<PRIVATE_REPO_URL>/simple/keyrings-<PRIVATE_PACKAGE>-auth/`
TRACE Sending fresh GET request for `<PRIVATE_REPO_URL>/simple/keyrings-<PRIVATE_PACKAGE>-auth/`
TRACE Handling request for `<PRIVATE_REPO_URL>/simple/keyrings-<PRIVATE_PACKAGE>-auth/` with authentication policy auto
TRACE Request for `<PRIVATE_REPO_URL>/simple/keyrings-<PRIVATE_PACKAGE>-auth/` is missing a password, looking for credentials
TRACE No password in cache for URL `<PRIVATE_REPO_URL>/simple`
TRACE No password in cache for URL `<PRIVATE_REPO_URL>/simple/keyrings-<PRIVATE_PACKAGE>-auth/`

TRACE Skipping fetch of credentials for `<PRIVATE_REPO_URL>/simple`, previous attempt failed
TRACE No password in cache for realm oauth2accesstoken@`<PRIVATE_HOST>`

TRACE Considering retry of HTTP error: Status(401) URL: `<PRIVATE_REPO_URL>/simple/keyrings-<PRIVATE_PACKAGE>-auth/`
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed due to status code: 401 Unauthorized
TRACE Received package metadata for: keyrings-<PRIVATE_PACKAGE>-auth

TRACE Considering retry of HTTP error: Status(401) URL: `<PRIVATE_REPO_URL>/simple/keyring/`
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed due to status code: 401 Unauthorized
TRACE Received package metadata for: keyring

DEBUG Searching for a compatible version of keyring (*)
TRACE Selecting candidate for keyring with range * with 0 remote versions
DEBUG No compatible version found for: keyring

TRACE Resolver derivation tree before reduction:
term root==0a0.dev0
  root==0a0.dev0 depends on keyring*
  keyring not found in the package registry

TRACE Resolver derivation tree after reduction:
term root==0a0.dev0
  root==0a0.dev0 depends on keyring*
  keyring not found in the package registry

× No solution found when resolving dependencies:
╰─▶ Because keyring was not found in the package registry and you require keyring, we can conclude that your requirements are unsatisfiable.

hint: An index URL (`<PRIVATE_REPO_URL>/simple`) could not be queried due to lack of valid authentication credentials (401 Unauthorized).

DEBUG Released lock at `/root/.local/share/uv/tools/.lock`

error building image: error building stage: failed to execute command: waiting for process to exit: exit status 1
```

---

_Comment by @jtfmumm on 2025-05-14 10:26_

Thanks for sharing! uv will look in your private index first and if the package is not found there, will look in the default index (PyPI). On 0.7, uv will not continue to the default if it encounters a 401. 

If in the trace logs for the 0.6 case we see that uv encounters a 401 from your private index and then installs from PyPI instead, your credentials are probably invalid. 

---

_Comment by @andy971022 on 2025-05-14 11:02_


@jtfmumm , the traces for 0.6.x



> Prior to 0.7, uv would ignore 401s and fall through to the default index when searching for a package. But because this creates dependency confusion vulnerabilities, uv no longer falls through to the default index when it hits a 401.

It seems like this is the case. But why is uv checking our private index to install `keyring` , which is a public package. We did not specify the index during the tool install step. The 401 is expected because it is only authenticated after `keyring` is properly installed.

```bash
ENV UV_KEYRING_PROVIDER subprocess
ENV UV_EXTRA_INDEX_URL our_private_index_url
ENV PATH /root/.local/bin:$PATH

uv tool install keyring --with keyrings.google-artifactregistry-auth   && uv sync --group group_1 --group  group_2 --extra-index-url $UV_EXTRA_INDEX_URL
```

```
DEBUG uv 0.6.14

DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version
TRACE stdout output from `ldd --version`: "ld.so (Debian GLIBC 2.36) stable release version 2.36.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.36 in stdout of `ldd --version`

TRACE Searching PATH for executables: python, python3
TRACE Checking PATH directory for interpreters: /usr/local/bin
TRACE Found possible Python executable: /usr/local/bin/python
TRACE Querying interpreter executable at /usr/local/bin/python
DEBUG Found `cpython-3.10.17-linux-x86_64-gnu` at `/usr/local/bin/python`

TRACE Checking lock for `/root/.local/share/uv/tools` at `/root/.local/share/uv/tools/.lock`
DEBUG Acquired lock for `/root/.local/share/uv/tools`
DEBUG Checking for Python environment at `/root/.local/share/uv/tools/keyring`

TRACE Caching credentials for `<PRIVATE_REPO_URL>/simple`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.17
DEBUG Solving with target Python version: >=3.10.17
DEBUG Adding direct dependency: keyring*
DEBUG Adding direct dependency: keyrings-<PRIVATE_PACKAGE>-auth*

TRACE Chose package for decision: keyring
TRACE Fetching metadata for keyring from `<PRIVATE_REPO_URL>/simple/keyring/`
TRACE Fetching metadata for keyrings-<PRIVATE_PACKAGE>-auth from `<PRIVATE_REPO_URL>/simple/keyrings-<PRIVATE_PACKAGE>-auth/`

DEBUG No cache entry for: `<PRIVATE_REPO_URL>/simple/keyring/`
TRACE Sending fresh GET request for `<PRIVATE_REPO_URL>/simple/keyring/`
TRACE Handling request for `<PRIVATE_REPO_URL>/simple/keyring/` with authentication policy auto
TRACE Request for `<PRIVATE_REPO_URL>/simple/keyring/` is missing password; looking for credentials
TRACE No credentials found for `<PRIVATE_REPO_URL>`

DEBUG No netrc file found
DEBUG Checking keyring for credentials for `<PRIVATE_REPO_URL>`
WARN Failure running `keyring` command: No such file or directory (os error 2)

TRACE Checking keyring for host `<PRIVATE_DOMAIN>`
WARN Failure running `keyring` command: No such file or directory (os error 2)

TRACE Considering retry of HTTP error: Status(401) URL: `<PRIVATE_REPO_URL>/simple/keyring/`
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for keyring from `https://pypi.org/simple/keyring/`
TRACE Fetching metadata for keyrings-<PRIVATE_PACKAGE>-auth from `https://pypi.org/simple/keyrings-<PRIVATE_PACKAGE>-auth/`

DEBUG No cache entry for: `https://pypi.org/simple/keyring/`
TRACE Sending fresh GET request for `https://pypi.org/simple/keyring/`
TRACE Request for `https://pypi.org/simple/keyring/` is unauthenticated, proceeding without cache credentials

DEBUG No cache entry for: `https://pypi.org/simple/keyrings-<PRIVATE_PACKAGE>-auth/`
TRACE Sending fresh GET request for `https://pypi.org/simple/keyrings-<PRIVATE_PACKAGE>-auth/`
TRACE Request for `https://pypi.org/simple/keyrings-<PRIVATE_PACKAGE>-auth/` is unauthenticated, proceeding without cache credentials

TRACE Received package metadata for: keyrings-<PRIVATE_PACKAGE>-auth
TRACE Selecting candidate for keyrings-<PRIVATE_PACKAGE>-auth from available versions
TRACE Selected candidate: keyrings-<PRIVATE_PACKAGE>-auth 1.1.2

TRACE Skipping irrelevant files (e.g., win32 executables) for keyring
DEBUG No cache entry for: `https://files.pythonhosted.org/.../keyrings.<PRIVATE_PACKAGE>_auth-1.1.2-py3-none-any.whl.metadata`
TRACE Sending fresh GET request for `https://files.pythonhosted.org/.../keyrings.<PRIVATE_PACKAGE>_auth-1.1.2-py3-none-any.whl.metadata`
TRACE Request for `https://files.pythonhosted.org/.../keyrings.<PRIVATE_PACKAGE>_auth-1.1.2-py3-none-any.whl.metadata` is unauthenticated, proceeding without cache credentials

TRACE Received package metadata for: keyring
TRACE Selecting candidate for package keyring with range *
TRACE Selected candidate: keyring version 25.6.0
DEBUG Selected package: keyring==25.6.0 [compatible] (keyring-25.6.0-py3-none-any.whl)

```

---

_Comment by @jtfmumm on 2025-05-14 11:25_

It’s because uv will check the extra index url first. If you unset the env var and don’t pass it in at the command line, the installation should work. It is possible that a private index has its own version of a public package. 

---

_Comment by @andy971022 on 2025-05-14 11:53_

Thank you so much for the help, @jtfmumm ! Yeah, simply switching the command order works. 

Just another question unrelated to the issue if you would kindly answer.

We manage dependency groups in requirements.txt by `uv add -r requirements.group_x.txt --group group_x`

Say we have 3 groups, where group_1 and group_2 share a common package, and group 3 doesn't (so there is no reason to make that package common across all groups). What would be the best way to update the common package and have the state/dependency versions maintained in the respective requirements.txt files? Currently, I am doing `uv remove common_package` from all the groups and updating the requirements.txt one by one and `uv add -r requirements.group_x.txt --group group_x` back. I wonder if there is a way to update the common package in one go?



This works 
```
ENV PATH /root/.local/bin:$PATH
RUN uv -vv tool install keyring --with keyrings.google-artifactregistry-auth 

ENV UV_KEYRING_PROVIDER subprocess
ENV UV_EXTRA_INDEX_URL our_private_index_url

RUN  uv -vv sync --group group_1 --group group_2 --extra-index-url $UV_EXTRA_INDEX_URL
```

---

_Comment by @jmspereira on 2025-05-14 18:12_

Hey @jtfmumm,

I have an issue similar to @andy971022. 
Locally, in my setup, for the developers, the installation script relies on keyring by passing this parameters to the install:

```
--keyring-provider subprocess
--extra-index-url https://<username>@<private-repository>/repository/<org-name>-pypi/simple
```

For the CI pipeline, those parameters still exist, however we have defined a global uv.toml file with the extra-index-url with the username and password hardcoded, for instance:

```toml
[pip]
extra-index-url = ["https://<username>:<password>@<private-repository>/repository/<org-name>-pypi/simple"]
```

Until uv 0.7.x everything worked fine. However, from 0.7.x our CI pipeline crashes because it tries to use keyring instead of relying on the credentials passed by the uv.toml file.

(This was the setup that we had with pip before migrating to uv)

Do you have any suggestions about this issue? 
Best,

---

_Comment by @jtfmumm on 2025-05-14 19:26_

@jmspereira Would you mind sharing the trace logs from your CI?

---

_Comment by @jmspereira on 2025-05-14 20:31_

```
DEBUG Released lock at `/home/runner/<internal-env-path>/<internal-lib>/.lock`
DEBUG uv 0.7.3
DEBUG Marking explicit source tree for reinstall: `/home/runner/_work/<internal-path>/<internal-lib>`
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /home/runner/<internal-env-path>/<internal-lib>/bin/python3
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/home/runner/<internal-env-path>/<internal-lib>/bin/python3` (active virtual environment)
Using Python 3.12.3 environment at: /home/runner/<internal-env-path>/<internal-lib>
TRACE Checking lock for `/home/runner/<internal-env-path>/<internal-lib>` at `/home/runner/<internal-env-path>/<internal-lib>/.lock`
DEBUG Acquired lock for `/home/runner/<internal-env-path>/<internal-lib>`
TRACE Caching credentials for ***<private-repo>/repository/<org-name>-py/simple
TRACE Caching credentials for ***<private-repo>/repository/<org-name>-pypi
TRACE Caching credentials for https://***@<private-repo>/repository/<org-name>-pypi/simple
TRACE Caching credentials for https://***@<private-repo>/repository/<org-name>-pypi
DEBUG Using request timeout of 30s
DEBUG Found 36 packages in `--find-links` entry: file:///home/runner/_work/<internal-path>/dist
DEBUG Found PEP 621 metadata for /home/runner/_work/<internal-path>/<internal-lib> in `pyproject.toml` (<internal-lib>)
TRACE Performing lookahead for <internal-lib> @ file:///home/runner/_work/<internal-path>/<internal-lib>
DEBUG Found static `pyproject.toml` for: <internal-lib> @ file:///home/runner/_work/<internal-path>/<internal-lib>
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: pytest>=6.2.2
DEBUG Adding direct dependency: pytest-repeat>=0.8.0
DEBUG Adding direct dependency: pytest-xdist>=2.2.1
DEBUG Adding direct dependency: pytest-timeout>=2.0.0
DEBUG Adding direct dependency: pytest-custom-exit-code>=0.3.0
DEBUG Adding direct dependency: pytest-asyncio>=0.23.3
DEBUG Adding direct dependency: pytest-httpx>=0.35.0
DEBUG Adding direct dependency: pytest-benchmark>=3.4.1
DEBUG Adding direct dependency: pytest-randomly>=3.10.3
DEBUG Adding direct dependency: pytest-rerunfailures>=13.0
DEBUG Adding direct dependency: coverage[toml]>=6.2
DEBUG Adding direct dependency: psutil>=5.8.0
DEBUG Adding direct dependency: pytest-cov>=3.0.0
DEBUG Adding direct dependency: anyio>=4.9.0, <4.9.0+
DEBUG Adding direct dependency: certifi>=2025.4.26, <2025.4.26+
DEBUG Adding direct dependency: h11>=0.16.0, <0.16.0+
DEBUG Adding direct dependency: httpcore>=1.0.9, <1.0.9+
DEBUG Adding direct dependency: httpx>=0.28.1, <0.28.1+
DEBUG Adding direct dependency: idna>=3.10, <3.10+
DEBUG Adding direct dependency: sniffio>=1.3.1, <1.3.1+
DEBUG Adding direct dependency: typing-extensions>=4.13.2, <4.13.2+
DEBUG Adding direct dependency: <internal-lib>*
TRACE Fetching metadata for pytest from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: <internal-lib>. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, anyio, certifi, h11, httpcore, httpx, idna, sniffio, typing-extensions
TRACE Fetching metadata for pytest-repeat from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
DEBUG Searching for a compatible version of <internal-lib> @ file:///home/runner/_work/<internal-path>/<internal-lib> (*)
DEBUG Adding transitive dependency for <internal-lib>==3.17.0: httpx>=0.23.0
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0
TRACE Chose package for decision: anyio. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, typing-extensions, certifi, h11, httpcore, httpx, idna, sniffio
TRACE Fetching metadata for pytest-xdist from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Fetching metadata for pytest-timeout from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE Fetching metadata for pytest-custom-exit-code from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE Fetching metadata for pytest-asyncio from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE Fetching metadata for pytest-httpx from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE Fetching metadata for pytest-benchmark from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE Fetching metadata for pytest-randomly from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE Fetching metadata for pytest-rerunfailures from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE Fetching metadata for coverage from https://***@<private-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE Fetching metadata for psutil from https://***@<private-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE Fetching metadata for pytest-cov from https://***@<private-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE Fetching metadata for anyio from https://***@<private-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE Fetching metadata for certifi from https://***@<private-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE Fetching metadata for h11 from https://***@<private-repo>/repository/<org-name>-pypi/simple/h11/
TRACE Fetching metadata for httpcore from https://***@<private-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE Fetching metadata for httpx from https://***@<private-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE Fetching metadata for idna from https://***@<private-repo>/repository/<org-name>-pypi/simple/idna/
TRACE Fetching metadata for sniffio from https://***@<private-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE Fetching metadata for typing-extensions from https://***@<private-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest/
DEBUG No netrc file found
DEBUG Checking keyring for credentials for index URL ***@https://<private-repo>/repository/<org-name>-pypi/simple
TRACE Checking keyring for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-repeat.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-repeat/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-repeat/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-xdist.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-xdist/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-xdist/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-custom-exit-code.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-asyncio.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-httpx.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-httpx/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-httpx/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-benchmark.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-timeout.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-timeout/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-timeout/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-randomly.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-randomly/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-randomly/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-rerunfailures.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/coverage.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/coverage/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/coverage/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/psutil.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/psutil/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/psutil/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-cov.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-cov/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest-cov/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/anyio.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/anyio/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/anyio/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/certifi.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/certifi/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/certifi/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/h11.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/h11/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/h11/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/h11/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/h11/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/h11/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/httpcore.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/httpcore/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/httpcore/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/httpx.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/httpx/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/httpx/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/idna.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/idna/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/idna/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/idna/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/idna/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/idna/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/sniffio.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/sniffio/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/sniffio/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/typing-extensions.rkyv
DEBUG No cache entry for: https://<private-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE Sending fresh GET request for https://<private-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/typing-extensions/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/typing-extensions/ is missing a password, looking for credentials
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple
TRACE No password in cache for URL https://<private-repo>/repository/<org-name>-pypi/simple/typing-extensions/
Traceback (most recent call last):
  File "/home/runner/<internal-env-path>/<internal-lib>/bin/keyring", line 10, in <module>
    sys.exit(main())
             ^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/cli.py", line 216, in main
    return cli.run(argv)
           ^^^^^^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/cli.py", line 120, in run
    return method()
           ^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/cli.py", line 129, in do_get
    credential = getattr(self, f'_get_{self.get_mode}')()
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/cli.py", line 145, in _get_password
    password = get_password(self.service, self.username)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/core.py", line 63, in get_password
    return get_keyring().get_password(service_name, username)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/backends/fail.py", line 28, in get_password
    raise NoKeyringError(msg)
keyring.errors.NoKeyringError: No recommended backend was available. Install a recommended 3rd party backend package; or, install the keyrings.alt package if you want to use the non-recommended backends. See https://pypi.org/project/keyring for details.
TRACE Checking keyring for host <private-repo>
Traceback (most recent call last):
  File "/home/runner/<internal-env-path>/<internal-lib>/bin/keyring", line 10, in <module>
    sys.exit(main())
             ^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/cli.py", line 216, in main
    return cli.run(argv)
           ^^^^^^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/cli.py", line 120, in run
    return method()
           ^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/cli.py", line 129, in do_get
    credential = getattr(self, f'_get_{self.get_mode}')()
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/cli.py", line 145, in _get_password
    password = get_password(self.service, self.username)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/core.py", line 63, in get_password
    return get_keyring().get_password(service_name, username)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-env-path>/<internal-lib>/lib/python3.12/site-packages/keyring/backends/fail.py", line 28, in get_password
    raise NoKeyringError(msg)
keyring.errors.NoKeyringError: No recommended backend was available. Install a recommended 3rd party backend package; or, install the keyrings.alt package if you want to use the non-recommended backends. See https://pypi.org/project/keyring for details.
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Skipping fetch of credentials for https://<private-repo>/repository/<org-name>-pypi/simple, previous attempt failed
TRACE Found cached credentials for realm ***@https://<private-repo>
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-randomly/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-randomly/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-randomly
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-benchmark/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-benchmark
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-custom-exit-code/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-custom-exit-code
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-asyncio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-asyncio
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-xdist/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-xdist/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-xdist
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/certifi/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/certifi/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: certifi
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-httpx/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-httpx/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-httpx
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-repeat/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-repeat/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-repeat
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/psutil/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/psutil/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: psutil
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/anyio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/anyio/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: anyio
DEBUG Searching for a compatible version of anyio (>=4.9.0, <4.9.0+)
TRACE Selecting candidate for anyio with range >=4.9.0, <4.9.0+ with 0 remote versions
DEBUG No compatible version found for: anyio
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-rerunfailures/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-rerunfailures
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/coverage/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/coverage/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: coverage
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/httpx/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/httpx/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: httpx
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-timeout/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-timeout/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-timeout
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-cov/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/pytest-cov/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: pytest-cov
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/httpcore/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/httpcore/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: httpcore
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/sniffio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/sniffio/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: sniffio
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/typing-extensions/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/typing-extensions/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: typing-extensions
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/h11/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/h11/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: h11
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<private-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/idna/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://<private-repo>/repository/<org-name>-pypi/simple/idna/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: idna
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on anyio==4.9.0
  anyio not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on anyio==4.9.0
  anyio not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because anyio was not found in the package registry and you require
      anyio==4.9.0, we can conclude that your requirements are unsatisfiable.

      hint: An index URL
      (https://<private-repo>/repository/<org-name>-pypi/simple) could not
      be queried due to a lack of valid authentication credentials (401
      Unauthorized).
DEBUG Released lock at `/home/runner/<internal-env-path>/<internal-lib>/.lock`
```

Here it is! 

---

_Comment by @jtfmumm on 2025-05-15 06:53_

It look like the index URL uv is using only has a username:

```
TRACE Handling request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest/ with authentication policy auto
TRACE Request for https://****@<private-repo>/repository/<org-name>-pypi/simple/pytest/ is missing a password, looking for credentials
```

If you're passing that URL in via `--extra-index-url` in CI, that should explain it. Before 0.7, uv would have silently fallen back to the default index when encountering a 401, but that behavior is vulnerable to dependency confusion.

---

_Comment by @jmspereira on 2025-05-15 07:10_

No, the URL I am passing in the `--extra-index-url` has both the username and the password. It was impossible to return 401 because there are dependencies that only exist on private-pypi. 

---

_Comment by @jtfmumm on 2025-05-15 07:17_

Can you share the trace logs running the same command with 0.6.17? And could you share the command you're running?

---

_Comment by @jmspereira on 2025-05-15 07:48_

The full command that I am running is:

`uv pip install --keyring-provider subprocess --find-links /home/runner/_work/<internal-path>/dist --extra-index-url https://***@<private-repo>/repository/<org-name>-pypi/simple  . --requirement /home/runner/_work/<internal-path>/test-requirements.txt --requirement /home/runner/_work/<internal-path>/<internal-lib>/resources/requirements/requirements.txt `

Whereas I mentioned here the extra-index-url only has the username, however, there is an extra-index-url defined at a global uv.toml that has both the username and the password.

---

_Comment by @jmspereira on 2025-05-15 07:49_

The trace with uv 0.6.17:

```
DEBUG uv 0.6.17
DEBUG Marking explicit source tree for reinstall: `/home/runner/<internal-path>/<internal-lib>`
warning: Requirements file `resources/requirements/local-requirements.txt` does not contain any dependencies
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /home/runner/<internal-env-path>/bin/python3
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/home/runner/<internal-env-path>/bin/python3` (active virtual environment)
Using Python 3.12.3 environment at: /home/runner/<internal-env-path>
TRACE Checking lock for `/home/runner/<internal-env-path>` at `/home/runner/<internal-env-path>/.lock`
DEBUG Acquired lock for `/home/runner/<internal-env-path>`
TRACE Caching credentials for ***<internal-repo>/repository/<org-name>-pypi/simple
TRACE Caching credentials for ***<internal-repo>/repository/<org-name>-pypi
TRACE Caching credentials for https://***@<internal-repo>/repository/<org-name>-pypi/simple
TRACE Caching credentials for https://***@<internal-repo>/repository/<org-name>-pypi
DEBUG Using request timeout of 30s
DEBUG Found 36 packages in `--find-links` entry: file:///home/runner/_work/<internal-path>/dist
DEBUG Found PEP 621 metadata for /home/runner/<internal-path>/<internal-lib> in `pyproject.toml` (<internal-lib>)
TRACE Performing lookahead for <internal-lib> @ file:///home/runner/<internal-path>/<internal-lib>
DEBUG Found static `pyproject.toml` for: <internal-lib> @ file:///home/runner/<internal-path>/<internal-lib>
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: pytest>=6.2.2
DEBUG Adding direct dependency: pytest-repeat>=0.8.0
DEBUG Adding direct dependency: pytest-xdist>=2.2.1
DEBUG Adding direct dependency: pytest-timeout>=2.0.0
DEBUG Adding direct dependency: pytest-custom-exit-code>=0.3.0
DEBUG Adding direct dependency: pytest-asyncio>=0.23.3
DEBUG Adding direct dependency: pytest-httpx>=0.35.0
DEBUG Adding direct dependency: pytest-benchmark>=3.4.1
DEBUG Adding direct dependency: pytest-randomly>=3.10.3
DEBUG Adding direct dependency: pytest-rerunfailures>=13.0
DEBUG Adding direct dependency: coverage[toml]>=6.2
DEBUG Adding direct dependency: psutil>=5.8.0
DEBUG Adding direct dependency: pytest-cov>=3.0.0
DEBUG Adding direct dependency: anyio>=4.9.0, <4.9.0+
DEBUG Adding direct dependency: certifi>=2025.4.26, <2025.4.26+
DEBUG Adding direct dependency: h11>=0.16.0, <0.16.0+
DEBUG Adding direct dependency: httpcore>=1.0.9, <1.0.9+
DEBUG Adding direct dependency: httpx>=0.28.1, <0.28.1+
DEBUG Adding direct dependency: idna>=3.10, <3.10+
DEBUG Adding direct dependency: sniffio>=1.3.1, <1.3.1+
DEBUG Adding direct dependency: typing-extensions>=4.13.2, <4.13.2+
TRACE Fetching metadata for pytest from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: <internal-lib>. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, anyio, certifi, h11, httpcore, httpx, idna, sniffio, typing-extensions
TRACE Fetching metadata for pytest-repeat from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
DEBUG Searching for a compatible version of <internal-lib> @ file:///home/runner/<internal-path>/<internal-lib> (*)
DEBUG Adding transitive dependency for <internal-lib>==3.17.0: httpx>=0.23.0
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0
TRACE Chose package for decision: anyio. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, typing-extensions, certifi, h11, httpcore, httpx, idna, sniffio
TRACE Fetching metadata for pytest-xdist from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Fetching metadata for pytest-timeout from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE Fetching metadata for pytest-custom-exit-code from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE Fetching metadata for pytest-asyncio from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE Fetching metadata for pytest-httpx from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE Fetching metadata for pytest-benchmark from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE Fetching metadata for pytest-randomly from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE Fetching metadata for pytest-rerunfailures from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE Fetching metadata for coverage from https://***@<internal-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE Fetching metadata for psutil from https://***@<internal-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE Fetching metadata for pytest-cov from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE Fetching metadata for anyio from https://***@<internal-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE Fetching metadata for certifi from https://***@<internal-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE Fetching metadata for h11 from https://***@<internal-repo>/repository/<org-name>-pypi/simple/h11/
TRACE Fetching metadata for httpcore from https://***@<internal-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE Fetching metadata for httpx from https://***@<internal-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE Fetching metadata for idna from https://***@<internal-repo>/repository/<org-name>-pypi/simple/idna/
TRACE Fetching metadata for sniffio from https://***@<internal-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE Fetching metadata for typing-extensions from https://***@<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
...
```

---

_Comment by @jmspereira on 2025-05-15 09:32_

```
...
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-repeat.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-timeout.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-custom-exit-code.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-asyncio.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-xdist.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-httpx.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-benchmark.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-randomly.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-rerunfailures.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/coverage.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/coverage/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/coverage/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/psutil.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/psutil/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/psutil/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-cov.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/anyio.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/anyio/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/anyio/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/certifi.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/certifi/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/certifi/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/h11.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/h11/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/h11/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/h11/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/h11/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/httpcore.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/httpcore/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/httpcore/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/httpx.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/httpx/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/httpx/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/idna.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/idna/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/idna/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/idna/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/idna/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/sniffio.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/sniffio/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/sniffio/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/typing-extensions.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-timeout/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-timeout from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-timeout.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-repeat/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-repeat from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-repeat.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-xdist/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-xdist from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-rerunfailures/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-rerunfailures from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-xdist.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/ already contains username and password
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-rerunfailures.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-randomly/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-randomly from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-randomly.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-asyncio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-asyncio from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-asyncio.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/psutil/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/psutil/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for psutil from ***<internal-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/psutil.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/psutil/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/psutil/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/psutil/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/coverage/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/coverage/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for coverage from ***<internal-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/coverage.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/coverage/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/coverage/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/coverage/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-benchmark/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-benchmark from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-benchmark.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-httpx/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-httpx from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-httpx.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/certifi/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/certifi/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for certifi from ***<internal-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/certifi.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/certifi/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/certifi/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/certifi/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-xdist/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-xdist/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-xdist from https://pypi.org/simple/pytest-xdist/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-xdist.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-xdist/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-xdist/
TRACE Handling request for https://pypi.org/simple/pytest-xdist/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-xdist/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-xdist/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-xdist/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-custom-exit-code/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-custom-exit-code from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-custom-exit-code.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/h11/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/h11/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for h11 from ***<internal-repo>/repository/<org-name>-pypi/simple/h11/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/h11.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/h11/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/h11/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/h11/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/h11/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-cov/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-cov from ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pytest-cov.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/httpcore/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/httpcore/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for httpcore from ***<internal-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/httpcore.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/httpcore/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/httpcore/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/httpcore/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/anyio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/anyio/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for anyio from ***<internal-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/anyio.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/anyio/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/anyio/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/anyio/ already contains username and password
TRACE Cached request https://pypi.org/simple/pytest-xdist/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: pytest-xdist
TRACE Selecting candidate for pytest-xdist with range >=2.2.1 with 69 remote versions
TRACE Found candidate for package pytest-xdist with range >=2.2.1 after 1 steps: 3.6.1 version
TRACE Returning candidate for package pytest-xdist with range >=2.2.1 after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-xdist/3.6.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl.metadata
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-repeat/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-repeat/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-repeat from https://pypi.org/simple/pytest-repeat/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-repeat.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-repeat/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-repeat/
TRACE Handling request for https://pypi.org/simple/pytest-repeat/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-repeat/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-repeat/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-repeat/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/typing-extensions/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for typing-extensions from ***<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/typing-extensions.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/ already contains username and password
TRACE Cached request https://pypi.org/simple/pytest-repeat/ is storable because its response has a 'public' cache-control directive
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/sniffio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/sniffio/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for sniffio from ***<internal-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/sniffio.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/sniffio/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/sniffio/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/sniffio/ already contains username and password
TRACE Received package metadata for: pytest-repeat
TRACE Selecting candidate for pytest-repeat with range >=0.8.0 with 13 remote versions
TRACE Found candidate for package pytest-repeat with range >=0.8.0 after 1 steps: 0.9.4 version
TRACE Returning candidate for package pytest-repeat with range >=0.8.0 after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-repeat/0.9.4-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl.metadata
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest from https://pypi.org/simple/pytest/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest/
TRACE Handling request for https://pypi.org/simple/pytest/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest/
TRACE Cached request https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pytest-xdist==3.6.1
TRACE Cached request https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pytest-repeat==0.9.4
TRACE Cached request https://pypi.org/simple/pytest/ is storable because its response has a 'public' cache-control directive
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-rerunfailures/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-rerunfailures/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-rerunfailures from https://pypi.org/simple/pytest-rerunfailures/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-rerunfailures.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-rerunfailures/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-rerunfailures/
TRACE Handling request for https://pypi.org/simple/pytest-rerunfailures/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-rerunfailures/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-rerunfailures/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-rerunfailures/
TRACE Received package metadata for: pytest
TRACE Selecting candidate for pytest with range >=6.2.2 with 183 remote versions
TRACE Found candidate for package pytest with range >=6.2.2 after 1 steps: 8.3.5 version
TRACE Returning candidate for package pytest with range >=6.2.2 after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest/8.3.5-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl.metadata
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-timeout/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-timeout/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-timeout from https://pypi.org/simple/pytest-timeout/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-timeout.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-timeout/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-timeout/
TRACE Handling request for https://pypi.org/simple/pytest-timeout/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-timeout/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-timeout/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-timeout/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/idna/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/idna/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for idna from ***<internal-repo>/repository/<org-name>-pypi/simple/idna/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/idna.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/idna/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/idna/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/idna/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/idna/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/certifi/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/certifi/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for certifi from https://pypi.org/simple/certifi/
TRACE Cached request https://pypi.org/simple/certifi/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
TRACE Received package metadata for: certifi
TRACE Selecting candidate for certifi with range >=2025.4.26, <2025.4.26+ with 63 remote versions
TRACE Found candidate for package certifi with range >=2025.4.26, <2025.4.26+ after 1 steps: 2025.4.26 version
TRACE Returning candidate for package certifi with range >=2025.4.26, <2025.4.26+ after 1 steps
TRACE Cached request https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/4a/7e/3db2bd1b1f9e95f7cddca6d6e75e2f2bd9f51b1246e546d88addca0106bd/certifi-2025.4.26-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4a/7e/3db2bd1b1f9e95f7cddca6d6e75e2f2bd9f51b1246e546d88addca0106bd/certifi-2025.4.26-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: certifi==2025.4.26
TRACE Received built distribution metadata for: pytest==8.3.5
TRACE Cached request https://pypi.org/simple/pytest-rerunfailures/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/pytest-timeout/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: pytest-rerunfailures
TRACE Selecting candidate for pytest-rerunfailures with range >=13.0 with 34 remote versions
TRACE Found candidate for package pytest-rerunfailures with range >=13.0 after 1 steps: 15.1 version
TRACE Returning candidate for package pytest-rerunfailures with range >=13.0 after 1 steps
TRACE Received package metadata for: pytest-timeout
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-rerunfailures/15.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl.metadata
TRACE Selecting candidate for pytest-timeout with range >=2.0.0 with 23 remote versions
TRACE Found candidate for package pytest-timeout with range >=2.0.0 after 1 steps: 2.4.0 version
TRACE Returning candidate for package pytest-timeout with range >=2.0.0 after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-timeout/2.4.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl.metadata
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/httpx/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/httpx/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for httpx from ***<internal-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/httpx.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/httpx/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/httpx/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/httpx/ already contains username and password
TRACE Cached request https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pytest-rerunfailures==15.1
TRACE Cached request https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pytest-timeout==2.4.0
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/coverage/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/coverage/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for coverage from https://pypi.org/simple/coverage/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/coverage.rkyv
DEBUG No cache entry for: https://pypi.org/simple/coverage/
TRACE Sending fresh GET request for https://pypi.org/simple/coverage/
TRACE Handling request for https://pypi.org/simple/coverage/ with authentication policy auto
TRACE Request for https://pypi.org/simple/coverage/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/coverage/
TRACE Attempting unauthenticated request for https://pypi.org/simple/coverage/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/psutil/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/psutil/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for psutil from https://pypi.org/simple/psutil/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/psutil.rkyv
DEBUG No cache entry for: https://pypi.org/simple/psutil/
TRACE Sending fresh GET request for https://pypi.org/simple/psutil/
TRACE Handling request for https://pypi.org/simple/psutil/ with authentication policy auto
TRACE Request for https://pypi.org/simple/psutil/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/psutil/
TRACE Attempting unauthenticated request for https://pypi.org/simple/psutil/
TRACE Cached request https://pypi.org/simple/coverage/ is storable because its response has a 'public' cache-control directive
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-cov/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-cov/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-cov from https://pypi.org/simple/pytest-cov/
TRACE Cached request https://pypi.org/simple/psutil/ is storable because its response has a 'public' cache-control directive
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-cov.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-cov/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-cov/
TRACE Handling request for https://pypi.org/simple/pytest-cov/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-cov/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-cov/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-cov/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/anyio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/anyio/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for anyio from https://pypi.org/simple/anyio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/anyio.rkyv
DEBUG No cache entry for: https://pypi.org/simple/anyio/
TRACE Sending fresh GET request for https://pypi.org/simple/anyio/
TRACE Handling request for https://pypi.org/simple/anyio/ with authentication policy auto
TRACE Request for https://pypi.org/simple/anyio/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/anyio/
TRACE Attempting unauthenticated request for https://pypi.org/simple/anyio/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/idna/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/idna/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for idna from https://pypi.org/simple/idna/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-custom-exit-code/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-custom-exit-code/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-custom-exit-code from https://pypi.org/simple/pytest-custom-exit-code/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-custom-exit-code.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-custom-exit-code/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-custom-exit-code/
TRACE Handling request for https://pypi.org/simple/pytest-custom-exit-code/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-custom-exit-code/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-custom-exit-code/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-custom-exit-code/
TRACE Cached request https://pypi.org/simple/idna/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/idna/
TRACE Received package metadata for: idna
TRACE Received package metadata for: psutil
TRACE Selecting candidate for idna with range >=3.10, <3.10+ with 32 remote versions
TRACE Found candidate for package idna with range >=3.10, <3.10+ after 1 steps: 3.10 version
TRACE Returning candidate for package idna with range >=3.10, <3.10+ after 1 steps
TRACE Selecting candidate for psutil with range >=5.8.0 with 95 remote versions
TRACE Found candidate for package psutil with range >=5.8.0 after 1 steps: 7.0.0 version
TRACE Returning candidate for package psutil with range >=5.8.0 after 1 steps
TRACE Cached request https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: idna==3.10
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/psutil/7.0.0-c0164a3d3dead0b8.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/sniffio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/sniffio/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for sniffio from https://pypi.org/simple/sniffio/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/typing-extensions/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/typing-extensions/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for typing-extensions from https://pypi.org/simple/typing-extensions/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/h11/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/h11/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for h11 from https://pypi.org/simple/h11/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/httpcore/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/httpcore/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for httpcore from https://pypi.org/simple/httpcore/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/httpx/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/httpx/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for httpx from https://pypi.org/simple/httpx/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-asyncio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-asyncio/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-asyncio from https://pypi.org/simple/pytest-asyncio/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-httpx/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-httpx/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-httpx from https://pypi.org/simple/pytest-httpx/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-randomly/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-randomly/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-randomly from https://pypi.org/simple/pytest-randomly/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pytest-benchmark/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pytest-benchmark/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pytest-benchmark from https://pypi.org/simple/pytest-benchmark/
TRACE Cached request https://pypi.org/simple/pytest-cov/ is storable because its response has a 'public' cache-control directive
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/sniffio.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sniffio/
TRACE Sending fresh GET request for https://pypi.org/simple/sniffio/
TRACE Handling request for https://pypi.org/simple/sniffio/ with authentication policy auto
TRACE Request for https://pypi.org/simple/sniffio/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sniffio/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sniffio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/h11.rkyv
DEBUG No cache entry for: https://pypi.org/simple/h11/
TRACE Sending fresh GET request for https://pypi.org/simple/h11/
TRACE Handling request for https://pypi.org/simple/h11/ with authentication policy auto
TRACE Request for https://pypi.org/simple/h11/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/h11/
TRACE Attempting unauthenticated request for https://pypi.org/simple/h11/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/httpcore.rkyv
DEBUG No cache entry for: https://pypi.org/simple/httpcore/
TRACE Sending fresh GET request for https://pypi.org/simple/httpcore/
TRACE Handling request for https://pypi.org/simple/httpcore/ with authentication policy auto
TRACE Request for https://pypi.org/simple/httpcore/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/httpcore/
TRACE Attempting unauthenticated request for https://pypi.org/simple/httpcore/
TRACE Cached request https://pypi.org/simple/typing-extensions/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
TRACE Received package metadata for: typing-extensions
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/httpx.rkyv
DEBUG No cache entry for: https://pypi.org/simple/httpx/
TRACE Sending fresh GET request for https://pypi.org/simple/httpx/
TRACE Handling request for https://pypi.org/simple/httpx/ with authentication policy auto
TRACE Request for https://pypi.org/simple/httpx/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/httpx/
TRACE Attempting unauthenticated request for https://pypi.org/simple/httpx/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-asyncio.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-asyncio/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-asyncio/
TRACE Handling request for https://pypi.org/simple/pytest-asyncio/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-asyncio/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-asyncio/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-asyncio/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-httpx.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-httpx/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-httpx/
TRACE Handling request for https://pypi.org/simple/pytest-httpx/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-httpx/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-httpx/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-httpx/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-randomly.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-randomly/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-randomly/
TRACE Handling request for https://pypi.org/simple/pytest-randomly/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-randomly/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-randomly/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-randomly/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pytest-benchmark.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytest-benchmark/
TRACE Sending fresh GET request for https://pypi.org/simple/pytest-benchmark/
TRACE Handling request for https://pypi.org/simple/pytest-benchmark/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pytest-benchmark/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytest-benchmark/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytest-benchmark/
TRACE Received package metadata for: coverage
TRACE Selecting candidate for typing-extensions with range >=4.13.2, <4.13.2+ with 44 remote versions
TRACE Found candidate for package typing-extensions with range >=4.13.2, <4.13.2+ after 1 steps: 4.13.2 version
TRACE Returning candidate for package typing-extensions with range >=4.13.2, <4.13.2+ after 1 steps
TRACE Received package metadata for: pytest-cov
TRACE Cached request https://files.pythonhosted.org/packages/8b/54/b1ae86c0973cc6f0210b53d508ca3641fb6d0c56823f288d108bc7ab3cc8/typing_extensions-4.13.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8b/54/b1ae86c0973cc6f0210b53d508ca3641fb6d0c56823f288d108bc7ab3cc8/typing_extensions-4.13.2-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: typing-extensions==4.13.2
TRACE Selecting candidate for pytest-cov with range >=3.0.0 with 49 remote versions
TRACE Found candidate for package pytest-cov with range >=3.0.0 after 1 steps: 6.1.1 version
TRACE Returning candidate for package pytest-cov with range >=3.0.0 after 1 steps
TRACE Cached request https://pypi.org/simple/anyio/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-cov/6.1.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl.metadata
TRACE Received package metadata for: anyio
DEBUG Searching for a compatible version of anyio (>=4.9.0, <4.9.0+)
TRACE Selecting candidate for anyio with range >=4.9.0, <4.9.0+ with 62 remote versions
TRACE Found candidate for package anyio with range >=4.9.0, <4.9.0+ after 1 steps: 4.9.0 version
TRACE Returning candidate for package anyio with range >=4.9.0, <4.9.0+ after 1 steps
DEBUG Selecting: anyio==4.9.0 [compatible] (anyio-4.9.0-py3-none-any.whl)
TRACE Received built distribution metadata for: psutil==7.0.0
TRACE Selecting candidate for anyio with range >=4.9.0, <4.9.0+ with 62 remote versions
TRACE Found candidate for package anyio with range >=4.9.0, <4.9.0+ after 1 steps: 4.9.0 version
TRACE Returning candidate for package anyio with range >=4.9.0, <4.9.0+ after 1 steps
TRACE Cached request https://pypi.org/simple/pytest-custom-exit-code/ is storable because its response has a 'public' cache-control directive
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/anyio/4.9.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl.metadata
TRACE Received package metadata for: pytest-custom-exit-code
TRACE Selecting candidate for pytest-custom-exit-code with range >=0.3.0 with 6 remote versions
TRACE Found candidate for package pytest-custom-exit-code with range >=0.3.0 after 1 steps: 0.3.0 version
TRACE Returning candidate for package pytest-custom-exit-code with range >=0.3.0 after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-custom-exit-code/0.3.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl.metadata
TRACE Cached request https://pypi.org/simple/pytest-asyncio/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/httpx/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/h11/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/pytest-benchmark/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/httpcore/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/pytest-randomly/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/pytest-httpx/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/sniffio/ is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: pytest-asyncio
TRACE Received package metadata for: httpx
TRACE Received package metadata for: h11
TRACE Received package metadata for: pytest-benchmark
TRACE Received package metadata for: httpcore
TRACE Received package metadata for: pytest-randomly
TRACE Received package metadata for: pytest-httpx
TRACE Received package metadata for: sniffio
TRACE Received built distribution metadata for: pytest-cov==6.1.1
TRACE Received built distribution metadata for: anyio==4.9.0
TRACE Selecting candidate for pytest-asyncio with range >=0.23.3 with 66 remote versions
TRACE Found candidate for package pytest-asyncio with range >=0.23.3 after 2 steps: 0.26.0 version
TRACE Returning candidate for package pytest-asyncio with range >=0.23.3 after 2 steps
TRACE Selecting candidate for httpx with range >=0.28.1, <0.28.1+ with 71 remote versions
TRACE Found candidate for package httpx with range >=0.28.1, <0.28.1+ after 1 steps: 0.28.1 version
DEBUG Adding transitive dependency for anyio==4.9.0: idna>=2.8
TRACE Returning candidate for package httpx with range >=0.28.1, <0.28.1+ after 1 steps
DEBUG Adding transitive dependency for anyio==4.9.0: sniffio>=1.1
DEBUG Adding transitive dependency for anyio==4.9.0: typing-extensions{python_full_version < '3.13'}>=4.5
TRACE Selecting candidate for h11 with range >=0.16.0, <0.16.0+ with 13 remote versions
TRACE Found candidate for package h11 with range >=0.16.0, <0.16.0+ after 1 steps: 0.16.0 version
TRACE Returning candidate for package h11 with range >=0.16.0, <0.16.0+ after 1 steps
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0
TRACE Chose package for decision: certifi. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, typing-extensions, h11, httpcore, httpx, idna, sniffio
DEBUG Searching for a compatible version of certifi (>=2025.4.26, <2025.4.26+)
TRACE Selecting candidate for pytest-benchmark with range >=3.4.1 with 45 remote versions
TRACE Found candidate for package pytest-benchmark with range >=3.4.1 after 1 steps: 5.1.0 version
TRACE Returning candidate for package pytest-benchmark with range >=3.4.1 after 1 steps
TRACE Selecting candidate for certifi with range >=2025.4.26, <2025.4.26+ with 63 remote versions
TRACE Found candidate for package certifi with range >=2025.4.26, <2025.4.26+ after 1 steps: 2025.4.26 version
TRACE Returning candidate for package certifi with range >=2025.4.26, <2025.4.26+ after 1 steps
TRACE Selecting candidate for httpcore with range >=1.0.9, <1.0.9+ with 64 remote versions
DEBUG Selecting: certifi==2025.4.26 [compatible] (certifi-2025.4.26-py3-none-any.whl)
TRACE Found candidate for package httpcore with range >=1.0.9, <1.0.9+ after 1 steps: 1.0.9 version
TRACE Returning candidate for package httpcore with range >=1.0.9, <1.0.9+ after 1 steps
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26
TRACE Chose package for decision: h11. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, typing-extensions, sniffio, httpcore, httpx, idna
DEBUG Searching for a compatible version of h11 (>=0.16.0, <0.16.0+)
TRACE Selecting candidate for h11 with range >=0.16.0, <0.16.0+ with 13 remote versions
TRACE Found candidate for package h11 with range >=0.16.0, <0.16.0+ after 1 steps: 0.16.0 version
TRACE Returning candidate for package h11 with range >=0.16.0, <0.16.0+ after 1 steps
TRACE Selecting candidate for pytest-randomly with range >=3.10.3 with 34 remote versions
DEBUG Selecting: h11==0.16.0 [compatible] (h11-0.16.0-py3-none-any.whl)
TRACE Found candidate for package pytest-randomly with range >=3.10.3 after 1 steps: 3.16.0 version
TRACE Returning candidate for package pytest-randomly with range >=3.10.3 after 1 steps
TRACE Selecting candidate for pytest-httpx with range >=0.35.0 with 52 remote versions
TRACE Found candidate for package pytest-httpx with range >=0.35.0 after 1 steps: 0.35.0 version
TRACE Returning candidate for package pytest-httpx with range >=0.35.0 after 1 steps
TRACE Selecting candidate for sniffio with range >=1.3.1, <1.3.1+ with 6 remote versions
TRACE Found candidate for package sniffio with range >=1.3.1, <1.3.1+ after 1 steps: 1.3.1 version
TRACE Returning candidate for package sniffio with range >=1.3.1, <1.3.1+ after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-asyncio/0.26.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl.metadata
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/httpx/0.28.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/h11/0.16.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl.metadata
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-benchmark/5.1.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl.metadata
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/httpcore/1.0.9-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl.metadata
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-randomly/3.16.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl.metadata
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-httpx/0.35.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl.metadata
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/sniffio/1.3.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: sniffio==1.3.1
TRACE Received built distribution metadata for: pytest-custom-exit-code==0.3.0
TRACE Received built distribution metadata for: pytest-asyncio==0.26.0
TRACE Cached request https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: httpx==0.28.1
TRACE Received built distribution metadata for: h11==0.16.0
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0
TRACE Received built distribution metadata for: httpcore==1.0.9
TRACE Chose package for decision: httpcore. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, typing-extensions, sniffio, idna, httpx
DEBUG Searching for a compatible version of httpcore (>=1.0.9, <1.0.9+)
TRACE Selecting candidate for httpcore with range >=1.0.9, <1.0.9+ with 64 remote versions
TRACE Cached request https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Found candidate for package httpcore with range >=1.0.9, <1.0.9+ after 1 steps: 1.0.9 version
TRACE Returning candidate for package httpcore with range >=1.0.9, <1.0.9+ after 1 steps
DEBUG Selecting: httpcore==1.0.9 [compatible] (httpcore-1.0.9-py3-none-any.whl)
TRACE Cached request https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Adding transitive dependency for httpcore==1.0.9: certifi*
DEBUG Adding transitive dependency for httpcore==1.0.9: h11>=0.16
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9
TRACE Chose package for decision: httpx. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, typing-extensions, sniffio, idna
DEBUG Searching for a compatible version of httpx (>=0.28.1, <0.28.1+)
TRACE Selecting candidate for httpx with range >=0.28.1, <0.28.1+ with 71 remote versions
TRACE Found candidate for package httpx with range >=0.28.1, <0.28.1+ after 1 steps: 0.28.1 version
TRACE Returning candidate for package httpx with range >=0.28.1, <0.28.1+ after 1 steps
DEBUG Selecting: httpx==0.28.1 [compatible] (httpx-0.28.1-py3-none-any.whl)
DEBUG Adding transitive dependency for httpx==0.28.1: anyio*
DEBUG Adding transitive dependency for httpx==0.28.1: certifi*
DEBUG Adding transitive dependency for httpx==0.28.1: httpcore>=1.dev0, <2.dev0
DEBUG Adding transitive dependency for httpx==0.28.1: idna*
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1
TRACE Chose package for decision: idna. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, typing-extensions, sniffio
DEBUG Searching for a compatible version of idna (>=3.10, <3.10+)
TRACE Selecting candidate for idna with range >=3.10, <3.10+ with 32 remote versions
TRACE Found candidate for package idna with range >=3.10, <3.10+ after 1 steps: 3.10 version
TRACE Returning candidate for package idna with range >=3.10, <3.10+ after 1 steps
DEBUG Selecting: idna==3.10 [compatible] (idna-3.10-py3-none-any.whl)
TRACE Cached request https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10
TRACE Chose package for decision: sniffio. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov, typing-extensions
DEBUG Searching for a compatible version of sniffio (>=1.3.1, <1.3.1+)
TRACE Selecting candidate for sniffio with range >=1.3.1, <1.3.1+ with 6 remote versions
TRACE Found candidate for package sniffio with range >=1.3.1, <1.3.1+ after 1 steps: 1.3.1 version
TRACE Returning candidate for package sniffio with range >=1.3.1, <1.3.1+ after 1 steps
DEBUG Selecting: sniffio==1.3.1 [compatible] (sniffio-1.3.1-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1
TRACE Chose package for decision: typing-extensions. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov
DEBUG Searching for a compatible version of typing-extensions (>=4.13.2, <4.13.2+)
TRACE Selecting candidate for typing-extensions with range >=4.13.2, <4.13.2+ with 44 remote versions
TRACE Found candidate for package typing-extensions with range >=4.13.2, <4.13.2+ after 1 steps: 4.13.2 version
TRACE Returning candidate for package typing-extensions with range >=4.13.2, <4.13.2+ after 1 steps
DEBUG Selecting: typing-extensions==4.13.2 [compatible] (typing_extensions-4.13.2-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2
TRACE Chose package for decision: typing-extensions{python_full_version < '3.13'}. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (>=4.5)
TRACE Selecting candidate for typing-extensions with range >=4.5 with 44 remote versions
TRACE Found candidate for package typing-extensions with range >=4.5 after 1 steps: 4.13.2 version
TRACE Returning candidate for package typing-extensions with range >=4.5 after 1 steps
DEBUG Selecting: typing-extensions==4.13.2 [compatible] (typing_extensions-4.13.2-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.13.2: typing-extensions==4.13.2
DEBUG Adding transitive dependency for typing-extensions==4.13.2: typing-extensions{python_full_version < '3.13'}==4.13.2
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2
TRACE Chose package for decision: typing-extensions{python_full_version < '3.13'}. remaining choices: pytest, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, pytest-cov
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.13'} (==4.13.2)
TRACE Selecting candidate for typing-extensions with range ==4.13.2 with 44 remote versions
TRACE Found candidate for package typing-extensions with range ==4.13.2 after 1 steps: 4.13.2 version
TRACE Returning candidate for package typing-extensions with range ==4.13.2 after 1 steps
DEBUG Selecting: typing-extensions==4.13.2 [compatible] (typing_extensions-4.13.2-py3-none-any.whl)
TRACE Received built distribution metadata for: pytest-httpx==0.35.0
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2
TRACE Chose package for decision: pytest. remaining choices: pytest-cov, pytest-repeat, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil
DEBUG Searching for a compatible version of pytest (>=6.2.2)
TRACE Selecting candidate for pytest with range >=6.2.2 with 183 remote versions
TRACE Found candidate for package pytest with range >=6.2.2 after 1 steps: 8.3.5 version
TRACE Returning candidate for package pytest with range >=6.2.2 after 1 steps
DEBUG Selecting: pytest==8.3.5 [compatible] (pytest-8.3.5-py3-none-any.whl)
DEBUG Adding transitive dependency for pytest==8.3.5: iniconfig*
DEBUG Adding transitive dependency for pytest==8.3.5: packaging*
DEBUG Adding transitive dependency for pytest==8.3.5: pluggy>=1.5, <2
TRACE Fetching metadata for iniconfig from https://***@<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5
TRACE Chose package for decision: pytest-repeat. remaining choices: pytest-cov, pluggy, pytest-xdist, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, iniconfig, packaging
DEBUG Searching for a compatible version of pytest-repeat (>=0.8.0)
TRACE Selecting candidate for pytest-repeat with range >=0.8.0 with 13 remote versions
TRACE Found candidate for package pytest-repeat with range >=0.8.0 after 1 steps: 0.9.4 version
TRACE Returning candidate for package pytest-repeat with range >=0.8.0 after 1 steps
TRACE Fetching metadata for packaging from https://***@<internal-repo>/repository/<org-name>-pypi/simple/packaging/
DEBUG Selecting: pytest-repeat==0.9.4 [compatible] (pytest_repeat-0.9.4-py3-none-any.whl)
TRACE Fetching metadata for pluggy from https://***@<internal-repo>/repository/<org-name>-pypi/simple/pluggy/
DEBUG Adding transitive dependency for pytest-repeat==0.9.4: pytest*
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4
TRACE Chose package for decision: pytest-xdist. remaining choices: pytest-cov, pluggy, packaging, pytest-timeout, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, iniconfig
DEBUG Searching for a compatible version of pytest-xdist (>=2.2.1)
TRACE Selecting candidate for pytest-xdist with range >=2.2.1 with 69 remote versions
TRACE Found candidate for package pytest-xdist with range >=2.2.1 after 1 steps: 3.6.1 version
TRACE Returning candidate for package pytest-xdist with range >=2.2.1 after 1 steps
DEBUG Selecting: pytest-xdist==3.6.1 [compatible] (pytest_xdist-3.6.1-py3-none-any.whl)
TRACE Received built distribution metadata for: pytest-benchmark==5.1.0
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/iniconfig.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/
DEBUG Adding transitive dependency for pytest-xdist==3.6.1: execnet>=2.1
DEBUG Adding transitive dependency for pytest-xdist==3.6.1: pytest>=7.0.0
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1
TRACE Chose package for decision: pytest-timeout. remaining choices: pytest-cov, pluggy, packaging, execnet, pytest-custom-exit-code, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil, iniconfig
DEBUG Searching for a compatible version of pytest-timeout (>=2.0.0)
TRACE Selecting candidate for pytest-timeout with range >=2.0.0 with 23 remote versions
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/packaging.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/packaging/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/packaging/
TRACE Found candidate for package pytest-timeout with range >=2.0.0 after 1 steps: 2.4.0 version
TRACE Returning candidate for package pytest-timeout with range >=2.0.0 after 1 steps
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/packaging/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/packaging/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
DEBUG Selecting: pytest-timeout==2.4.0 [compatible] (pytest_timeout-2.4.0-py3-none-any.whl)
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pluggy.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pluggy/
DEBUG Adding transitive dependency for pytest-timeout==2.4.0: pytest>=7.0.0
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pluggy/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pluggy/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/pluggy/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0
TRACE Chose package for decision: pytest-custom-exit-code. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, pytest-asyncio, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures, psutil
DEBUG Searching for a compatible version of pytest-custom-exit-code (>=0.3.0)
TRACE Selecting candidate for pytest-custom-exit-code with range >=0.3.0 with 6 remote versions
TRACE Found candidate for package pytest-custom-exit-code with range >=0.3.0 after 1 steps: 0.3.0 version
TRACE Returning candidate for package pytest-custom-exit-code with range >=0.3.0 after 1 steps
DEBUG Selecting: pytest-custom-exit-code==0.3.0 [compatible] (pytest_custom_exit_code-0.3.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pytest-custom-exit-code==0.3.0: pytest>=4.0.2
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0
TRACE Chose package for decision: pytest-asyncio. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, psutil, pytest-httpx, pytest-benchmark, pytest-randomly, pytest-rerunfailures
DEBUG Searching for a compatible version of pytest-asyncio (>=0.23.3)
TRACE Selecting candidate for pytest-asyncio with range >=0.23.3 with 66 remote versions
TRACE Found candidate for package pytest-asyncio with range >=0.23.3 after 2 steps: 0.26.0 version
TRACE Returning candidate for package pytest-asyncio with range >=0.23.3 after 2 steps
DEBUG Selecting: pytest-asyncio==0.26.0 [compatible] (pytest_asyncio-0.26.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pytest-asyncio==0.26.0: pytest>=8.2, <9
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0
TRACE Chose package for decision: pytest-httpx. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, psutil, pytest-benchmark, pytest-randomly, pytest-rerunfailures
DEBUG Searching for a compatible version of pytest-httpx (>=0.35.0)
TRACE Selecting candidate for pytest-httpx with range >=0.35.0 with 52 remote versions
TRACE Found candidate for package pytest-httpx with range >=0.35.0 after 1 steps: 0.35.0 version
TRACE Returning candidate for package pytest-httpx with range >=0.35.0 after 1 steps
TRACE Fetching metadata for execnet from https://***@<internal-repo>/repository/<org-name>-pypi/simple/execnet/
DEBUG Selecting: pytest-httpx==0.35.0 [compatible] (pytest_httpx-0.35.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pytest-httpx==0.35.0: httpx>=0.28.dev0, <0.29.dev0
DEBUG Adding transitive dependency for pytest-httpx==0.35.0: pytest>=8.dev0, <9.dev0
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0
TRACE Chose package for decision: pytest-benchmark. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, psutil, pytest-rerunfailures, pytest-randomly
DEBUG Searching for a compatible version of pytest-benchmark (>=3.4.1)
TRACE Selecting candidate for pytest-benchmark with range >=3.4.1 with 45 remote versions
TRACE Found candidate for package pytest-benchmark with range >=3.4.1 after 1 steps: 5.1.0 version
TRACE Returning candidate for package pytest-benchmark with range >=3.4.1 after 1 steps
DEBUG Selecting: pytest-benchmark==5.1.0 [compatible] (pytest_benchmark-5.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for pytest-benchmark==5.1.0: pytest>=8.1
DEBUG Adding transitive dependency for pytest-benchmark==5.1.0: py-cpuinfo*
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0
TRACE Chose package for decision: pytest-randomly. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, psutil, pytest-rerunfailures, py-cpuinfo
DEBUG Searching for a compatible version of pytest-randomly (>=3.10.3)
TRACE Selecting candidate for pytest-randomly with range >=3.10.3 with 34 remote versions
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/execnet.rkyv
TRACE Found candidate for package pytest-randomly with range >=3.10.3 after 1 steps: 3.16.0 version
TRACE Returning candidate for package pytest-randomly with range >=3.10.3 after 1 steps
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/execnet/
DEBUG Selecting: pytest-randomly==3.16.0 [compatible] (pytest_randomly-3.16.0-py3-none-any.whl)
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/execnet/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/execnet/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/execnet/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE Received built distribution metadata for: pytest-randomly==3.16.0
TRACE Fetching metadata for py-cpuinfo from https://***@<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/
DEBUG Adding transitive dependency for pytest-randomly==3.16.0: pytest*
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0
TRACE Chose package for decision: pytest-rerunfailures. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, psutil, py-cpuinfo
DEBUG Searching for a compatible version of pytest-rerunfailures (>=13.0)
TRACE Selecting candidate for pytest-rerunfailures with range >=13.0 with 34 remote versions
TRACE Found candidate for package pytest-rerunfailures with range >=13.0 after 1 steps: 15.1 version
TRACE Returning candidate for package pytest-rerunfailures with range >=13.0 after 1 steps
DEBUG Selecting: pytest-rerunfailures==15.1 [compatible] (pytest_rerunfailures-15.1-py3-none-any.whl)
DEBUG Adding transitive dependency for pytest-rerunfailures==15.1: packaging>=17.1
DEBUG Adding transitive dependency for pytest-rerunfailures==15.1: pytest>=7.4, <8.2.2 | >=8.2.2+
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/py-cpuinfo.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1
TRACE Chose package for decision: coverage[toml]. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, psutil, py-cpuinfo
DEBUG Searching for a compatible version of coverage[toml] (>=6.2)
TRACE Selecting candidate for coverage with range >=6.2 with 150 remote versions
TRACE Found candidate for package coverage with range >=6.2 after 1 steps: 7.8.0 version
TRACE Returning candidate for package coverage with range >=6.2 after 1 steps
DEBUG Selecting: coverage==7.8.0 [compatible] (coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for coverage==7.8.0: coverage==7.8.0
DEBUG Adding transitive dependency for coverage==7.8.0: coverage[toml]==7.8.0
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1
TRACE Selecting candidate for coverage with range ==7.8.0 with 150 remote versions
TRACE Chose package for decision: coverage. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, psutil, py-cpuinfo, coverage[toml]
TRACE Found candidate for package coverage with range ==7.8.0 after 1 steps: 7.8.0 version
DEBUG Searching for a compatible version of coverage (==7.8.0)
TRACE Returning candidate for package coverage with range ==7.8.0 after 1 steps
TRACE Selecting candidate for coverage with range ==7.8.0 with 150 remote versions
TRACE Found candidate for package coverage with range ==7.8.0 after 1 steps: 7.8.0 version
TRACE Returning candidate for package coverage with range ==7.8.0 after 1 steps
DEBUG Selecting: coverage==7.8.0 [compatible] (coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/coverage/7.8.0-46b83087afecf19d.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: coverage==7.8.0
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0
TRACE Chose package for decision: coverage[toml]. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, psutil, py-cpuinfo
DEBUG Searching for a compatible version of coverage[toml] (==7.8.0)
TRACE Selecting candidate for coverage with range ==7.8.0 with 150 remote versions
TRACE Found candidate for package coverage with range ==7.8.0 after 1 steps: 7.8.0 version
TRACE Returning candidate for package coverage with range ==7.8.0 after 1 steps
DEBUG Selecting: coverage==7.8.0 [compatible] (coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0, coverage[toml]==7.8.0
TRACE Chose package for decision: psutil. remaining choices: pytest-cov, pluggy, packaging, execnet, iniconfig, py-cpuinfo
DEBUG Searching for a compatible version of psutil (>=5.8.0)
TRACE Selecting candidate for psutil with range >=5.8.0 with 95 remote versions
TRACE Found candidate for package psutil with range >=5.8.0 after 1 steps: 7.0.0 version
TRACE Returning candidate for package psutil with range >=5.8.0 after 1 steps
DEBUG Selecting: psutil==7.0.0 [compatible] (psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0, coverage[toml]==7.8.0, psutil==7.0.0
TRACE Chose package for decision: pytest-cov. remaining choices: py-cpuinfo, pluggy, packaging, execnet, iniconfig
DEBUG Searching for a compatible version of pytest-cov (>=3.0.0)
TRACE Selecting candidate for pytest-cov with range >=3.0.0 with 49 remote versions
TRACE Found candidate for package pytest-cov with range >=3.0.0 after 1 steps: 6.1.1 version
TRACE Returning candidate for package pytest-cov with range >=3.0.0 after 1 steps
DEBUG Selecting: pytest-cov==6.1.1 [compatible] (pytest_cov-6.1.1-py3-none-any.whl)
DEBUG Adding transitive dependency for pytest-cov==6.1.1: pytest>=4.6
DEBUG Adding transitive dependency for pytest-cov==6.1.1: coverage[toml]>=7.5
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0, coverage[toml]==7.8.0, psutil==7.0.0, pytest-cov==6.1.1
TRACE Chose package for decision: iniconfig. remaining choices: py-cpuinfo, pluggy, packaging, execnet
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/packaging/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/packaging/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for packaging from ***<internal-repo>/repository/<org-name>-pypi/simple/packaging/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/packaging.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/packaging/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/packaging/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/packaging/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/packaging/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/py-cpuinfo/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for py-cpuinfo from ***<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/py-cpuinfo.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/execnet/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/execnet/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for execnet from ***<internal-repo>/repository/<org-name>-pypi/simple/execnet/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/execnet.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/execnet/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/execnet/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/execnet/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/execnet/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pluggy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pluggy/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pluggy from ***<internal-repo>/repository/<org-name>-pypi/simple/pluggy/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/pluggy.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/pluggy/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/pluggy/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/pluggy/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/pluggy/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/iniconfig/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for iniconfig from ***<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/iniconfig.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/packaging/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/packaging/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for packaging from https://pypi.org/simple/packaging/
TRACE Cached request https://pypi.org/simple/packaging/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
TRACE Received package metadata for: packaging
DEBUG Found installed version of packaging==25.0 that satisfies *
TRACE Using installed packaging 25.0 that satisfies *
TRACE Received installed distribution metadata for: packaging==25.0
DEBUG Found installed version of packaging==25.0 that satisfies >=17.1
TRACE Using installed packaging 25.0 that satisfies >=17.1
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/py-cpuinfo/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/py-cpuinfo/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for py-cpuinfo from https://pypi.org/simple/py-cpuinfo/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/py-cpuinfo.rkyv
DEBUG No cache entry for: https://pypi.org/simple/py-cpuinfo/
TRACE Sending fresh GET request for https://pypi.org/simple/py-cpuinfo/
TRACE Handling request for https://pypi.org/simple/py-cpuinfo/ with authentication policy auto
TRACE Request for https://pypi.org/simple/py-cpuinfo/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/py-cpuinfo/
TRACE Attempting unauthenticated request for https://pypi.org/simple/py-cpuinfo/
TRACE Cached request https://pypi.org/simple/py-cpuinfo/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: py-cpuinfo
TRACE Selecting candidate for py-cpuinfo with range * with 23 remote versions
TRACE Found candidate for package py-cpuinfo with range * after 1 steps: 9.0.0 version
TRACE Returning candidate for package py-cpuinfo with range * after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/py-cpuinfo/9.0.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl.metadata
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/pluggy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/pluggy/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pluggy from https://pypi.org/simple/pluggy/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/pluggy.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pluggy/
TRACE Sending fresh GET request for https://pypi.org/simple/pluggy/
TRACE Handling request for https://pypi.org/simple/pluggy/ with authentication policy auto
TRACE Request for https://pypi.org/simple/pluggy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pluggy/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pluggy/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/execnet/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/execnet/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for execnet from https://pypi.org/simple/execnet/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/execnet.rkyv
DEBUG No cache entry for: https://pypi.org/simple/execnet/
TRACE Sending fresh GET request for https://pypi.org/simple/execnet/
TRACE Handling request for https://pypi.org/simple/execnet/ with authentication policy auto
TRACE Request for https://pypi.org/simple/execnet/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/execnet/
TRACE Attempting unauthenticated request for https://pypi.org/simple/execnet/
TRACE Cached request https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://pypi.org/simple/pluggy/ is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: py-cpuinfo==9.0.0
TRACE Received package metadata for: pluggy
TRACE Selecting candidate for pluggy with range >=1.5, <2 with 23 remote versions
TRACE Found candidate for package pluggy with range >=1.5, <2 after 1 steps: 1.5.0 version
TRACE Returning candidate for package pluggy with range >=1.5, <2 after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pluggy/1.5.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
TRACE Cached request https://pypi.org/simple/execnet/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: execnet
TRACE Selecting candidate for execnet with range >=2.1 with 33 remote versions
TRACE Found candidate for package execnet with range >=2.1 after 1 steps: 2.1.1 version
TRACE Returning candidate for package execnet with range >=2.1 after 1 steps
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/execnet/2.1.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pluggy==1.5.0
TRACE Cached request https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: execnet==2.1.1
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/iniconfig/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/iniconfig/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for iniconfig from https://pypi.org/simple/iniconfig/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/pypi/iniconfig.rkyv
DEBUG No cache entry for: https://pypi.org/simple/iniconfig/
TRACE Sending fresh GET request for https://pypi.org/simple/iniconfig/
TRACE Handling request for https://pypi.org/simple/iniconfig/ with authentication policy auto
TRACE Request for https://pypi.org/simple/iniconfig/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/iniconfig/
TRACE Attempting unauthenticated request for https://pypi.org/simple/iniconfig/
TRACE Cached request https://pypi.org/simple/iniconfig/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: iniconfig
TRACE Selecting candidate for iniconfig with range * with 8 remote versions
TRACE Found candidate for package iniconfig with range * after 1 steps: 2.1.0 version
TRACE Returning candidate for package iniconfig with range * after 1 steps
DEBUG Searching for a compatible version of iniconfig (*)
TRACE Selecting candidate for iniconfig with range * with 8 remote versions
TRACE Found candidate for package iniconfig with range * after 1 steps: 2.1.0 version
TRACE Returning candidate for package iniconfig with range * after 1 steps
DEBUG Selecting: iniconfig==2.1.0 [compatible] (iniconfig-2.1.0-py3-none-any.whl)
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/iniconfig/2.1.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: iniconfig==2.1.0
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0, coverage[toml]==7.8.0, psutil==7.0.0, pytest-cov==6.1.1, iniconfig==2.1.0
TRACE Chose package for decision: packaging. remaining choices: py-cpuinfo, pluggy, execnet
DEBUG Searching for a compatible version of packaging (>=17.1)
DEBUG Found installed version of packaging==25.0 that satisfies >=17.1
TRACE Using installed packaging 25.0 that satisfies >=17.1
DEBUG Selecting: packaging==25.0 [installed] (installed)
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0, coverage[toml]==7.8.0, psutil==7.0.0, pytest-cov==6.1.1, iniconfig==2.1.0, packaging==25.0
TRACE Chose package for decision: pluggy. remaining choices: py-cpuinfo, execnet
DEBUG Searching for a compatible version of pluggy (>=1.5, <2)
TRACE Selecting candidate for pluggy with range >=1.5, <2 with 23 remote versions
TRACE Found candidate for package pluggy with range >=1.5, <2 after 1 steps: 1.5.0 version
TRACE Returning candidate for package pluggy with range >=1.5, <2 after 1 steps
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0, coverage[toml]==7.8.0, psutil==7.0.0, pytest-cov==6.1.1, iniconfig==2.1.0, packaging==25.0, pluggy==1.5.0
TRACE Chose package for decision: execnet. remaining choices: py-cpuinfo
DEBUG Searching for a compatible version of execnet (>=2.1)
TRACE Selecting candidate for execnet with range >=2.1 with 33 remote versions
TRACE Found candidate for package execnet with range >=2.1 after 1 steps: 2.1.1 version
TRACE Returning candidate for package execnet with range >=2.1 after 1 steps
DEBUG Selecting: execnet==2.1.1 [compatible] (execnet-2.1.1-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0, coverage[toml]==7.8.0, psutil==7.0.0, pytest-cov==6.1.1, iniconfig==2.1.0, packaging==25.0, pluggy==1.5.0, execnet==2.1.1
TRACE Chose package for decision: py-cpuinfo. remaining choices: 
DEBUG Searching for a compatible version of py-cpuinfo (*)
TRACE Selecting candidate for py-cpuinfo with range * with 23 remote versions
TRACE Found candidate for package py-cpuinfo with range * after 1 steps: 9.0.0 version
TRACE Returning candidate for package py-cpuinfo with range * after 1 steps
DEBUG Selecting: py-cpuinfo==9.0.0 [compatible] (py_cpuinfo-9.0.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, <internal-lib>==3.17.0, anyio==4.9.0, certifi==2025.4.26, h11==0.16.0, httpcore==1.0.9, httpx==0.28.1, idna==3.10, sniffio==1.3.1, typing-extensions==4.13.2, typing-extensions{python_full_version < '3.13'}==4.13.2, pytest==8.3.5, pytest-repeat==0.9.4, pytest-xdist==3.6.1, pytest-timeout==2.4.0, pytest-custom-exit-code==0.3.0, pytest-asyncio==0.26.0, pytest-httpx==0.35.0, pytest-benchmark==5.1.0, pytest-randomly==3.16.0, pytest-rerunfailures==15.1, coverage==7.8.0, coverage[toml]==7.8.0, psutil==7.0.0, pytest-cov==6.1.1, iniconfig==2.1.0, packaging==25.0, pluggy==1.5.0, execnet==2.1.1, py-cpuinfo==9.0.0
DEBUG Tried 27 versions: anyio 1, certifi 1, coverage 1, execnet 1, h11 1, httpcore 1, httpx 1, idna 1, iniconfig 1, packaging 1, pluggy 1, psutil 1, py-cpuinfo 1, pytest 1, pytest-asyncio 1, pytest-benchmark 1, pytest-cov 1, pytest-custom-exit-code 1, pytest-httpx 1, pytest-randomly 1, pytest-repeat 1, pytest-rerunfailures 1, pytest-timeout 1, pytest-xdist 1, <internal-lib> 1, sniffio 1, typing-extensions 1
DEBUG marker environment resolution took 0.230s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.3", version: "3.12.3" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "5.10.226-214.880.amzn2.x86_64", platform_system: "Linux", platform_version: "#1 SMP Tue Oct 8 16:18:15 UTC 2024", python_full_version: StringVersion { string: "3.12.3", version: "3.12.3" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> pytest
TRACE Resolution edge:     0a0.dev0 -> 8.3.5
TRACE Resolution edge: ROOT -> pytest-repeat
TRACE Resolution edge:     0a0.dev0 -> 0.9.4
TRACE Resolution edge: ROOT -> pytest-xdist
TRACE Resolution edge:     0a0.dev0 -> 3.6.1
TRACE Resolution edge: ROOT -> pytest-timeout
TRACE Resolution edge:     0a0.dev0 -> 2.4.0
TRACE Resolution edge: ROOT -> pytest-custom-exit-code
TRACE Resolution edge:     0a0.dev0 -> 0.3.0
TRACE Resolution edge: ROOT -> pytest-asyncio
TRACE Resolution edge:     0a0.dev0 -> 0.26.0
TRACE Resolution edge: ROOT -> pytest-httpx
TRACE Resolution edge:     0a0.dev0 -> 0.35.0
TRACE Resolution edge: ROOT -> pytest-benchmark
TRACE Resolution edge:     0a0.dev0 -> 5.1.0
TRACE Resolution edge: ROOT -> pytest-randomly
TRACE Resolution edge:     0a0.dev0 -> 3.16.0
TRACE Resolution edge: ROOT -> pytest-rerunfailures
TRACE Resolution edge:     0a0.dev0 -> 15.1
TRACE Resolution edge: ROOT -> coverage
TRACE Resolution edge:     0a0.dev0 -> 7.8.0 (extra: toml)
TRACE Resolution edge: ROOT -> coverage
TRACE Resolution edge:     0a0.dev0 -> 7.8.0
TRACE Resolution edge: ROOT -> psutil
TRACE Resolution edge:     0a0.dev0 -> 7.0.0
TRACE Resolution edge: ROOT -> pytest-cov
TRACE Resolution edge:     0a0.dev0 -> 6.1.1
TRACE Resolution edge: ROOT -> anyio
TRACE Resolution edge:     0a0.dev0 -> 4.9.0
TRACE Resolution edge: ROOT -> certifi
TRACE Resolution edge:     0a0.dev0 -> 2025.4.26
TRACE Resolution edge: ROOT -> h11
TRACE Resolution edge:     0a0.dev0 -> 0.16.0
TRACE Resolution edge: ROOT -> httpcore
TRACE Resolution edge:     0a0.dev0 -> 1.0.9
TRACE Resolution edge: ROOT -> httpx
TRACE Resolution edge:     0a0.dev0 -> 0.28.1
TRACE Resolution edge: ROOT -> idna
TRACE Resolution edge:     0a0.dev0 -> 3.10
TRACE Resolution edge: ROOT -> sniffio
TRACE Resolution edge:     0a0.dev0 -> 1.3.1
TRACE Resolution edge: ROOT -> typing-extensions
TRACE Resolution edge:     0a0.dev0 -> 4.13.2
TRACE Resolution edge: ROOT -> <internal-lib>
TRACE Resolution edge:     0a0.dev0 -> 3.17.0
TRACE Resolution edge: <internal-lib> -> httpx
TRACE Resolution edge:     3.17.0 -> 0.28.1
TRACE Resolution edge: pytest-xdist -> execnet
TRACE Resolution edge:     3.6.1 -> 2.1.1
TRACE Resolution edge: pytest-xdist -> pytest
TRACE Resolution edge:     3.6.1 -> 8.3.5
TRACE Resolution edge: pytest-asyncio -> pytest
TRACE Resolution edge:     0.26.0 -> 8.3.5
TRACE Resolution edge: pytest-randomly -> pytest
TRACE Resolution edge:     3.16.0 -> 8.3.5
TRACE Resolution edge: httpx -> anyio
TRACE Resolution edge:     0.28.1 -> 4.9.0
TRACE Resolution edge: httpx -> certifi
TRACE Resolution edge:     0.28.1 -> 2025.4.26
TRACE Resolution edge: httpx -> httpcore
TRACE Resolution edge:     0.28.1 -> 1.0.9
TRACE Resolution edge: httpx -> idna
TRACE Resolution edge:     0.28.1 -> 3.10
TRACE Resolution edge: pytest-repeat -> pytest
TRACE Resolution edge:     0.9.4 -> 8.3.5
TRACE Resolution edge: pytest-custom-exit-code -> pytest
TRACE Resolution edge:     0.3.0 -> 8.3.5
TRACE Resolution edge: pytest-benchmark -> pytest
TRACE Resolution edge:     5.1.0 -> 8.3.5
TRACE Resolution edge: pytest-benchmark -> py-cpuinfo
TRACE Resolution edge:     5.1.0 -> 9.0.0
TRACE Resolution edge: anyio -> idna
TRACE Resolution edge:     4.9.0 -> 3.10
TRACE Resolution edge: anyio -> sniffio
TRACE Resolution edge:     4.9.0 -> 1.3.1
TRACE Resolution edge: anyio -> typing-extensions
TRACE Resolution edge:     4.9.0 -> 4.13.2 ; python_full_version < '3.13'
TRACE Resolution edge: httpcore -> certifi
TRACE Resolution edge:     1.0.9 -> 2025.4.26
TRACE Resolution edge: httpcore -> h11
TRACE Resolution edge:     1.0.9 -> 0.16.0
TRACE Resolution edge: pytest -> iniconfig
TRACE Resolution edge:     8.3.5 -> 2.1.0
TRACE Resolution edge: pytest -> packaging
TRACE Resolution edge:     8.3.5 -> 25.0
TRACE Resolution edge: pytest -> pluggy
TRACE Resolution edge:     8.3.5 -> 1.5.0
TRACE Resolution edge: pytest-timeout -> pytest
TRACE Resolution edge:     2.4.0 -> 8.3.5
TRACE Resolution edge: pytest-httpx -> httpx
TRACE Resolution edge:     0.35.0 -> 0.28.1
TRACE Resolution edge: pytest-httpx -> pytest
TRACE Resolution edge:     0.35.0 -> 8.3.5
TRACE Resolution edge: pytest-rerunfailures -> packaging
TRACE Resolution edge:     15.1 -> 25.0
TRACE Resolution edge: pytest-rerunfailures -> pytest
TRACE Resolution edge:     15.1 -> 8.3.5
TRACE Resolution edge: pytest-cov -> pytest
TRACE Resolution edge:     6.1.1 -> 8.3.5
TRACE Resolution edge: pytest-cov -> coverage
TRACE Resolution edge:     6.1.1 -> 7.8.0 (extra: toml)
TRACE Resolution edge: pytest-cov -> coverage
TRACE Resolution edge:     6.1.1 -> 7.8.0
Resolved 27 packages in 231ms
DEBUG Identified uncached distribution: pytest-rerunfailures==15.1
DEBUG Identified uncached distribution: h11==0.16.0
DEBUG Registry requirement already cached: idna==3.10
DEBUG Identified uncached distribution: iniconfig==2.1.0
DEBUG Identified uncached distribution: pytest-repeat==0.9.4
DEBUG Identified uncached distribution: pytest-custom-exit-code==0.3.0
DEBUG Identified uncached distribution: anyio==4.9.0
DEBUG Identified uncached distribution: psutil==7.0.0
DEBUG Identified uncached distribution: coverage==7.8.0
DEBUG Identified uncached distribution: httpcore==1.0.9
DEBUG Identified uncached distribution: pytest-benchmark==5.1.0
DEBUG Registry requirement already cached: certifi==2025.4.26
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("packaging"), version: "25.0", path: "/home/runner/<internal-env-path>/python3.12/site-packages/packaging-25.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "25.0" }]), index: None, conflict: None }
DEBUG Requirement already installed: packaging==25.0
DEBUG Identified uncached distribution: py-cpuinfo==9.0.0
DEBUG Identified uncached distribution: pytest-asyncio==0.26.0
DEBUG Identified uncached distribution: pytest-httpx==0.35.0
DEBUG Identified uncached distribution: pytest-xdist==3.6.1
DEBUG Identified uncached distribution: pytest-randomly==3.16.0
DEBUG Identified uncached distribution: httpx==0.28.1
DEBUG Identified uncached distribution: pluggy==1.5.0
DEBUG Must revalidate requirement: <internal-lib>
DEBUG Identified uncached distribution: execnet==2.1.1
DEBUG Registry requirement already cached: typing-extensions==4.13.2
DEBUG Identified uncached distribution: sniffio==1.3.1
DEBUG Identified uncached distribution: pytest==8.3.5
DEBUG Identified uncached distribution: pytest-timeout==2.4.0
DEBUG Identified uncached distribution: pytest-cov==6.1.1
DEBUG Unnecessary package: secretstorage==3.3.3
DEBUG Unnecessary package: build==1.2.2.post1
DEBUG Unnecessary package: cffi==1.17.1
DEBUG Unnecessary package: cryptography==44.0.3
DEBUG Unnecessary package: jaraco-classes==3.4.0
DEBUG Unnecessary package: jaraco-context==6.0.1
DEBUG Unnecessary package: jaraco-functools==4.1.0
DEBUG Unnecessary package: jeepney==0.9.0
DEBUG Unnecessary package: keyring==25.6.0
DEBUG Unnecessary package: more-itertools==10.7.0
DEBUG Preserving seed package: pip==25.1.1
DEBUG Unnecessary package: pycparser==2.22
DEBUG Unnecessary package: pyproject-hooks==1.2.0
TRACE Checking lock for `/home/runner/.cache/uv/sdists-v9/editable/dd1c6159e9a26351` at `/home/runner/.cache/uv/sdists-v9/editable/dd1c6159e9a26351/.lock`
DEBUG Acquired lock for `/home/runner/.cache/uv/sdists-v9/editable/dd1c6159e9a26351`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1747294154, tv_nsec: 413187254 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1747294154, tv_nsec: 413187254 })))}
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest/8.3.5-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/psutil/7.0.0-c0164a3d3dead0b8.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Handling request for https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/coverage/7.8.0-46b83087afecf19d.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Handling request for https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/anyio/4.9.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/httpcore/1.0.9-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/httpx/0.28.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-xdist/3.6.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-benchmark/5.1.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/execnet/2.1.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/h11/0.16.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-cov/6.1.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/py-cpuinfo/9.0.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pluggy/1.5.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-asyncio/0.26.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-httpx/0.35.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-timeout/2.4.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-rerunfailures/15.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/sniffio/1.3.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-randomly/3.16.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/iniconfig/2.1.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-repeat/0.9.4-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl
TRACE No cache entry exists for /home/runner/.cache/uv/wheels-v5/pypi/pytest-custom-exit-code/0.3.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl
   Building <internal-lib> @ file:///home/runner/_work/<internal/<internal-path>/<internal-lib>
DEBUG Building: <internal-lib> @ file:///home/runner/_work/<internal/<internal-path>/<internal-lib>
TRACE Preview is disabled, not checking for direct build
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /usr/bin/python3.12
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: setuptools*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: setuptools. remaining choices: 
TRACE Fetching metadata for setuptools from https://***@<internal-repo>/repository/<org-name>-pypi/simple/setuptools/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/setuptools.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/setuptools/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/setuptools/
TRACE Handling request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/setuptools/ with authentication policy auto
TRACE Request for https://****@<internal-repo>/repository/<org-name>-pypi/simple/setuptools/ is missing a password, looking for credentials
TRACE Found cached credentials for realm ***@https://<internal-repo>
TRACE Cached request https://files.pythonhosted.org/packages/b0/ed/026d467c1853dd83102411a78126b4842618e86c895f93528b0528c7a620/pytest_httpx-0.35.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/f3/30/11d836ff01c938969efa319b4ebe2374ed79d28043a12bfc908577aab9f3/pytest_rerunfailures-15.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/22/70/b31577d7c46d8e2f9baccfed5067dd8475262a2331ffb0bfdf19361c9bde/pytest_randomly-3.16.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/2c/e1/e6716421ea10d38022b952c159d5161ca1193197fb744506875fbb87ea7b/iniconfig-2.1.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/73/d4/8b706b81b07b43081bd68a2c0359fe895b74bf664b20aca8005d2bb3be71/pytest_repeat-0.9.4-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/fa/b6/3127540ecdf1464a00e5a01ee60a1b09175f6913f0644ac748494d9c4b21/pytest_timeout-2.4.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/43/09/2aea36ff60d16dd8879bdb2f5b3ee0ba8d08cbbdcdfe870e695ce3784385/execnet-2.1.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/28/d0/def53b4a790cfb21483016430ed828f64830dd981ebe1089971cd10cab25/pytest_cov-6.1.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/9e/d6/b41653199ea09d5969d4e385df9bbfd9a100f28ca7e824ce7c0a016e3053/pytest_benchmark-5.1.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/30/3d/64ad57c803f1fa1e963a7946b6e0fea4a70df53c1a7fed304586539c2bac/pytest-8.3.5-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/20/7f/338843f449ace853647ace35870874f69a764d251872ed1b4de9f234822c/pytest_asyncio-0.26.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/6d/82/1d96bf03ee4c0fdc3c0cbe61470070e659ca78dc0086fb88b66c185e2449/pytest_xdist-3.6.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/35/a0/effb6cbbccfd1c106c572d3d619b3418d71093afb4cd4f91f51e6a1799d2/pytest_custom_exit_code-0.3.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/10/e0/dc8355f992b6cc2f9dcd5ef6242b62a3f73264893bc09fbb08bfcab18eb4/coverage-7.8.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/bf/b9/b0eb3f3cbcb734d930fdf839431606844a825b23eaf9a6ab371edac8162c/psutil-7.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/e0/a9/023730ba63db1e494a271cb018dcd361bd2c917ba7004c3e49d5daf795a2/py_cpuinfo-9.0.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/setuptools/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/setuptools/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for setuptools from ***<internal-repo>/repository/<org-name>-pypi/simple/setuptools/
TRACE No cache entry exists for /home/runner/.cache/uv/simple-v15/index/c9b6133a0cd7a3fc/setuptools.rkyv
DEBUG No cache entry for: https://<internal-repo>/repository/<org-name>-pypi/simple/setuptools/
TRACE Sending fresh GET request for https://<internal-repo>/repository/<org-name>-pypi/simple/setuptools/
TRACE Handling request for ***<internal-repo>/repository/<org-name>-pypi/simple/setuptools/ with authentication policy auto
TRACE Request for ***<internal-repo>/repository/<org-name>-pypi/simple/setuptools/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("<internal-repo>")), port: None, path: "/repository/<org-name>-pypi/simple/setuptools/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://<internal-repo>/repository/<org-name>-pypi/simple/setuptools/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for setuptools from https://pypi.org/simple/setuptools/
TRACE Cached request https://pypi.org/simple/setuptools/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
TRACE Received package metadata for: setuptools
TRACE Selecting candidate for setuptools with range * with 613 remote versions
DEBUG Searching for a compatible version of setuptools (*)
TRACE Selecting candidate for setuptools with range * with 613 remote versions
TRACE Found candidate for package setuptools with range * after 1 steps: 80.7.1 version
TRACE Found candidate for package setuptools with range * after 1 steps: 80.7.1 version
TRACE Returning candidate for package setuptools with range * after 1 steps
TRACE Returning candidate for package setuptools with range * after 1 steps
DEBUG Selecting: setuptools==80.7.1 [compatible] (setuptools-80.7.1-py3-none-any.whl)
TRACE Cached request https://files.pythonhosted.org/packages/a1/18/0e835c3a557dc5faffc8f91092f62fc337c1dab1066715842e7a4b318ec4/setuptools-80.7.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a1/18/0e835c3a557dc5faffc8f91092f62fc337c1dab1066715842e7a4b318ec4/setuptools-80.7.1-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: setuptools==80.7.1
TRACE Assigned packages: root==0a0.dev0, setuptools==80.7.1
DEBUG Tried 1 versions: setuptools 1
DEBUG marker environment resolution took 0.033s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.3", version: "3.12.3" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "5.10.226-214.880.amzn2.x86_64", platform_system: "Linux", platform_version: "#1 SMP Tue Oct 8 16:18:15 UTC 2024", python_full_version: StringVersion { string: "3.12.3", version: "3.12.3" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> setuptools
TRACE Resolution edge:     0a0.dev0 -> 80.7.1
DEBUG Installing in setuptools==80.7.1 in /home/runner/.cache/uv/builds-v0/.tmptL2kiW
DEBUG Registry requirement already cached: setuptools==80.7.1
DEBUG Installing build requirement: setuptools==80.7.1
TRACE Extracting file name=PackageName("setuptools")
TRACE Extracted 511 files name=PackageName("setuptools")
TRACE No entrypoints name=PackageName("setuptools")
TRACE No data name=PackageName("setuptools")
TRACE Writing installer metadata name=PackageName("setuptools")
TRACE Writing record name=PackageName("setuptools")
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta.get_requires_for_build_editable()`
DEBUG running egg_info
DEBUG writing src/<internal-lib>.egg-info/PKG-INFO
DEBUG writing dependency_links to src/<internal-lib>.egg-info/dependency_links.txt
DEBUG writing requirements to src/<internal-lib>.egg-info/requires.txt
DEBUG writing top-level names to src/<internal-lib>.egg-info/top_level.txt
DEBUG reading manifest file 'src/<internal-lib>.egg-info/SOURCES.txt'
DEBUG writing manifest file 'src/<internal-lib>.egg-info/SOURCES.txt'
DEBUG No workspace root found, using project root
DEBUG Calling `setuptools.build_meta.build_editable("/home/runner/.cache/uv/builds-v0/.tmphWM3IP", {}, None)`
DEBUG running editable_wheel
DEBUG creating /home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>.egg-info
DEBUG writing /home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>.egg-info/PKG-INFO
DEBUG writing dependency_links to /home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>.egg-info/dependency_links.txt
DEBUG writing requirements to /home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>.egg-info/requires.txt
DEBUG writing top-level names to /home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>.egg-info/top_level.txt
DEBUG writing manifest file '/home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>.egg-info/SOURCES.txt'
DEBUG reading manifest file '/home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>.egg-info/SOURCES.txt'
DEBUG writing manifest file '/home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>.egg-info/SOURCES.txt'
DEBUG creating '/home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>-3.17.0.dist-info'
DEBUG creating /home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>-3.17.0.dist-info/WHEEL
DEBUG running build_py
DEBUG running egg_info
DEBUG creating /tmp/tmp8zc8cvsu.build-temp/<internal-lib>.egg-info
DEBUG writing /tmp/tmp8zc8cvsu.build-temp/<internal-lib>.egg-info/PKG-INFO
DEBUG writing dependency_links to /tmp/tmp8zc8cvsu.build-temp/<internal-lib>.egg-info/dependency_links.txt
DEBUG writing requirements to /tmp/tmp8zc8cvsu.build-temp/<internal-lib>.egg-info/requires.txt
DEBUG writing top-level names to /tmp/tmp8zc8cvsu.build-temp/<internal-lib>.egg-info/top_level.txt
DEBUG writing manifest file '/tmp/tmp8zc8cvsu.build-temp/<internal-lib>.egg-info/SOURCES.txt'
DEBUG reading manifest file '/tmp/tmp8zc8cvsu.build-temp/<internal-lib>.egg-info/SOURCES.txt'
DEBUG writing manifest file '/tmp/tmp8zc8cvsu.build-temp/<internal-lib>.egg-info/SOURCES.txt'
DEBUG 
DEBUG         Editable install will be performed using .pth file to extend `sys.path` with:
DEBUG         ['src']
DEBUG         
DEBUG Options like `package-data`, `include/exclude-package-data` or
DEBUG `packages.find.exclude/include` may have no effect.
DEBUG 
DEBUG adding '__editable__.<internal-lib>-3.17.0.pth'
DEBUG creating '/home/runner/.cache/uv/builds-v0/.tmphWM3IP/.tmp-za46hcc4/<internal-lib>-3.17.0-0.editable-py3-none-any.whl' and adding '/tmp/tmpydi6iw9i<internal-lib>-3.17.0-0.editable-py3-none-any.whl' to it
DEBUG adding '<internal-lib>-3.17.0.dist-info/METADATA'
DEBUG adding '<internal-lib>-3.17.0.dist-info/WHEEL'
DEBUG adding '<internal-lib>-3.17.0.dist-info/top_level.txt'
DEBUG adding '<internal-lib>-3.17.0.dist-info/RECORD'
DEBUG Finished building: <internal-lib> @ file:///home/runner/_work/<internal/<internal-path>/<internal-lib>
      Built <internal-lib> @ file:///home/runner/_work/<internal/<internal-path>/<internal-lib>
DEBUG Released lock at `/home/runner/.cache/uv/sdists-v9/editable/dd1c6159e9a26351/.lock`
Prepared 23 packages in 598ms
TRACE Extracting file name=PackageName("pytest-timeout")
TRACE Extracting file name=PackageName("pytest-rerunfailures")
TRACE Extracting file name=PackageName("anyio")
TRACE Extracting file name=PackageName("execnet")
TRACE Extracting file name=PackageName("httpx")
TRACE Extracting file name=PackageName("pytest-benchmark")
TRACE Extracting file name=PackageName("pytest-asyncio")
TRACE Extracted 7 files name=PackageName("pytest-timeout")
TRACE No entrypoints name=PackageName("pytest-timeout")
TRACE No data name=PackageName("pytest-timeout")
TRACE Writing installer metadata name=PackageName("pytest-timeout")
TRACE Extracted 7 files name=PackageName("pytest-rerunfailures")
TRACE No entrypoints name=PackageName("pytest-rerunfailures")
TRACE No data name=PackageName("pytest-rerunfailures")
TRACE Writing installer metadata name=PackageName("pytest-rerunfailures")
TRACE Extracted 10 files name=PackageName("pytest-asyncio")
TRACE Writing record name=PackageName("pytest-rerunfailures")
TRACE No entrypoints name=PackageName("pytest-asyncio")
TRACE Extracted 22 files name=PackageName("execnet")
TRACE No data name=PackageName("pytest-asyncio")
TRACE Writing installer metadata name=PackageName("pytest-asyncio")
TRACE No entrypoints name=PackageName("execnet")
TRACE No data name=PackageName("execnet")
TRACE Writing installer metadata name=PackageName("execnet")
TRACE Extracted 25 files name=PackageName("pytest-benchmark")
TRACE Writing record name=PackageName("pytest-asyncio")
TRACE Extracted 29 files name=PackageName("httpx")
TRACE Writing record name=PackageName("execnet")
TRACE Writing record name=PackageName("pytest-timeout")
TRACE Extracted 47 files name=PackageName("anyio")
TRACE Extracting file name=PackageName("pytest-repeat")
TRACE No entrypoints name=PackageName("anyio")
TRACE No data name=PackageName("anyio")
TRACE Writing installer metadata name=PackageName("anyio")
TRACE Writing record name=PackageName("anyio")
TRACE Extracting file name=PackageName("pluggy")
TRACE Extracted 6 files name=PackageName("pytest-repeat")
TRACE Extracting file name=PackageName("pytest-cov")
TRACE No entrypoints name=PackageName("pytest-repeat")
TRACE No data name=PackageName("pytest-repeat")
TRACE Writing installer metadata name=PackageName("pytest-repeat")
TRACE Writing record name=PackageName("pytest-repeat")
TRACE Extracted 14 files name=PackageName("pluggy")
TRACE Extracted 13 files name=PackageName("pytest-cov")
TRACE No entrypoints name=PackageName("pluggy")
TRACE No data name=PackageName("pluggy")
TRACE Writing installer metadata name=PackageName("pluggy")
TRACE No entrypoints name=PackageName("pytest-cov")
TRACE No data name=PackageName("pytest-cov")
TRACE Writing installer metadata name=PackageName("pytest-cov")
TRACE Writing entrypoints name=PackageName("pytest-benchmark")
TRACE Writing record name=PackageName("pluggy")
TRACE Writing entrypoints name=PackageName("httpx")
TRACE Writing record name=PackageName("pytest-cov")
TRACE No data name=PackageName("pytest-benchmark")
TRACE Writing installer metadata name=PackageName("pytest-benchmark")
TRACE Extracting file name=PackageName("pytest-randomly")
TRACE No data name=PackageName("httpx")
TRACE Writing installer metadata name=PackageName("httpx")
TRACE Writing record name=PackageName("pytest-benchmark")
TRACE Writing record name=PackageName("httpx")
TRACE Extracted 8 files name=PackageName("pytest-randomly")
TRACE No entrypoints name=PackageName("pytest-randomly")
TRACE No data name=PackageName("pytest-randomly")
TRACE Writing installer metadata name=PackageName("pytest-randomly")
TRACE Extracting file name=PackageName("pytest-custom-exit-code")
TRACE Extracting file name=PackageName("coverage")
TRACE Extracting file name=PackageName("iniconfig")
TRACE Extracting file name=PackageName("h11")
TRACE Writing record name=PackageName("pytest-randomly")
TRACE Extracted 7 files name=PackageName("pytest-custom-exit-code")
TRACE No entrypoints name=PackageName("pytest-custom-exit-code")
TRACE No data name=PackageName("pytest-custom-exit-code")
TRACE Writing installer metadata name=PackageName("pytest-custom-exit-code")
TRACE Extracted 9 files name=PackageName("iniconfig")
TRACE No entrypoints name=PackageName("iniconfig")
TRACE No data name=PackageName("iniconfig")
TRACE Writing installer metadata name=PackageName("iniconfig")
TRACE Writing record name=PackageName("pytest-custom-exit-code")
TRACE Extracting file name=PackageName("pytest-xdist")
TRACE Extracting file name=PackageName("httpcore")
TRACE Writing record name=PackageName("iniconfig")
TRACE Extracting file name=PackageName("<internal-lib>")
TRACE Extracted 5 files name=PackageName("<internal-lib>")
TRACE No entrypoints name=PackageName("<internal-lib>")
TRACE No data name=PackageName("<internal-lib>")
TRACE Writing installer metadata name=PackageName("<internal-lib>")
TRACE Extracted 24 files name=PackageName("pytest-xdist")
TRACE Extracted 17 files name=PackageName("h11")
TRACE No entrypoints name=PackageName("pytest-xdist")
TRACE No data name=PackageName("pytest-xdist")
TRACE No entrypoints name=PackageName("h11")
TRACE Writing installer metadata name=PackageName("pytest-xdist")
TRACE Extracting file name=PackageName("sniffio")
TRACE No data name=PackageName("h11")
TRACE Writing installer metadata name=PackageName("h11")
TRACE Writing record name=PackageName("<internal-lib>")
TRACE Writing record name=PackageName("pytest-xdist")
TRACE Writing record name=PackageName("h11")
TRACE Extracted 58 files name=PackageName("coverage")
TRACE Extracting file name=PackageName("py-cpuinfo")
TRACE Writing entrypoints name=PackageName("coverage")
TRACE Extracted 36 files name=PackageName("httpcore")
TRACE Extracted 13 files name=PackageName("sniffio")
TRACE No entrypoints name=PackageName("httpcore")
TRACE No data name=PackageName("httpcore")
TRACE Writing installer metadata name=PackageName("httpcore")
TRACE No entrypoints name=PackageName("sniffio")
TRACE No data name=PackageName("sniffio")
TRACE Writing installer metadata name=PackageName("sniffio")
TRACE Extracted 9 files name=PackageName("py-cpuinfo")
TRACE Writing record name=PackageName("httpcore")
TRACE Extracting file name=PackageName("idna")
TRACE Writing record name=PackageName("sniffio")
TRACE No data name=PackageName("coverage")
TRACE Writing installer metadata name=PackageName("coverage")
TRACE Extracting file name=PackageName("certifi")
TRACE Extracting file name=PackageName("pytest")
TRACE Writing record name=PackageName("coverage")
TRACE Extracted 13 files name=PackageName("idna")
TRACE No entrypoints name=PackageName("idna")
TRACE No data name=PackageName("idna")
TRACE Writing installer metadata name=PackageName("idna")
TRACE Extracted 10 files name=PackageName("certifi")
TRACE No entrypoints name=PackageName("certifi")
TRACE No data name=PackageName("certifi")
TRACE Writing installer metadata name=PackageName("certifi")
TRACE Writing record name=PackageName("idna")
TRACE Writing record name=PackageName("certifi")
TRACE Extracting file name=PackageName("psutil")
TRACE Extracting file name=PackageName("pytest-httpx")
TRACE Extracting file name=PackageName("typing-extensions")
TRACE Writing entrypoints name=PackageName("py-cpuinfo")
TRACE No data name=PackageName("py-cpuinfo")
TRACE Writing installer metadata name=PackageName("py-cpuinfo")
TRACE Extracted 5 files name=PackageName("typing-extensions")
TRACE Extracted 14 files name=PackageName("pytest-httpx")
TRACE No entrypoints name=PackageName("typing-extensions")
TRACE No data name=PackageName("typing-extensions")
TRACE Writing installer metadata name=PackageName("typing-extensions")
TRACE No entrypoints name=PackageName("pytest-httpx")
TRACE No data name=PackageName("pytest-httpx")
TRACE Writing installer metadata name=PackageName("pytest-httpx")
TRACE Writing record name=PackageName("py-cpuinfo")
TRACE Writing record name=PackageName("typing-extensions")
TRACE Writing record name=PackageName("pytest-httpx")
TRACE Extracted 35 files name=PackageName("psutil")
TRACE No entrypoints name=PackageName("psutil")
TRACE No data name=PackageName("psutil")
TRACE Writing installer metadata name=PackageName("psutil")
TRACE Writing record name=PackageName("psutil")
TRACE Extracted 80 files name=PackageName("pytest")
TRACE Writing entrypoints name=PackageName("pytest")
TRACE No data name=PackageName("pytest")
TRACE Writing installer metadata name=PackageName("pytest")
TRACE Writing record name=PackageName("pytest")
```

(here it is the remaining output of the trace) 

---

_Comment by @jtfmumm on 2025-05-15 10:14_

Thank you for sharing. I've determined the cause of this in how we're handling the cached credentials in your case. I'm creating a fix now.

---

_Comment by @jmspereira on 2025-05-15 10:27_

Thank you! Please let me know if you need more information from my side. 

---

_Referenced in [astral-sh/uv#13463](../../astral-sh/uv/pulls/13463.md) on 2025-05-15 10:42_

---

_Closed by @jtfmumm on 2025-05-15 11:36_

---

_Closed by @jtfmumm on 2025-05-15 11:36_

---

_Comment by @jmspereira on 2025-05-16 08:10_

Hey @jtfmumm,
I tried the uv 0.7.4 version with the fix, and it appears to be working (i.e., it installs all the dependencies, meaning the credentials are correctly fetched). However, this stack continues to appear:

```
Traceback (most recent call last):
  File "/home/runner/<internal-path>/bin/keyring", line 10, in <module>
    sys.exit(main())
             ^^^^^^
  File "/home/runner/<internal-path>/lib/python3.12/site-packages/keyring/cli.py", line 216, in main
    return cli.run(argv)
           ^^^^^^^^^^^^^
  File "/home/runner/<internal-path>/lib/python3.12/site-packages/keyring/cli.py", line 120, in run
    return method()
           ^^^^^^^^
  File "/home/runner/<internal-path>/lib/python3.12/site-packages/keyring/cli.py", line 129, in do_get
    credential = getattr(self, f'_get_{self.get_mode}')()
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-path>/lib/python3.12/site-packages/keyring/cli.py", line 145, in _get_password
    password = get_password(self.service, self.username)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-path>/lib/python3.12/site-packages/keyring/core.py", line 63, in get_password
    return get_keyring().get_password(service_name, username)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/runner/<internal-path>/lib/python3.12/site-packages/keyring/backends/fail.py", line 28, in get_password
    raise NoKeyringError(msg)
keyring.errors.NoKeyringError: No recommended backend was available. Install a recommended 3rd party backend package; or, install the keyrings.alt package if you want to use the non-recommended backends. See https://pypi.org/project/keyring for details.
```

---
