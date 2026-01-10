```yaml
number: 2282
title: uv ignores system installed packages
type: issue
state: closed
author: RonNabuurs
labels:
  - bug
assignees: []
created_at: 2024-03-07T15:52:19Z
updated_at: 2025-01-28T07:33:16Z
url: https://github.com/astral-sh/uv/issues/2282
synced_at: 2026-01-10T04:27:57Z
```

# uv ignores system installed packages

---

_Issue opened by @RonNabuurs on 2024-03-07 15:52_

While trying to implement uv in my current pipeline I came across an issue regarding dependencies. 

To sketch a summary of my setup. 

I'm running my pipeline in alpine, which brings some challenges. Some packages cannot be easily compiled (and don't have a wheel available), which is why I use the extra index https://github.com/alpine-wheels/index . 

I use that index for packages like `mysqlcient` and `reportlab`.

My pipeline also  wants to install cryptography==41.0.3, but because cryptography is available in the alpine-wheel index, it tries to install it from there. Though cryptography already has available wheels via pypi, so I don't want it  to install via the alpine-wheels repo. 

My thought was to first run uv once to only install cryptography, so I dont use the index. And after that I run uv with my normal dependencies via pyproject.toml. But it appears that uv doesn't see the already installed cryptography.

Because I run uv in a pipeline I use the --system flag. 

See the output below:
```
/ # uv pip install --system cryptography==41.0.3
Resolved 3 packages in 114ms
Downloaded 3 packages in 114ms
Installed 3 packages in 6ms
 + cffi==1.16.0
 - cryptography==40.0.2
 + cryptography==41.0.3
 + pycparser==2.21
/ # uv pip install --system --extra-index-url https://alpine-wheels.github.io/index -r pyproject.toml
  x No solution found when resolving dependencies:
  `-> Because there is no version of cryptography==41.0.3 and <my-project> depends on cryptography==41.0.3, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @charliermarsh on 2024-03-07 15:56_

I believe this is the same as https://github.com/astral-sh/uv/issues/1661. We don't consider the current environment to be a "source" for identifying packages, though we probably should. (Merging into #1661.)

---

_Closed by @charliermarsh on 2024-03-07 15:56_

---

_Label `bug` added by @charliermarsh on 2024-03-07 15:56_

---

_Comment by @zanieb on 2024-04-01 21:31_

