```yaml
number: 12362
title: Prevent fallback to PyPi when private index returns 401/403 (to mitigate dependency confusion risk)
type: issue
state: closed
author: bendikhk
labels:
  - question
assignees: []
created_at: 2025-03-21T11:21:38Z
updated_at: 2025-04-29T21:37:08Z
url: https://github.com/astral-sh/uv/issues/12362
synced_at: 2026-01-12T16:01:01Z
```

# Prevent fallback to PyPi when private index returns 401/403 (to mitigate dependency confusion risk)

---

_@bendikhk_

I’m using uv with a setup where a private package index requires both authentication and a VPN connection. My intention is to always prefer the private index and only fall back to PyPI if the package truly does not exist on the private index.

This works as expected when the VPN and credentials are valid.

However, in cases where the VPN is disconnected or credentials are invalid (resulting in a 401 or 403), uv silently falls back to PyPI if the package is found there. This exposes a potential dependency confusion attack, where a malicious package on PyPI could be installed if the private index is temporarily unavailable.

Ideal behavior:
If the private index returns a 401 or 403, I would expect uv to not fall back to PyPI. Instead, it should treat this as a hard failure and return an error that it couldn't reach index 401/403.

I'm not 100% sure if this should be a feature request or a bug report. After reading the Package Indexes documentation I assumed it would work as as explained above. However if this is intentional, could an alternative be a stricter --index-strategy with this behaviour?

**Current setup:** 
`uv.toml`
```
[[index]]
url = "https://private-repo.com/simple"
authenticate = "always"
```
`.netrc`
```
machine private-repo.com
    login user
    password PW
```

### Example behaviour:
1. Private index unavailable and package not on pypi
```
$ uv pip install private-package-not-on-pypi
× No solution found when resolving dependencies:
╰─▶ Because private-package-not-on-pypi was not found in the package registry and you require it, your requirements are unsatisfiable.

hint: An index URL (https://private-repo.com/simple) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
```

2. Private index unavailable and package also on pyp
```
$ uv pip install private-package-also-on-pypi
→ Installs package from PyPI silently
```




### Platform

Linux 6.11.0-19-generic x86_64 GNU/Linux

### Version

0.6.9

### Python version

Python 3.12.9

---

_Label `bug` added by @bendikhk on 2025-03-21 11:21_

---

_Comment by @zanieb on 2025-03-21 13:30_

What's your private index (i.e. is it GitLab)?

---

_Comment by @zanieb on 2025-03-21 13:31_

Can you share verbose logs for the second case? e.g., with `-v`?

---

_Comment by @bendikhk on 2025-03-21 14:30_

We are using Artifactory.

Here is a test scenario where two different libraries are installed:
case 1: simplecalc 0.2.0 from private repo
case 2: simplecalc 0.1.0 installed from pypi
case 3: Install library only on private repository - Fails with hint to 401
case 4: private repo is not reachable - Fails hard

## Case 1: Can reach private repo:
```
(ddemo) appuser@bendikhk:~/app/ddemo$ uv pip install simplecalc --verbose
DEBUG uv 0.6.9
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/appuser/app/ddemo/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: simplecalc
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: simplecalc*
DEBUG No cache entry for: https://artifactory.local/repo/simple/simplecalc/
DEBUG Checking netrc for credentials for https://artifactory.local/repo/simple/simplecalc/
DEBUG Found credentials in netrc file for https://artifactory.local/repo/simple/simplecalc/
DEBUG Searching for a compatible version of simplecalc (*)
DEBUG Selecting: simplecalc==0.2.0 [compatible] (simplecalc-0.2.0-py3-none-any.whl)
DEBUG No cache entry for: https://artifactory.local/repo/simplecalc/0.2.0/simplecalc-0.2.0-py3-none-any.whl#sha256=ac546223446eb93a9f223b84815c43c7e33d16d66f8e175d16ebd2da53fcbacf
DEBUG Adding transitive dependency for simplecalc==0.2.0: typer>=0.15.2
DEBUG No cache entry for: https://artifactory.local/repo/simple/typer/
DEBUG No cache entry for: https://pypi.org/simple/typer/
DEBUG Searching for a compatible version of typer (>=0.15.2)
DEBUG Selecting: typer==0.15.2 [compatible] (typer-0.15.2-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for typer==0.15.2: click>=8.0.0
DEBUG Adding transitive dependency for typer==0.15.2: typing-extensions>=3.7.4.3
DEBUG Adding transitive dependency for typer==0.15.2: shellingham>=1.3.0
DEBUG Adding transitive dependency for typer==0.15.2: rich>=10.11.0
DEBUG No cache entry for: https://artifactory.local/repo/simple/click/
DEBUG No cache entry for: https://artifactory.local/repo/simple/typing-extensions/
DEBUG No cache entry for: https://artifactory.local/repo/simple/rich/
DEBUG No cache entry for: https://artifactory.local/repo/simple/shellingham/
DEBUG No cache entry for: https://pypi.org/simple/click/
DEBUG No cache entry for: https://pypi.org/simple/typing-extensions/
DEBUG No cache entry for: https://pypi.org/simple/rich/
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Selecting: click==8.1.8 [compatible] (click-8.1.8-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/shellingham/
DEBUG Searching for a compatible version of typing-extensions (>=3.7.4.3)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
WARN Fixing invalid version specifier by removing stray quotes (before: `>= '2.7'`; after: `>= 2.7`)
WARN Fixing invalid version specifier by removing stray quotes (before: `>= '2.7'`; after: `>= 2.7`)
DEBUG Searching for a compatible version of shellingham (>=1.3.0)
DEBUG Selecting: shellingham==1.5.4 [compatible] (shellingham-1.5.4-py2.py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of rich (>=10.11.0)
DEBUG Selecting: rich==13.9.4 [compatible] (rich-13.9.4-py3-none-any.whl)
DEBUG Adding transitive dependency for rich==13.9.4: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.9.4: pygments>=2.13.0, <3.0.0
DEBUG No cache entry for: https://artifactory.local/repo/simple/markdown-it-py/
DEBUG No cache entry for: https://artifactory.local/repo/simple/pygments/
DEBUG No cache entry for: https://pypi.org/simple/markdown-it-py/
DEBUG No cache entry for: https://pypi.org/simple/pygments/
WARN Skipping file for pygments: Pygments-1.6rc1-py2.7.egg
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==3.0.0 [compatible] (markdown_it_py-3.0.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.19.1 [compatible] (pygments-2.19.1-py3-none-any.whl)
DEBUG No cache entry for: https://artifactory.local/repo/simple/mdurl/
DEBUG No cache entry for: https://pypi.org/simple/mdurl/
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [compatible] (mdurl-0.1.2-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
DEBUG Tried 9 versions: click 1, markdown-it-py 1, mdurl 1, pygments 1, rich 1, shellingham 1, simplecalc 1, typer 1, typing-extensions 1
DEBUG marker environment resolution took 0.247s
Resolved 9 packages in 247ms
DEBUG Identified uncached distribution: click==8.1.8
DEBUG Identified uncached distribution: mdurl==0.1.2
DEBUG Identified uncached distribution: markdown-it-py==3.0.0
DEBUG Identified uncached distribution: shellingham==1.5.4
DEBUG Identified uncached distribution: pygments==2.19.1
DEBUG Identified uncached distribution: rich==13.9.4
DEBUG Identified uncached distribution: typer==0.15.2
DEBUG Identified uncached distribution: typing-extensions==4.12.2
DEBUG Identified uncached distribution: simplecalc==0.2.0
DEBUG No cache entry for: https://artifactory.local/repo/simplecalc/0.2.0/simplecalc-0.2.0-py3-none-any.whl#sha256=ac546223446eb93a9f223b84815c43c7e33d16d66f8e175d16ebd2da53fcbacf
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
Downloading pygments (1.2MiB)
 Downloaded pygments
Prepared 9 packages in 78ms
Installed 9 packages in 10ms
 + click==8.1.8
 + markdown-it-py==3.0.0
 + mdurl==0.1.2
 + pygments==2.19.1
 + rich==13.9.4
 + shellingham==1.5.4
 + simplecalc==0.2.0
 + typer==0.15.2
 + typing-extensions==4.12.2
DEBUG Released lock at `/home/appuser/app/ddemo/.venv/.lock`
```
Here **simplecalc==0.2.0** is installed from private repo

