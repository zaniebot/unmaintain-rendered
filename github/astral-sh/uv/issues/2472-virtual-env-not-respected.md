---
number: 2472
title: VIRTUAL_ENV not respected
type: issue
state: closed
author: PrinceJunkie
labels:
  - question
assignees: []
created_at: 2024-03-15T11:12:36Z
updated_at: 2024-03-16T15:08:03Z
url: https://github.com/astral-sh/uv/issues/2472
synced_at: 2026-01-07T13:12:17-06:00
---

# VIRTUAL_ENV not respected

---

_Issue opened by @PrinceJunkie on 2024-03-15 11:12_

I'm using uv primarily in a devcontainer where I don't have root rights and I don't want a .venv folder in the project folder.
Therefore I've set VIRTUAL_ENV to /home/dev/.local and everything should be installed there but right now it throws a Permission denied with a path which is not VIRTUAL_ENV.
  
I was using this exact setup before and it was working fine. Unfortunately I don't know the version in which it worked. It seems like VIRTUAL_ENV is not respected anymore.


Here's my devcontainer docker file:
Dockerfile:
```dockerfile
ARG PYTHON_VERSION="3.12"

FROM mcr.microsoft.com/vscode/devcontainers/python:${PYTHON_VERSION}-bullseye
ENV VIRTUAL_ENV=/home/dev/.local \
    PYTHONUNBUFFERED=1 \
    DEBIAN_FRONTEND=noninteractive

RUN pip install --upgrade pip \
    && rm /usr/bin/python3 && ln -s /usr/local/bin/python /usr/bin/python \
    && usermod -l dev -d /home/dev -m vscode \
    && sed -i 's/vscode/dev/' /etc/sudoers.d/vscode \
    && mv /etc/sudoers.d/vscode /etc/sudoers.d/dev \
    # Install uv as dev user
    && wget https://astral.sh/uv/install.sh && chmod 755 install.sh && sudo -H -u dev /install.sh && rm /install.sh
```

and here's what I'm running in the postCreateCommand (basically executing this as the dev user in a shell)
```bash
PYTHON_VERSION=$(python3 -c "import sys; print(f'python{sys.version_info.major}.{sys.version_info.minor}')")
mkdir -p $HOME/.local/bin
mkdir -p $HOME/.local/lib/$PYTHON_VERSION/site-packages
ln -s /usr/local/bin/python $HOME/.local/bin/python
uv pip install -r requirements-dev.txt
```

Here's the output while trying to e.g. install pytest:
```bash
dev ➜ /project $ echo $VIRTUAL_ENV    
/home/dev/.local
dev ➜ /project $ uv pip install pytest --verbose
DEBUG Found a virtualenv through VIRTUAL_ENV at: /home/dev/.local
DEBUG Cached interpreter info for Python 3.12.2, skipping probing: /home/dev/.local/bin/python
DEBUG Using Python 3.12.2 environment at /home/dev/.local/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.12.2
DEBUG Adding direct dependency: pytest*
DEBUG Found fresh response for: https://pypi.org/simple/pytest/
DEBUG Searching for a compatible version of pytest (*)
DEBUG Selecting: pytest==8.1.1 (pytest-8.1.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4d/7e/c79cecfdb6aa85c6c2e3cf63afc56d0f165f24f5c66c03c695c4d9b84756/pytest-8.1.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency: iniconfig*
DEBUG Adding transitive dependency: packaging*
DEBUG Adding transitive dependency: pluggy>=1.4, <2.0
DEBUG Found fresh response for: https://pypi.org/simple/iniconfig/
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of iniconfig (*)
DEBUG Selecting: iniconfig==2.0.0 (iniconfig-2.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a5/5b/0cc789b59e8cc1bf288b38111d002d8c5917123194d45b29dcdac64723cc/pluggy-1.4.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/49/df/1fceb2f8900f8639e278b056416d49134fb8d84c5942ffaa01ad34782422/packaging-24.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (*)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.4, <2.0)
DEBUG Selecting: pluggy==1.4.0 (pluggy-1.4.0-py3-none-any.whl)
Resolved 4 packages in 6ms
DEBUG Requirement already cached: iniconfig==2.0.0
DEBUG Requirement already cached: packaging==24.0
DEBUG Requirement already cached: pluggy==1.4.0
DEBUG Requirement already cached: pytest==8.1.1
DEBUG Unnecessary package: gitpython==3.1.41
DEBUG Unnecessary package: gitdb==4.0.11
DEBUG Preserving seed package: setuptools==69.0.3
DEBUG Unnecessary package: smmap==5.0.1
DEBUG Preserving seed package: pip==24.0
DEBUG Preserving seed package: wheel==0.42.0
error: Failed to install: iniconfig-2.0.0-py3-none-any.whl (iniconfig==2.0.0)
  Caused by: failed to create directory `/usr/local/lib/python3.12/site-packages/iniconfig`
  Caused by: Permission denied (os error 13)
```
* The current uv platform.
Devcontainer: mcr.microsoft.com/vscode/devcontainers/python:3.12-bullseye on Apple Silicon
* The current uv version (`uv --version`).
uv 0.1.20

---

_Comment by @helderco on 2024-03-15 11:37_