Hi! This should be addressed in the latest release ([0.1.27](https://github.com/astral-sh/uv/releases/tag/0.1.27)) via #2596.

Let us know if you have any problems.

---

_Comment by @RonNabuurs on 2024-04-05 12:12_

> Hi! This should be addressed in the latest release ([0.1.27](https://github.com/astral-sh/uv/releases/tag/0.1.27)) via #2596.
> 
> Let us know if you have any problems.

This issue is still persistent in my pipelines using uv 0.1.29. 

My test scenario:
```bash
docker run -it --rm alpine:3.18 sh
```

```sh
apk add python3 py3-pip
apk add py3-mysqlclient
apk add curl
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.cargo/env
uv pip install --system mysqlclient==2.1.1
```



---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-05 13:14_

---

_Comment by @charliermarsh on 2024-04-05 13:14_

I can take a look.

---

_Comment by @charliermarsh on 2024-04-05 13:18_

Oh, I think the issue here is that those packages are installed as eggs, which we don't support:

```
/ # ls /usr/lib/python3.11/site-packages
MySQLdb                            distutils-precedence.pth           packaging-23.1.dist-info           pkg_resources                      setuptools
README.txt                         mysqlclient-2.1.1-py3.11.egg-info  pip                                pyparsing                          setuptools-67.7.2-py3.11.egg-info
_distutils_hack                    packaging                          pip-23.1.2-py3.11.egg-info         pyparsing-3.0.9.dist-info
```

---

_Comment by @michael-o on 2025-01-27 20:04_

Although this might sound stupid, but it does not pick up for me as well:
```
$ uv pip list
Using Python 3.10.16 environment at: /usr/local
Package           Version
----------------- -------
packaging         24.2
pip               23.3.2
psycopg           3.1.20
psycopg-c         3.1.20
psycopg-pool      3.2.4
setuptools        63.1.0
typing-extensions 4.12.2
$ ls /usr/local/lib/python3.10/site-packages
__pycache__/                        packaging/                          psycopg/                            psycopg-3.1.20.dist-info/           typing_extensions.py
_distutils_hack/                    packaging-24.2.dist-info/           psycopg_c/                          README.txt
distutils-precedence.pth            pip/                                psycopg_c-3.1.20.dist-info/         setuptools/
easy-install.pth                    pip-23.3.2-py3.10.egg-info/         psycopg_pool/                       setuptools-63.1.0-py3.10.egg-info/
easy-install.pth.dist               pkg_resources/                      psycopg_pool-3.2.4.dist-info/       typing_extensions-4.12.2.dist-info/
```
but uv still insists on locally compiling latest version of psycopg_c:
```
$ uv sync
Using CPython 3.10.16 interpreter at: /usr/local/bin/python3.10
Creating virtual environment at: .venv
Resolved 70 packages in 1.05s
   Built brotli==1.1.0
   Built cryptography==44.0.0
   Built psycopg-c==3.2.4
   Built cffi==1.17.1
   Built regex==2024.11.6
   Built pydantic-core==2.27.2
   Built pyyaml==6.0.2
Prepared 7 packages in 2m 36s
Installed 68 packages in 215ms
 + annotated-types==0.7.0
 + anyio==4.8.0
 + asgiref==3.8.1
 + brotli==1.1.0
 + certifi==2024.12.14
 + cffi==1.17.1
 + cfgv==3.4.0
 + charset-normalizer==3.4.1
 + click==8.1.8
 + colorama==0.4.6
 + crispy-tailwind==1.0.3
 + cryptography==44.0.0
 + cssbeautifier==1.15.1
 + distlib==0.3.9
 + django==5.1.5
 + django-allauth==65.3.1
 + django-browser-reload==1.17.0
 + django-cotton==1.5.2
 + django-crispy-forms==2.3
 + django-debug-toolbar==5.0.1
 + django-htmx==1.21.0
 + django-innomotics-core==0.1.18
 + django-tailwind==3.8.0
 + djlint==1.36.4
 + editorconfig==0.17.0
 + environs==14.1.0
 + exceptiongroup==1.2.2
 + filelock==3.17.0
 + fkm-guideline==0.1.1
 + h11==0.14.0
 + httpcore==1.0.7
 + httpx==0.28.1
 + identify==2.6.6
 + idna==3.10
 + iniconfig==2.0.0
 + jsbeautifier==1.15.1
 + json5==0.10.0
 + marshmallow==3.26.0
 + nodeenv==1.9.1
 + oauthlib==3.2.2
 + packaging==24.2
 + pathspec==0.12.1
 + platformdirs==4.3.6
 + pluggy==1.5.0
 + pre-commit==4.1.0
 + psycopg==3.2.4
 + psycopg-c==3.2.4
 + psycopg-pool==3.2.4
 + pycparser==2.22
 + pydantic==2.10.6
 + pydantic-core==2.27.2
 + pyjwt==2.10.1
 + pytest==8.3.4
 + pytest-django==4.9.0
 + python-dotenv==1.0.1
 + pyyaml==6.0.2
 + regex==2024.11.6
 + requests==2.32.3
 + requests-oauthlib==2.0.0
 + six==1.17.0
 + sniffio==1.3.1
 + sqlparse==0.5.3
 + tomli==2.2.1
 + tqdm==4.67.1
 + typing-extensions==4.12.2
 + urllib3==2.3.0
 + virtualenv==20.29.1
 + whitenoise==6.8.2
```
What am I missing here (psycopg isn't an egg)? Should I file a bug?

Running:
```
$ uv --version
uv 0.5.9
$ python --version
Python 3.10.16
$ uname -a
FreeBSD deblndw013x3j.innomotics.net 13.4-RELEASE FreeBSD 13.4-STABLE a0da8a5fc GENERIC amd64
```

---

_Comment by @zanieb on 2025-01-27 20:27_

`uv sync` is installing to the project environment at `.venv` — it doesn't use packages from the interpreter's system environment at `/usr/local`

---

_Comment by @michael-o on 2025-01-27 20:37_

> `uv sync` is installing to the project environment at `.venv` — it doesn't use packages from the interpreter's system environment at `/usr/local`

So none of the venv related commands will reuse global packages?
None here https://docs.astral.sh/uv/reference/cli/#uv-sync

---

_Comment by @zanieb on 2025-01-27 20:44_

No, project environments are isolated from the system environment otherwise you wouldn't be able to reproduce an environment on another machine. Packages will only be respected if they're in the actual target environment, e.g., `uv pip install -e . --system` would presumably use the existing package.

---

_Comment by @michael-o on 2025-01-27 20:46_

> No, project environments are isolated from the system environment otherwise you wouldn't be able to reproduce an environment on another machine. Packages will only be respected if they're in the actual target environment, e.g., `uv pip install -e . --system` would presumably use the existing package.

That's quite valuable and disappointing since I cannot reuse system managed psycopg-c and are forced to recompile all over. Thanks!

---

_Comment by @michael-o on 2025-01-28 07:33_




> No, project environments are isolated from the system environment otherwise you wouldn't be able to reproduce an environment on another machine. Packages will only be respected if they're in the actual target environment, e.g., `uv pip install -e . --system` would presumably use the existing package.

After reading your posts again and other doumentation I understand that uv deviates from pip and operates either in the venv or with the global cache it builds up. Thank you. That is a changer.

---