## Case 2: Authentication is wrong for private repo:
```
(ddemo) appuser@bendikhk-office:~/app/ddemo$ uv pip install simplecalc --verbose
DEBUG uv 0.6.9
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/appuser/app/ddemo/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: simplecalc
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: simplecalc*
DEBUG No cache entry for: https://artifactory.local/repo/simple/simplecalc/
DEBUG Checking netrc for credentials for https://artifactory.local/repo/simple/simplecalc/
DEBUG Found credentials in netrc file for https://artifactory.local/repo/simple/simplecalc/
DEBUG No cache entry for: https://pypi.org/simple/simplecalc/
DEBUG Searching for a compatible version of simplecalc (*)
DEBUG Selecting: simplecalc==0.1.0 [compatible] (simplecalc-0.1.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for simplecalc==0.1.0: click>=7.0, <8.0
DEBUG Adding transitive dependency for simplecalc==0.1.0: toml>=0.10.0, <0.11.0
DEBUG No cache entry for: https://artifactory.local/repo/simple/click/
DEBUG No cache entry for: https://artifactory.local/repo/simple/toml/
DEBUG No cache entry for: https://pypi.org/simple/toml/
WARN Skipping file for toml: toml-0.10.0-py2.7.egg
DEBUG No cache entry for: https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/click/
DEBUG Searching for a compatible version of click (>=7.0, <8.0)
DEBUG Selecting: click==7.1.2 [compatible] (click-7.1.2-py2.py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of toml (>=0.10.0, <0.11.0)
DEBUG Selecting: toml==0.10.2 [compatible] (toml-0.10.2-py2.py3-none-any.whl)
DEBUG Tried 3 versions: click 1, simplecalc 1, toml 1
DEBUG marker environment resolution took 0.100s
Resolved 3 packages in 100ms
DEBUG Identified uncached distribution: toml==0.10.2
DEBUG Identified uncached distribution: simplecalc==0.1.0
DEBUG Identified uncached distribution: click==7.1.2
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl
Prepared 3 packages in 22ms
Installed 3 packages in 13ms
 + click==7.1.2
 + simplecalc==0.1.0
 + toml==0.10.2
DEBUG Released lock at `/home/appuser/app/ddemo/.venv/.lock`
``` 
Here **simplecalc==0.1.0** is installed from PyPi

## Case 3: Install lib only on private repo
```
(ddemo) appuser@bendikhk-office:~/app/ddemo$ uv pip install private-only-lib --verbose
DEBUG uv 0.6.9
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/appuser/app/ddemo/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: private-only-lib
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: private-only-lib*
DEBUG No cache entry for: https://artifactory.local/repo/simple/private-only-lib/
DEBUG Checking netrc for credentials for https://artifactory.local/repo/simple/private-only-lib/
DEBUG Found credentials in netrc file for https://artifactory.local/repo/simple/private-only-lib/
DEBUG No cache entry for: https://pypi.org/simple/private-only-lib/
DEBUG Checking netrc for credentials for https://pypi.org/simple/private-only-lib/
DEBUG Searching for a compatible version of private-only-lib (*)
DEBUG No compatible version found for: private-only-lib
  × No solution found when resolving dependencies:
  ╰─▶ Because private-only-lib was not found in the package registry and you require private-only-lib, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://artifactory.local/repo/simple) could not be queried due to a lack of valid authentication
      credentials (401 Unauthorized).
DEBUG Released lock at `/home/appuser/app/ddemo/.venv/.lock`
```
It still checks PyPi but gives a useful error message.

## Case 4: If private repo is not reachable at all it will hard fail and not check pypi
```
(ddemo) appuser@bendikhk-office:~/app/ddemo$ uv pip install numpy --verbose
DEBUG uv 0.6.9
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/appuser/app/ddemo/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: numpy
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
DEBUG Adding direct dependency: numpy*
DEBUG No cache entry for: https://wrongartifactory.local/repo/simple/numpy/
DEBUG Transient request failure for https://wrongartifactory.local/repo/simple/numpy/, retrying: error sending request for url (https://wrongartifactory.local/repo/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
DEBUG Transient request failure for https://wrongartifactory.local/repo/simple/numpy/, retrying: error sending request for url (https://wrongartifactory.local/repo/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
DEBUG Transient request failure for https://wrongartifactory.local/repo/simple/numpy/, retrying: error sending request for url (https://wrongartifactory.local/repo/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
DEBUG Transient request failure for https://wrongartifactory.local/repo/simple/numpy/, retrying: error sending request for url (https://wrongartifactory.local/repo/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
DEBUG Released lock at `/home/appuser/app/ddemo/.venv/.lock`
error: Failed to fetch: `https://wrongartifactory.local/repo/simple/numpy/`
  Caused by: Could not connect, are you offline?
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://wrongartifactory.local/repo/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
```


---

_Comment by @zanieb on 2025-03-21 15:20_

This looks the same as https://github.com/astral-sh/uv/issues/10684 (and there are other reports)

I don't think we're getting a 401 / 403 there (it would be logged). Artifactory forwards to PyPI when unauthenticated.

You can set `authenticate = "always"` on your index to fail fast if credentials are not attached.

---

_Label `bug` removed by @zanieb on 2025-03-21 15:21_

---

_Label `question` added by @zanieb on 2025-03-21 15:21_

---

_Comment by @bendikhk on 2025-03-21 15:57_

I'm using Authenticate = "always"

We have set up Artifactory to not forward to PyPi. And I don't think Artifactory would forward to PyPi in a scenario where authentication fails. And if you see from Scenario 3 it does report a 401 to the private repository. 

I can try to reproduce with a different private repository as well!

---

_Comment by @zanieb on 2025-03-21 16:02_

Could you run case two `-vv`, that'll get us more logs.

---

_Comment by @bendikhk on 2025-03-22 11:23_

I think I've made some progress on this now. I was able to reproduce it with a local instance of a pypiserver. And it seems like the problem occurs if you need authentication to list packages from the private repo!


Example 1:
```
# Start server where you are allowed to list:
$ uv run pypi-server run -p 8080 -P ~/.htpasswd -a update,download packages/ --verbose