To properly setup a virtual environment you need to update PATH as well:
```Dockerfile
ENV VIRTUAL_ENV=/home/dev/.local
ENV PATH=$VIRTUAL_ENV/bin:$PATH
```
Curious why you do this:
```
PYTHON_VERSION=$(python3 -c "import sys; print(f'python{sys.version_info.major}.{sys.version_info.minor}')")
mkdir -p $HOME/.local/bin
mkdir -p $HOME/.local/lib/$PYTHON_VERSION/site-packages
ln -s /usr/local/bin/python $HOME/.local/bin/python
```
Can just create the venv with:
```
uv venv $VIRTUAL_ENV
```

---

_Comment by @PrinceJunkie on 2024-03-15 11:46_

Setting PATH didn't change anything.

If you don't have a python file under the VIRTUAL_ENV/bin path it fails:
```
error: failed to canonicalize path `/home/dev/.local/bin/python`
  Caused by: No such file or directory (os error 2)
```


Didn't think about using uv venv, thanks for that!

---

_Comment by @helderco on 2024-03-15 11:51_

> If you don't have a python file under the VIRTUAL_ENV/bin path it fails:

Depends on what you do between that and creating it:

```console
$ docker run --rm -it python:3.11-slim bash
# pip install uv
# export VIRTUAL_ENV=$HOME/.local/dev
# export PATH=$VIRTUAL_ENV/bin:$PATH
# uv venv $VIRTUAL_ENV
Using Python 3.11.8 interpreter at: /usr/local/bin/python3
Creating virtualenv at: /root/.local/dev
# uv pip install typer-cli
Resolved 5 packages in 811ms
Downloaded 5 packages in 215ms
Installed 5 packages in 5ms
 + click==8.1.7
 + colorama==0.4.6
 + shellingham==1.4.0
 + typer==0.7.0
 + typer-cli==0.0.13
# which typer
/root/.local/dev/bin/typer
```

Can also set VIRTUAL_ENV, create it, then update PATH if there's stuff that needs to be in between.

---

_Comment by @PrinceJunkie on 2024-03-15 12:01_

```uv venv $VIRTUAL_ENV``` already creates the python file there, that's why its working in your example I guess.

Also I rely also on pip for my workflow. Using uv venv breaks somehow pip:

```bash
dev ➜ /eventbridge (chore/refactoring_v2) $ uv venv $VIRTUAL_ENV 
Using Python 3.12.2 interpreter at: /usr/local/bin/python3
Creating virtualenv at: /home/dev/.local
Activate with: source /home/dev/.local/bin/activate
dev ➜ /eventbridge (chore/refactoring_v2) $ python3.12 -m pip
/home/dev/.local/bin/python3.12: No module named pip
```

but doing it with the above method works fine.

I'm using [serverless-python-requirements](https://github.com/serverless/serverless-python-requirements) which does the above call of pip in this format.

---

_Comment by @helderco on 2024-03-15 13:13_

If you need pip you can `--seed` the venv:

```console
# uv venv --seed $VIRTUAL_ENV
Using Python 3.11.8 interpreter at: /usr/local/bin/python3
Creating virtualenv at: /root/.local/dev
 + pip==24.0
 + setuptools==69.2.0
 + wheel==0.43.0
# pip --version
pip 24.0 from /root/.local/dev/lib/python3.11/site-packages/pip (python 3.11)
```
Same result with `python -m pip --version`.

---

_Comment by @charliermarsh on 2024-03-15 13:36_

Yeah, I'd suggest using `uv venv --seed` and running within a virtual environment.

---

_Label `question` added by @charliermarsh on 2024-03-15 13:58_

---

_Comment by @PrinceJunkie on 2024-03-16 14:15_

Thanks, that worked but I don't know why. Could someone of you explain it to me why using `uv venv --seed `doesn't end in a permission denied error?

---

_Comment by @charliermarsh on 2024-03-16 14:23_

Likely because if you run `pip` outside of the virtualenv, or without `--seed`, that will be the system `pip`, and so you'll get an error when you try to `pip install`. If you use `--seed`, then we'll include a version of `pip` in the virtualenv, so once you activate it, that `pip install` will install into the virtual environment, which avoids any permission denied issues.

---

_Comment by @charliermarsh on 2024-03-16 14:24_

(We don't include `pip` in the virtualenv by default (and hence require `--seed`) because in most cases you shouldn't need it if you're already using `uv`. It's supported, of course, via `--seed`, but not the default.)

---

_Comment by @charliermarsh on 2024-03-16 14:26_

(Gonna close but happy to answer any more questions!)

---

_Closed by @charliermarsh on 2024-03-16 14:26_

---

_Comment by @PrinceJunkie on 2024-03-16 14:27_

> Likely because if you run `pip` outside of the virtualenv, or without `--seed`, that will be the system `pip`, and so you'll get an error when you try to `pip install`. If you use `--seed`, then we'll include a version of `pip` in the virtualenv, so once you activate it, that `pip install` will install into the virtual environment, which avoids any permission denied issues.

Yeah but I was getting the problem while using `uv pip install pytest` or did I miss sth. obvious?

---

_Comment by @charliermarsh on 2024-03-16 14:57_

Ah yeah. I think the issue there is that you were using `VIRTUAL_ENV` to point to `/home/dev/.local`. But that's not quite the same as installing into a virtual environment, even one rooted at `/home/dev/.local`, since when uv asks the Python interpreter for the "correct install path", it will tell it to use a system path that will then yield a permission error.

---