# Install with valid credentials 
$ uv pip install simplecalc -vv --no-cache
Installs simplecalc==0.3.0 from local server
```

Example 2:
```
# Start server where you are allowed to list:
$ uv run pypi-server run -p 8080 -P ~/.htpasswd -a update,download packages/ --verbose

# Install with **Invalid** credentials 
$ uv pip install simplecalc -vv --no-cache
error: Failed to fetch: `http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857`
  Caused by: HTTP status client error (403 Forbidden) for url (http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857)
```

Example 3:
```
# Start server where you are **NOT** allowed to list without authentication:
$ uv run pypi-server run -p 8080 -P ~/.htpasswd -a update,download,list packages/ --verbose

# Install with **Invalid** credentials 
$ uv pip install simplecalc -vv --no-cache
Installs simplecalc==0.1.0 from PyPi without any warnings
```
So it looks to me like it will only fail if it has found the actual package on the private-repo but can't retrieve it with GET.
I've added the logs with -vv below. Let me know if you want the code to reproduce.

## Logs

**Example 1: Valid install**
```
(ddemo) appuser@desktop:~/app/ddemo$ uv pip install simplecalc -vv
DEBUG uv 0.6.9
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /home/appuser/app/ddemo/.venv/bin/python3
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/appuser/app/ddemo/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: simplecalc
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: simplecalc*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: simplecalc. remaining choices:
TRACE Fetching metadata for simplecalc from http://localhost:8080/simple/simplecalc/
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/simplecalc.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/simplecalc/
TRACE Sending fresh GET request for http://localhost:8080/simple/simplecalc/
TRACE Handling request for http://localhost:8080/simple/simplecalc/
TRACE Request for http://localhost:8080/simple/simplecalc/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/simplecalc/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/simplecalc/
TRACE Cached request http://localhost:8080/simple/simplecalc/ is storable because its response has a heuristically cacheable status code 200
TRACE Received package metadata for: simplecalc
TRACE Selecting candidate for simplecalc with range * with 1 remote versions
DEBUG Searching for a compatible version of simplecalc (*)
TRACE Selecting candidate for simplecalc with range * with 1 remote versions
TRACE Found candidate for package simplecalc with range * after 1 steps: 0.3.0 version
TRACE Found candidate for package simplecalc with range * after 1 steps: 0.3.0 version
TRACE Returning candidate for package simplecalc with range * after 1 steps
TRACE Returning candidate for package simplecalc with range * after 1 steps
DEBUG Selecting: simplecalc==0.3.0 [compatible] (simplecalc-0.3.0-py3-none-any.whl)
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/simplecalc/0.3.0-py3-none-any.msgpack
DEBUG No cache entry for: http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Sending fresh HEAD request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Handling request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Attempting unauthenticated request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm http://localhost:8080
DEBUG Checking netrc for credentials for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
DEBUG Found credentials in netrc file for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Retrying request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 with Credentials { username: Username(Some("testuser")), password: Some("testpass") }
TRACE Updating cached credentials for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 to Credentials { username: Username(Some("testuser")), password: Some("testpass") }
TRACE Cached request http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is storable because its response has a heuristically cacheable status code 200
TRACE Getting metadata for simplecalc-0.3.0-py3-none-any.whl by range request
TRACE Handling request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is unauthenticated, checking cache
TRACE Found cached credentials for URL http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is fully authenticated
TRACE Received built distribution metadata for: simplecalc==0.3.0
DEBUG Adding transitive dependency for simplecalc==0.3.0: typer>=0.15.2
TRACE Fetching metadata for typer from http://localhost:8080/simple/typer/
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0
TRACE Chose package for decision: typer. remaining choices:
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/typer.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/typer/
TRACE Sending fresh GET request for http://localhost:8080/simple/typer/
TRACE Handling request for http://localhost:8080/simple/typer/
TRACE Request for http://localhost:8080/simple/typer/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/typer/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/typer/
TRACE Cached request http://localhost:8080/simple/typer/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: typer
TRACE Selecting candidate for typer with range >=0.15.2 with 50 remote versions
TRACE Found candidate for package typer with range >=0.15.2 after 1 steps: 0.15.2 version
TRACE Returning candidate for package typer with range >=0.15.2 after 1 steps
DEBUG Searching for a compatible version of typer (>=0.15.2)
TRACE Selecting candidate for typer with range >=0.15.2 with 50 remote versions
TRACE Found candidate for package typer with range >=0.15.2 after 1 steps: 0.15.2 version
TRACE Returning candidate for package typer with range >=0.15.2 after 1 steps
DEBUG Selecting: typer==0.15.2 [compatible] (typer-0.15.2-py3-none-any.whl)
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/typer/0.15.2-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: typer==0.15.2
DEBUG Adding transitive dependency for typer==0.15.2: click>=8.0.0
DEBUG Adding transitive dependency for typer==0.15.2: typing-extensions>=3.7.4.3
DEBUG Adding transitive dependency for typer==0.15.2: shellingham>=1.3.0
DEBUG Adding transitive dependency for typer==0.15.2: rich>=10.11.0
TRACE Fetching metadata for click from http://localhost:8080/simple/click/
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0, typer==0.15.2
TRACE Chose package for decision: click. remaining choices: rich, typing-extensions, shellingham
TRACE Fetching metadata for typing-extensions from http://localhost:8080/simple/typing-extensions/
TRACE Fetching metadata for shellingham from http://localhost:8080/simple/shellingham/
TRACE Fetching metadata for rich from http://localhost:8080/simple/rich/
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/click.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/click/
TRACE Sending fresh GET request for http://localhost:8080/simple/click/
TRACE Handling request for http://localhost:8080/simple/click/
TRACE Request for http://localhost:8080/simple/click/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/click/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/click/
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/typing-extensions.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/typing-extensions/
TRACE Sending fresh GET request for http://localhost:8080/simple/typing-extensions/
TRACE Handling request for http://localhost:8080/simple/typing-extensions/
TRACE Request for http://localhost:8080/simple/typing-extensions/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/typing-extensions/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/typing-extensions/
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/shellingham.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/shellingham/
TRACE Sending fresh GET request for http://localhost:8080/simple/shellingham/
TRACE Handling request for http://localhost:8080/simple/shellingham/
TRACE Request for http://localhost:8080/simple/shellingham/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/shellingham/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/shellingham/
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/rich.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/rich/
TRACE Sending fresh GET request for http://localhost:8080/simple/rich/
TRACE Handling request for http://localhost:8080/simple/rich/
TRACE Request for http://localhost:8080/simple/rich/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/rich/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/rich/
TRACE Cached request http://localhost:8080/simple/typing-extensions/ is storable because its response has a 'public' cache-control directive
TRACE Cached request http://localhost:8080/simple/click/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: click
TRACE Selecting candidate for click with range >=8.0.0 with 54 remote versions
DEBUG Searching for a compatible version of click (>=8.0.0)
TRACE Found candidate for package click with range >=8.0.0 after 1 steps: 8.1.8 version
TRACE Selecting candidate for click with range >=8.0.0 with 54 remote versions
TRACE Returning candidate for package click with range >=8.0.0 after 1 steps
TRACE Found candidate for package click with range >=8.0.0 after 1 steps: 8.1.8 version
TRACE Returning candidate for package click with range >=8.0.0 after 1 steps
DEBUG Selecting: click==8.1.8 [compatible] (click-8.1.8-py3-none-any.whl)
TRACE Received package metadata for: typing-extensions
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/click/8.1.8-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
TRACE Selecting candidate for typing-extensions with range >=3.7.4.3 with 41 remote versions
TRACE Found candidate for package typing-extensions with range >=3.7.4.3 after 2 steps: 4.12.2 version
TRACE Returning candidate for package typing-extensions with range >=3.7.4.3 after 2 steps
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/typing-extensions/4.12.2-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
TRACE Cached request http://localhost:8080/simple/shellingham/ is storable because its response has a 'public' cache-control directive
WARN Fixing invalid version specifier by removing stray quotes (before: `>= '2.7'`; after: `>= 2.7`)
WARN Fixing invalid version specifier by removing stray quotes (before: `>= '2.7'`; after: `>= 2.7`)
TRACE Cached request http://localhost:8080/simple/rich/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: shellingham
TRACE Selecting candidate for shellingham with range >=1.3.0 with 23 remote versions
TRACE Found candidate for package shellingham with range >=1.3.0 after 1 steps: 1.5.4 version
TRACE Returning candidate for package shellingham with range >=1.3.0 after 1 steps
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/shellingham/1.5.4-py2.py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: typing-extensions==4.12.2
TRACE Cached request https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: click==8.1.8
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0, typer==0.15.2, click==8.1.8
TRACE Chose package for decision: typing-extensions. remaining choices: rich, shellingham
DEBUG Searching for a compatible version of typing-extensions (>=3.7.4.3)
TRACE Selecting candidate for typing-extensions with range >=3.7.4.3 with 41 remote versions
TRACE Found candidate for package typing-extensions with range >=3.7.4.3 after 2 steps: 4.12.2 version
TRACE Returning candidate for package typing-extensions with range >=3.7.4.3 after 2 steps
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0, typer==0.15.2, click==8.1.8, typing-extensions==4.12.2
TRACE Chose package for decision: shellingham. remaining choices: rich
DEBUG Searching for a compatible version of shellingham (>=1.3.0)
TRACE Selecting candidate for shellingham with range >=1.3.0 with 23 remote versions
TRACE Found candidate for package shellingham with range >=1.3.0 after 1 steps: 1.5.4 version
TRACE Returning candidate for package shellingham with range >=1.3.0 after 1 steps
DEBUG Selecting: shellingham==1.5.4 [compatible] (shellingham-1.5.4-py2.py3-none-any.whl)
TRACE Received built distribution metadata for: shellingham==1.5.4
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0, typer==0.15.2, click==8.1.8, typing-extensions==4.12.2, shellingham==1.5.4
TRACE Chose package for decision: rich. remaining choices:
TRACE Received package metadata for: rich
TRACE Selecting candidate for rich with range >=10.11.0 with 198 remote versions
DEBUG Searching for a compatible version of rich (>=10.11.0)
TRACE Found candidate for package rich with range >=10.11.0 after 1 steps: 13.9.4 version
TRACE Selecting candidate for rich with range >=10.11.0 with 198 remote versions
TRACE Returning candidate for package rich with range >=10.11.0 after 1 steps
TRACE Found candidate for package rich with range >=10.11.0 after 1 steps: 13.9.4 version
TRACE Returning candidate for package rich with range >=10.11.0 after 1 steps
DEBUG Selecting: rich==13.9.4 [compatible] (rich-13.9.4-py3-none-any.whl)
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/rich/13.9.4-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: rich==13.9.4
DEBUG Adding transitive dependency for rich==13.9.4: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.9.4: pygments>=2.13.0, <3.0.0
TRACE Fetching metadata for markdown-it-py from http://localhost:8080/simple/markdown-it-py/
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0, typer==0.15.2, click==8.1.8, typing-extensions==4.12.2, shellingham==1.5.4, rich==13.9.4
TRACE Chose package for decision: markdown-it-py. remaining choices: pygments
TRACE Fetching metadata for pygments from http://localhost:8080/simple/pygments/
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/markdown-it-py.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/markdown-it-py/
TRACE Sending fresh GET request for http://localhost:8080/simple/markdown-it-py/
TRACE Handling request for http://localhost:8080/simple/markdown-it-py/
TRACE Request for http://localhost:8080/simple/markdown-it-py/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/markdown-it-py/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/markdown-it-py/
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/pygments.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/pygments/
TRACE Sending fresh GET request for http://localhost:8080/simple/pygments/
TRACE Handling request for http://localhost:8080/simple/pygments/
TRACE Request for http://localhost:8080/simple/pygments/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/pygments/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/pygments/
TRACE Cached request http://localhost:8080/simple/pygments/ is storable because its response has a 'public' cache-control directive
WARN Skipping file for pygments: Pygments-0.10-py2.3.egg
WARN Skipping file for pygments: Pygments-0.10-py2.4.egg
WARN Skipping file for pygments: Pygments-0.10-py2.5.egg
WARN Skipping file for pygments: Pygments-0.11-py2.3.egg
WARN Skipping file for pygments: Pygments-0.11-py2.4.egg
WARN Skipping file for pygments: Pygments-0.11-py2.5.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.5-py2.3.egg
WARN Skipping file for pygments: Pygments-0.5-py2.4.egg
WARN Skipping file for pygments: Pygments-0.5-py2.5.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.6-py2.3.egg
WARN Skipping file for pygments: Pygments-0.6-py2.4.egg
WARN Skipping file for pygments: Pygments-0.6-py2.5.egg
WARN Skipping file for pygments: Pygments-0.7-py2.3.egg
WARN Skipping file for pygments: Pygments-0.7-py2.4.egg
WARN Skipping file for pygments: Pygments-0.7-py2.5.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.8-py2.3.egg
WARN Skipping file for pygments: Pygments-0.8-py2.4.egg
WARN Skipping file for pygments: Pygments-0.8-py2.5.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.9-py2.3.egg
WARN Skipping file for pygments: Pygments-0.9-py2.4.egg
WARN Skipping file for pygments: Pygments-0.9-py2.5.egg
WARN Skipping file for pygments: Pygments-1.0-py2.3.egg
WARN Skipping file for pygments: Pygments-1.0-py2.4.egg
WARN Skipping file for pygments: Pygments-1.0-py2.5.egg
WARN Skipping file for pygments: Pygments-1.0-py2.6.egg
WARN Skipping file for pygments: Pygments-1.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.6.egg
WARN Skipping file for pygments: Pygments-1.3-py2.4.egg
WARN Skipping file for pygments: Pygments-1.3-py2.5.egg
WARN Skipping file for pygments: Pygments-1.3-py2.6.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.4-py2.4.egg
WARN Skipping file for pygments: Pygments-1.4-py2.5.egg
WARN Skipping file for pygments: Pygments-1.4-py2.6.egg
WARN Skipping file for pygments: Pygments-1.4-py2.7.egg
WARN Skipping file for pygments: Pygments-1.5-py2.6.egg
WARN Skipping file for pygments: Pygments-1.5-py2.7.egg
WARN Skipping file for pygments: Pygments-1.6-py2.6.egg
WARN Skipping file for pygments: Pygments-1.6-py2.7.egg
WARN Skipping file for pygments: Pygments-1.6rc1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.6rc1-py2.7.egg
TRACE Cached request http://localhost:8080/simple/markdown-it-py/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: pygments
TRACE Received package metadata for: markdown-it-py
TRACE Selecting candidate for pygments with range >=2.13.0, <3.0.0 with 66 remote versions
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
TRACE Selecting candidate for markdown-it-py with range >=2.2.0 with 42 remote versions
TRACE Found candidate for package pygments with range >=2.13.0, <3.0.0 after 1 steps: 2.19.1 version
TRACE Returning candidate for package pygments with range >=2.13.0, <3.0.0 after 1 steps
TRACE Found candidate for package markdown-it-py with range >=2.2.0 after 1 steps: 3.0.0 version
TRACE Returning candidate for package markdown-it-py with range >=2.2.0 after 1 steps
DEBUG Selecting: markdown-it-py==3.0.0 [compatible] (markdown_it_py-3.0.0-py3-none-any.whl)
TRACE Selecting candidate for markdown-it-py with range >=2.2.0 with 42 remote versions
TRACE Found candidate for package markdown-it-py with range >=2.2.0 after 1 steps: 3.0.0 version
TRACE Returning candidate for package markdown-it-py with range >=2.2.0 after 1 steps
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/pygments/2.19.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/markdown-it-py/3.0.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pygments==2.19.1
TRACE Cached request https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: markdown-it-py==3.0.0
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
TRACE Fetching metadata for mdurl from http://localhost:8080/simple/mdurl/
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0, typer==0.15.2, click==8.1.8, typing-extensions==4.12.2, shellingham==1.5.4, rich==13.9.4, markdown-it-py==3.0.0
TRACE Chose package for decision: pygments. remaining choices: mdurl
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
TRACE Selecting candidate for pygments with range >=2.13.0, <3.0.0 with 66 remote versions
TRACE Found candidate for package pygments with range >=2.13.0, <3.0.0 after 1 steps: 2.19.1 version
TRACE Returning candidate for package pygments with range >=2.13.0, <3.0.0 after 1 steps
DEBUG Selecting: pygments==2.19.1 [compatible] (pygments-2.19.1-py3-none-any.whl)
TRACE No cache entry exists for /home/appuser/.cache/uv/simple-v15/index/c043adf7183e3e19/mdurl.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/mdurl/
TRACE Sending fresh GET request for http://localhost:8080/simple/mdurl/
TRACE Handling request for http://localhost:8080/simple/mdurl/
TRACE Request for http://localhost:8080/simple/mdurl/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/mdurl/
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0, typer==0.15.2, click==8.1.8, typing-extensions==4.12.2, shellingham==1.5.4, rich==13.9.4, markdown-it-py==3.0.0, pygments==2.19.1
TRACE Attempting unauthenticated request for http://localhost:8080/simple/mdurl/
TRACE Chose package for decision: mdurl. remaining choices:
TRACE Cached request http://localhost:8080/simple/mdurl/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: mdurl
TRACE Selecting candidate for mdurl with range >=0.1, <1.dev0 with 4 remote versions
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
TRACE Found candidate for package mdurl with range >=0.1, <1.dev0 after 1 steps: 0.1.2 version
TRACE Selecting candidate for mdurl with range >=0.1, <1.dev0 with 4 remote versions
TRACE Returning candidate for package mdurl with range >=0.1, <1.dev0 after 1 steps
TRACE Found candidate for package mdurl with range >=0.1, <1.dev0 after 1 steps: 0.1.2 version
TRACE Returning candidate for package mdurl with range >=0.1, <1.dev0 after 1 steps
DEBUG Selecting: mdurl==0.1.2 [compatible] (mdurl-0.1.2-py3-none-any.whl)
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/mdurl/0.1.2-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: mdurl==0.1.2
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.3.0, typer==0.15.2, click==8.1.8, typing-extensions==4.12.2, shellingham==1.5.4, rich==13.9.4, markdown-it-py==3.0.0, pygments==2.19.1, mdurl==0.1.2
DEBUG Tried 9 versions: click 1, markdown-it-py 1, mdurl 1, pygments 1, rich 1, shellingham 1, simplecalc 1, typer 1, typing-extensions 1
DEBUG marker environment resolution took 0.117s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.9", version: "3.12.9" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "6.11.0-19-generic", platform_system: "Linux", platform_version: "#19~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Mon Feb 17 11:51:52 UTC 2", python_full_version: StringVersion { string: "3.12.9", version: "3.12.9" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> simplecalc
TRACE Resolution edge:     0a0.dev0 -> 0.3.0
TRACE Resolution edge: markdown-it-py -> mdurl
TRACE Resolution edge:     3.0.0 -> 0.1.2
TRACE Resolution edge: rich -> markdown-it-py
TRACE Resolution edge:     13.9.4 -> 3.0.0
TRACE Resolution edge: rich -> pygments
TRACE Resolution edge:     13.9.4 -> 2.19.1
TRACE Resolution edge: typer -> click
TRACE Resolution edge:     0.15.2 -> 8.1.8
TRACE Resolution edge: typer -> typing-extensions
TRACE Resolution edge:     0.15.2 -> 4.12.2
TRACE Resolution edge: typer -> shellingham
TRACE Resolution edge:     0.15.2 -> 1.5.4
TRACE Resolution edge: typer -> rich
TRACE Resolution edge:     0.15.2 -> 13.9.4
TRACE Resolution edge: simplecalc -> typer
TRACE Resolution edge:     0.3.0 -> 0.15.2
Resolved 9 packages in 117ms
DEBUG Identified uncached distribution: markdown-it-py==3.0.0
DEBUG Identified uncached distribution: shellingham==1.5.4
DEBUG Identified uncached distribution: simplecalc==0.3.0
DEBUG Identified uncached distribution: rich==13.9.4
DEBUG Identified uncached distribution: typer==0.15.2
DEBUG Identified uncached distribution: pygments==2.19.1
DEBUG Identified uncached distribution: typing-extensions==4.12.2
DEBUG Identified uncached distribution: click==8.1.8
DEBUG Identified uncached distribution: mdurl==0.1.2
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/simplecalc/0.3.0-py3-none-any.http
DEBUG No cache entry for: http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Sending fresh GET request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Handling request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is unauthenticated, checking cache
TRACE Found cached credentials for URL http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is fully authenticated
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/pygments/2.19.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/rich/13.9.4-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/click/8.1.8-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/markdown-it-py/3.0.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/typer/0.15.2-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/typing-extensions/4.12.2-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/mdurl/0.1.2-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
TRACE No cache entry exists for /home/appuser/.cache/uv/wheels-v5/index/c043adf7183e3e19/shellingham/1.5.4-py2.py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
TRACE Cached request http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is storable because its response has a heuristically cacheable status code 200
TRACE Cached request https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
Downloading pygments (1.2MiB)
TRACE Cached request https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl is storable because its response has a 'public' cache-control directive
 Downloaded pygments
Prepared 9 packages in 60ms
TRACE Extracting file name=PackageName("mdurl")
TRACE Extracting file name=PackageName("simplecalc")
TRACE Extracting file name=PackageName("click")
TRACE Extracting file name=PackageName("shellingham")
TRACE Extracting file name=PackageName("pygments")
TRACE Extracting file name=PackageName("rich")
TRACE Extracting file name=PackageName("typing-extensions")
TRACE Extracting file name=PackageName("markdown-it-py")
TRACE Extracting file name=PackageName("typer")
TRACE Extracted 7 files name=PackageName("simplecalc")
TRACE Extracted 5 files name=PackageName("typing-extensions")
TRACE Extracted 11 files name=PackageName("mdurl")
TRACE No entrypoints name=PackageName("typing-extensions")
TRACE No entrypoints name=PackageName("mdurl")
TRACE No data name=PackageName("typing-extensions")
TRACE No data name=PackageName("mdurl")
TRACE Writing installer metadata name=PackageName("typing-extensions")
TRACE Writing installer metadata name=PackageName("mdurl")
TRACE Extracted 13 files name=PackageName("shellingham")
TRACE Extracted 21 files name=PackageName("click")
TRACE No entrypoints name=PackageName("shellingham")
TRACE No data name=PackageName("shellingham")
TRACE Writing installer metadata name=PackageName("shellingham")
TRACE No entrypoints name=PackageName("click")
TRACE No data name=PackageName("click")
TRACE Writing installer metadata name=PackageName("click")
TRACE Writing record name=PackageName("typing-extensions")
TRACE Writing record name=PackageName("mdurl")
TRACE Extracted 21 files name=PackageName("typer")
TRACE Writing record name=PackageName("shellingham")
TRACE Writing record name=PackageName("click")
TRACE Extracted 83 files name=PackageName("rich")
TRACE Writing entrypoints name=PackageName("simplecalc")
TRACE No entrypoints name=PackageName("rich")
TRACE No data name=PackageName("rich")
TRACE Writing installer metadata name=PackageName("rich")
TRACE Writing entrypoints name=PackageName("typer")
TRACE No data name=PackageName("simplecalc")
TRACE Writing installer metadata name=PackageName("simplecalc")
TRACE Extracted 74 files name=PackageName("markdown-it-py")
TRACE Writing record name=PackageName("rich")
TRACE No data name=PackageName("typer")
TRACE Writing installer metadata name=PackageName("typer")
TRACE Writing entrypoints name=PackageName("markdown-it-py")
TRACE Writing record name=PackageName("simplecalc")
TRACE Writing record name=PackageName("typer")
TRACE No data name=PackageName("markdown-it-py")
TRACE Writing installer metadata name=PackageName("markdown-it-py")
TRACE Writing record name=PackageName("markdown-it-py")
TRACE Extracted 343 files name=PackageName("pygments")
TRACE Writing entrypoints name=PackageName("pygments")
TRACE No data name=PackageName("pygments")
TRACE Writing installer metadata name=PackageName("pygments")
TRACE Writing record name=PackageName("pygments")
Installed 9 packages in 5ms
 + click==8.1.8
 + markdown-it-py==3.0.0
 + mdurl==0.1.2
 + pygments==2.19.1
 + rich==13.9.4
 + shellingham==1.5.4
 + simplecalc==0.3.0
 + typer==0.15.2
 + typing-extensions==4.12.2
DEBUG Released lock at `/home/appuser/app/ddemo/.venv/.lock`
```

**Example 2: Invalid auth - Fails with 401**
```
(ddemo) appuser@desktop:~/app/ddemo$ uv pip install simplecalc --no-cache -vv
DEBUG uv 0.6.9
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /home/appuser/app/ddemo/.venv/bin/python3
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/appuser/app/ddemo/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: simplecalc
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: simplecalc*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: simplecalc. remaining choices:
TRACE Fetching metadata for simplecalc from http://localhost:8080/simple/simplecalc/
TRACE No cache entry exists for /tmp/.tmpTckpDc/simple-v15/index/c043adf7183e3e19/simplecalc.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/simplecalc/
TRACE Sending fresh GET request for http://localhost:8080/simple/simplecalc/
TRACE Handling request for http://localhost:8080/simple/simplecalc/
TRACE Request for http://localhost:8080/simple/simplecalc/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/simplecalc/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/simplecalc/
TRACE Cached request http://localhost:8080/simple/simplecalc/ is storable because its response has a heuristically cacheable status code 200
TRACE Received package metadata for: simplecalc
TRACE Selecting candidate for simplecalc with range * with 1 remote versions
DEBUG Searching for a compatible version of simplecalc (*)
TRACE Selecting candidate for simplecalc with range * with 1 remote versions
TRACE Found candidate for package simplecalc with range * after 1 steps: 0.3.0 version
TRACE Found candidate for package simplecalc with range * after 1 steps: 0.3.0 version
TRACE Returning candidate for package simplecalc with range * after 1 steps
TRACE Returning candidate for package simplecalc with range * after 1 steps
DEBUG Selecting: simplecalc==0.3.0 [compatible] (simplecalc-0.3.0-py3-none-any.whl)
TRACE No cache entry exists for /tmp/.tmpTckpDc/wheels-v5/index/c043adf7183e3e19/simplecalc/0.3.0-py3-none-any.msgpack
DEBUG No cache entry for: http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Sending fresh HEAD request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Handling request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Attempting unauthenticated request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm http://localhost:8080
DEBUG Checking netrc for credentials for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
DEBUG Found credentials in netrc file for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Retrying request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 with Credentials { username: Username(Some("testuser")), password: Some("testpass123") }
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "http", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("localhost")), port: Some(8080), path: "/packages/simplecalc-0.3.0-py3-none-any.whl", query: None, fragment: Some("sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857") }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857" }))) }
TRACE Cannot retry error: not an IO error
WARN Range requests not supported for simplecalc-0.3.0-py3-none-any.whl; streaming wheel
TRACE No cache entry exists for /tmp/.tmpTckpDc/wheels-v5/index/c043adf7183e3e19/simplecalc/0.3.0-py3-none-any.msgpack
DEBUG No cache entry for: http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Sending fresh GET request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Handling request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Attempting unauthenticated request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm http://localhost:8080
TRACE Using credentials from previous fetch for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857
TRACE Retrying request for http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857 with Credentials { username: Username(Some("testuser")), password: Some("testpass123") }
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "http", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("localhost")), port: Some(8080), path: "/packages/simplecalc-0.3.0-py3-none-any.whl", query: None, fragment: Some("sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857") }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Released lock at `/home/appuser/app/ddemo/.venv/.lock`
error: Failed to fetch: `http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857`
  Caused by: HTTP status client error (403 Forbidden) for url (http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl#sha256=4a910604b5445b87b10e5ea300407cd3521d7354db921c8fbabe8b00353a5857)

```

**Example 3: Invalid auth, not allowed to list so falls back to PyPi silently:**
```
(ddemo) appuser@desktop:~/app/ddemo$ uv pip install simplecalc -vv --no-cache
DEBUG uv 0.6.9
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /home/appuser/app/ddemo/.venv/bin/python3
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/appuser/app/ddemo/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: simplecalc
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: simplecalc*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: simplecalc. remaining choices:
TRACE Fetching metadata for simplecalc from http://localhost:8080/simple/simplecalc/
TRACE No cache entry exists for /tmp/.tmp9KbQch/simple-v15/index/c043adf7183e3e19/simplecalc.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/simplecalc/
TRACE Sending fresh GET request for http://localhost:8080/simple/simplecalc/
TRACE Handling request for http://localhost:8080/simple/simplecalc/
TRACE Request for http://localhost:8080/simple/simplecalc/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/simplecalc/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/simplecalc/
TRACE Request for http://localhost:8080/simple/simplecalc/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm http://localhost:8080
DEBUG Checking netrc for credentials for http://localhost:8080/simple/simplecalc/
DEBUG Found credentials in netrc file for http://localhost:8080/simple/simplecalc/
TRACE Retrying request for http://localhost:8080/simple/simplecalc/ with Credentials { username: Username(Some("testuser")), password: Some("testpass123") }
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "http", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("localhost")), port: Some(8080), path: "/simple/simplecalc/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "http://localhost:8080/simple/simplecalc/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for simplecalc from https://pypi.org/simple/simplecalc/
TRACE No cache entry exists for /tmp/.tmp9KbQch/simple-v15/pypi/simplecalc.rkyv
DEBUG No cache entry for: https://pypi.org/simple/simplecalc/
TRACE Sending fresh GET request for https://pypi.org/simple/simplecalc/
TRACE Handling request for https://pypi.org/simple/simplecalc/
TRACE Request for https://pypi.org/simple/simplecalc/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/simplecalc/
TRACE Attempting unauthenticated request for https://pypi.org/simple/simplecalc/
TRACE Cached request https://pypi.org/simple/simplecalc/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: simplecalc
TRACE Selecting candidate for simplecalc with range * with 1 remote versions
DEBUG Searching for a compatible version of simplecalc (*)
TRACE Selecting candidate for simplecalc with range * with 1 remote versions
TRACE Found candidate for package simplecalc with range * after 1 steps: 0.1.0 version
TRACE Returning candidate for package simplecalc with range * after 1 steps
TRACE Found candidate for package simplecalc with range * after 1 steps: 0.1.0 version
TRACE Returning candidate for package simplecalc with range * after 1 steps
DEBUG Selecting: simplecalc==0.1.0 [compatible] (simplecalc-0.1.0-py3-none-any.whl)
TRACE No cache entry exists for /tmp/.tmp9KbQch/wheels-v5/pypi/simplecalc/0.1.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: simplecalc==0.1.0
DEBUG Adding transitive dependency for simplecalc==0.1.0: click>=7.0, <8.0
DEBUG Adding transitive dependency for simplecalc==0.1.0: toml>=0.10.0, <0.11.0
TRACE Fetching metadata for click from http://localhost:8080/simple/click/
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.1.0
TRACE Chose package for decision: click. remaining choices: toml
TRACE Fetching metadata for toml from http://localhost:8080/simple/toml/
TRACE No cache entry exists for /tmp/.tmp9KbQch/simple-v15/index/c043adf7183e3e19/click.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/click/
TRACE Sending fresh GET request for http://localhost:8080/simple/click/
TRACE Handling request for http://localhost:8080/simple/click/
TRACE Request for http://localhost:8080/simple/click/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/click/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/click/
TRACE No cache entry exists for /tmp/.tmp9KbQch/simple-v15/index/c043adf7183e3e19/toml.rkyv
DEBUG No cache entry for: http://localhost:8080/simple/toml/
TRACE Sending fresh GET request for http://localhost:8080/simple/toml/
TRACE Handling request for http://localhost:8080/simple/toml/
TRACE Request for http://localhost:8080/simple/toml/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost:8080/simple/toml/
TRACE Attempting unauthenticated request for http://localhost:8080/simple/toml/
TRACE Request for http://localhost:8080/simple/click/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm http://localhost:8080
TRACE Using credentials from previous fetch for http://localhost:8080/simple/click/
TRACE Retrying request for http://localhost:8080/simple/click/ with Credentials { username: Username(Some("testuser")), password: Some("testpass123") }
TRACE Request for http://localhost:8080/simple/toml/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm http://localhost:8080
TRACE Using credentials from previous fetch for http://localhost:8080/simple/toml/
TRACE Retrying request for http://localhost:8080/simple/toml/ with Credentials { username: Username(Some("testuser")), password: Some("testpass123") }
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "http", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("localhost")), port: Some(8080), path: "/simple/click/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "http://localhost:8080/simple/click/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for click from https://pypi.org/simple/click/
TRACE No cache entry exists for /tmp/.tmp9KbQch/simple-v15/pypi/click.rkyv
DEBUG No cache entry for: https://pypi.org/simple/click/
TRACE Sending fresh GET request for https://pypi.org/simple/click/
TRACE Handling request for https://pypi.org/simple/click/
TRACE Request for https://pypi.org/simple/click/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/click/
TRACE Attempting unauthenticated request for https://pypi.org/simple/click/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "http", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("localhost")), port: Some(8080), path: "/simple/toml/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "http://localhost:8080/simple/toml/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for toml from https://pypi.org/simple/toml/
TRACE No cache entry exists for /tmp/.tmp9KbQch/simple-v15/pypi/toml.rkyv
DEBUG No cache entry for: https://pypi.org/simple/toml/
TRACE Sending fresh GET request for https://pypi.org/simple/toml/
TRACE Handling request for https://pypi.org/simple/toml/
TRACE Request for https://pypi.org/simple/toml/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/toml/
TRACE Attempting unauthenticated request for https://pypi.org/simple/toml/
TRACE Cached request https://pypi.org/simple/click/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: click
TRACE Selecting candidate for click with range >=7.0, <8.0 with 54 remote versions
DEBUG Searching for a compatible version of click (>=7.0, <8.0)
TRACE Selecting candidate for click with range >=7.0, <8.0 with 54 remote versions
TRACE Found candidate for package click with range >=7.0, <8.0 after 1 steps: 7.1.2 version
TRACE Found candidate for package click with range >=7.0, <8.0 after 1 steps: 7.1.2 version
TRACE Returning candidate for package click with range >=7.0, <8.0 after 1 steps
TRACE Returning candidate for package click with range >=7.0, <8.0 after 1 steps
DEBUG Selecting: click==7.1.2 [compatible] (click-7.1.2-py2.py3-none-any.whl)
TRACE No cache entry exists for /tmp/.tmp9KbQch/wheels-v5/pypi/click/7.1.2-py2.py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl.metadata
TRACE Cached request https://pypi.org/simple/toml/ is storable because its response has a 'public' cache-control directive
WARN Skipping file for toml: toml-0.10.0-py2.7.egg
TRACE Received package metadata for: toml
TRACE Selecting candidate for toml with range >=0.10.0, <0.11.0 with 16 remote versions
TRACE Found candidate for package toml with range >=0.10.0, <0.11.0 after 1 steps: 0.10.2 version
TRACE Returning candidate for package toml with range >=0.10.0, <0.11.0 after 1 steps
TRACE No cache entry exists for /tmp/.tmp9KbQch/wheels-v5/pypi/toml/0.10.2-py2.py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: click==7.1.2
TRACE Received built distribution metadata for: toml==0.10.2
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.1.0, click==7.1.2
TRACE Chose package for decision: toml. remaining choices:
DEBUG Searching for a compatible version of toml (>=0.10.0, <0.11.0)
TRACE Selecting candidate for toml with range >=0.10.0, <0.11.0 with 16 remote versions
TRACE Found candidate for package toml with range >=0.10.0, <0.11.0 after 1 steps: 0.10.2 version
TRACE Returning candidate for package toml with range >=0.10.0, <0.11.0 after 1 steps
DEBUG Selecting: toml==0.10.2 [compatible] (toml-0.10.2-py2.py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, simplecalc==0.1.0, click==7.1.2, toml==0.10.2
DEBUG Tried 3 versions: click 1, simplecalc 1, toml 1
DEBUG marker environment resolution took 0.091s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.9", version: "3.12.9" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "6.11.0-19-generic", platform_system: "Linux", platform_version: "#19~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Mon Feb 17 11:51:52 UTC 2", python_full_version: StringVersion { string: "3.12.9", version: "3.12.9" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> simplecalc
TRACE Resolution edge:     0a0.dev0 -> 0.1.0
TRACE Resolution edge: simplecalc -> click
TRACE Resolution edge:     0.1.0 -> 7.1.2
TRACE Resolution edge: simplecalc -> toml
TRACE Resolution edge:     0.1.0 -> 0.10.2
Resolved 3 packages in 91ms
DEBUG Identified uncached distribution: toml==0.10.2
DEBUG Identified uncached distribution: simplecalc==0.1.0
DEBUG Identified uncached distribution: click==7.1.2
TRACE No cache entry exists for /tmp/.tmp9KbQch/wheels-v5/pypi/click/7.1.2-py2.py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl
TRACE No cache entry exists for /tmp/.tmp9KbQch/wheels-v5/pypi/toml/0.10.2-py2.py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl
TRACE No cache entry exists for /tmp/.tmp9KbQch/wheels-v5/pypi/simplecalc/0.1.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl
TRACE Cached request https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/d2/3d/fa76db83bf75c4f8d338c2fd15c8d33fdd7ad23a9b5e57eb6c5de26b430e/click-7.1.2-py2.py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Cached request https://files.pythonhosted.org/packages/d7/ea/d80cd703defa72a36857e5ff325ed079b3fc005bd8ea39877779e93fff08/simplecalc-0.1.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
Prepared 3 packages in 18ms
TRACE Extracting file name=PackageName("click")
TRACE Extracting file name=PackageName("toml")
TRACE Extracting file name=PackageName("simplecalc")
TRACE Extracted 10 files name=PackageName("toml")
TRACE No entrypoints name=PackageName("toml")
TRACE No data name=PackageName("toml")
TRACE Writing installer metadata name=PackageName("toml")
TRACE Extracted 22 files name=PackageName("click")
TRACE No entrypoints name=PackageName("click")
TRACE No data name=PackageName("click")
TRACE Writing installer metadata name=PackageName("click")
TRACE Extracted 14 files name=PackageName("simplecalc")
TRACE Writing record name=PackageName("toml")
TRACE Writing record name=PackageName("click")
TRACE Writing entrypoints name=PackageName("simplecalc")
TRACE No data name=PackageName("simplecalc")
TRACE Writing installer metadata name=PackageName("simplecalc")
TRACE Writing record name=PackageName("simplecalc")
Installed 3 packages in 2ms
 + click==7.1.2
 + simplecalc==0.1.0
 + toml==0.10.2
DEBUG Released lock at `/home/appuser/app/ddemo/.venv/.lock`
```


---

_Comment by @jtfmumm on 2025-03-24 17:07_

Thanks for providing these logs. Would you mind also sharing your `pyproject.toml`?

---

_Comment by @bendikhk on 2025-03-24 18:46_

> Thanks for providing these logs. Would you mind also sharing your `pyproject.toml`?

In the examples above I only used ` uv pip install simplecalc` without any pyproject.toml.

I've added the code to reproduce different scenarios [here](https://github.com/bendikhk/uv-auth-example)

---

_Comment by @bendikhk on 2025-03-24 19:06_

But you can see the same issue with a pyproject.toml like below:
```
[project]
name = "ddemo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "simplecalc>=0.1.0",
]
```

```
# Start server where you are **NOT** allowed to list without authentication:
$ uv run pypi-server run -p 8080 -P ~/.htpasswd -a update,download,list packages/ --verbose

# create project with pyproject above and try to sync with invalid auth
$ uv sync 
Installed 3 packages in 2ms
 + click==7.1.2
 + simplecalc==0.1.0 # from pypi
 + toml==0.10.2

```
However if you cloned the repo with a lockfile for example it would fail with:
```
  × Failed to download `simplecalc==0.3.0`
  ├─▶ Failed to fetch:
  │   `http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl`
  ╰─▶ HTTP status client error (403 Forbidden) for url
      (http://localhost:8080/packages/simplecalc-0.3.0-py3-none-any.whl)
  help: `simplecalc` (v0.3.0) was included because `ddemo` (v0.1.0) depends on
        `simplecalc`
```

Just to show that `uv sync`and `uv add` has the same behaviour.

---

_Comment by @bendikhk on 2025-04-08 12:25_

Hi again,

I see now that this is properly documented in the [documentation](https://docs.astral.sh/uv/configuration/indexes/#using-credential-providers), which is good!

> If the discovered credentials are not valid (i.e., the index returns a HTTP 401 or 403), then uv will treat packages as unavailable and query the next configured index as described in the [index strategy](https://docs.astral.sh/uv/configuration/indexes/#searching-across-multiple-indexes) section.

However, the current implementation is still a huge risk factor for us. If someone forgets to enable vpn, or have an invalid password they are open for "dependency confusion" attacks. And I would still argue that with the "first-index" strategy and authenticate always it should fail hard when not being able to access the index. Are there any plans to change this behavior in the near future, or perhaps introduce a setting that allows users to opt into stricter index access checks for cases like ours?


---

_Comment by @jtfmumm on 2025-04-08 13:55_

@bendikhk I am actually working on a design to address this very problem. I hope to begin work on the implementation in the near future.

---

_Comment by @bendikhk on 2025-04-08 19:12_

> [@bendikhk](https://github.com/bendikhk) I am actually working on a design to address this very problem. I hope to begin work on the implementation in the near future.

Great! Let me know if I can help with some testing! :)

---

_Comment by @sfentonn on 2025-04-25 22:13_

Forgive me if I'm not understanding the current issue, but is this not what default = true does? I'm configuring something similar with artifactory and it seems to be working for me. https://docs.astral.sh/uv/configuration/indexes/#defining-an-index "To exclude PyPI from the list of indexes, set default = true on another index entry".

---

_Comment by @zanieb on 2025-04-25 22:17_

Yeah, `default = true` also solves this problem. Some users need PyPI for some of their packages still.

---

_Closed by @zanieb on 2025-04-29 21:37_

---
