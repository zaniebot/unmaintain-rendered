```yaml
number: 2486
title: "`python -m pip install uv` does not install stub - `pip install uv` does"
type: issue
state: closed
author: bersbersbers
labels: []
assignees: []
created_at: 2024-03-16T12:23:27Z
updated_at: 2024-08-11T15:38:32Z
url: https://github.com/astral-sh/uv/issues/2486
synced_at: 2026-01-12T15:58:38Z
```

# `python -m pip install uv` does not install stub - `pip install uv` does

---

_@bersbersbers_

I am not even sure this is a bug in `uv` (rather than in `pip` or `pyenv`), but since I have never noticed such behavior (e.g., with `pylint`), I thought it made sense to report it.

Basically, `python -m pip install uv` behaves different from `pip install uv` in installing a `uv` executable with `pyenv`, see below:

```
(project_3.12) bers@hostname:/mnt/c/ws/project$ which python
/home/bers/.pyenv/shims/python

(project_3.12) bers@hostname:/mnt/c/ws/project$ python --version
Python 3.12.1

(project_3.12) bers@hostname:/mnt/c/ws/project$ uv
-bash: /home/bers/.pyenv/shims/uv: No such file or directory

(project_3.12) bers@hostname:/mnt/c/ws/project$ python -m uv
/home/bers/.pyenv/versions/project_3.12/bin/python3: No module named uv

(project_3.12) bers@hostname:/mnt/c/ws/project$ python -m pip install uv
Collecting uv
  Using cached uv-0.1.21-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (25 kB)
Using cached uv-0.1.21-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (11.1 MB)
Installing collected packages: uv
Successfully installed uv-0.1.21

(project_3.12) bers@hostname:/mnt/c/ws/project$ uv
-bash: /home/bers/.pyenv/shims/uv: No such file or directory

(project_3.12) bers@hostname:/mnt/c/ws/project$ python -m uv
Usage: uv [OPTIONS] <COMMAND>

Commands:
  pip      Resolve and install Python packages
  venv     Create a virtual environment
  cache    Manage the cache
  version  Display uv's version
  help     Print this message or the help of the given subcommand(s)

Options:
  -q, --quiet                  Do not print any output
  -v, --verbose...             Use verbose output
      --color <COLOR>          Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls             Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
  -n, --no-cache               Avoid reading from or writing to the cache [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]
  -h, --help                   Print help (see more with '--help')
  -V, --version                Print version

(project_3.12) bers@hostname:/mnt/c/ws/project$ pip install uv
Requirement already satisfied: uv in /home/bers/.pyenv/versions/project_3.12/lib/python3.12/site-packages (0.1.21)

(project_3.12) bers@hostname:/mnt/c/ws/project$ uv
Usage: uv [OPTIONS] <COMMAND>

Commands:
  pip      Resolve and install Python packages
  venv     Create a virtual environment
  cache    Manage the cache
  version  Display uv's version
  help     Print this message or the help of the given subcommand(s)

Options:
  -q, --quiet                  Do not print any output
  -v, --verbose...             Use verbose output
      --color <COLOR>          Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls             Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
  -n, --no-cache               Avoid reading from or writing to the cache [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]
  -h, --help                   Print help (see more with '--help')
  -V, --version                Print version
```

---

_Comment by @charliermarsh on 2024-03-18 03:16_

Is it possible that this worked prior to v0.1.19? If so, it might be fixed by https://github.com/astral-sh/uv/pull/2503.

---

_Comment by @bersbersbers on 2024-03-18 08:30_

> Is it possible that this worked prior to v0.1.19?

Doesn't look like it:
```
(project_3.12) bers@hostname:/mnt/c/ws/project$ uv --version
-bash: uv: command not found

(project_3.12) bers@hostname:/mnt/c/ws/project$ python -m pip install uv==0.1.19
Collecting uv==0.1.19
  Downloading uv-0.1.19-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (25 kB)
Downloading uv-0.1.19-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (11.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 11.0/11.0 MB 9.6 MB/s eta 0:00:00
Installing collected packages: uv
Successfully installed uv-0.1.19

(project_3.12) bers@hostname:/mnt/c/ws/project$ uv --version
-bash: uv: command not found

(project_3.12) bers@hostname:/mnt/c/ws/project$ python -m pip install uv==0.1.18
Collecting uv==0.1.18
  Downloading uv-0.1.18-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (23 kB)
Downloading uv-0.1.18-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (11.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 11.0/11.0 MB 13.5 MB/s eta 0:00:00
Installing collected packages: uv
  Attempting uninstall: uv
    Found existing installation: uv 0.1.19
    Uninstalling uv-0.1.19:
      Successfully uninstalled uv-0.1.19
Successfully installed uv-0.1.18

(project_3.12) bers@hostname:/mnt/c/ws/project$ uv --version
-bash: uv: command not found

(project_3.12) bers@hostname:/mnt/c/ws/project$ python -m pip install uv==0.1.1
Collecting uv==0.1.1
  Downloading uv-0.1.1-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (16 kB)
Downloading uv-0.1.1-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (10.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 10.1/10.1 MB 5.2 MB/s eta 0:00:00
Installing collected packages: uv
  Attempting uninstall: uv
    Found existing installation: uv 0.1.18
    Uninstalling uv-0.1.18:
      Successfully uninstalled uv-0.1.18
Successfully installed uv-0.1.1

(project_3.12) bers@hostname:/mnt/c/ws/project$ uv --version
-bash: uv: command not found

(project_3.12) bers@hostname:/mnt/c/ws/project$ pip install uv==0.1.1
Requirement already satisfied: uv==0.1.1 in /home/bers/.pyenv/versions/project_3.12/lib/python3.12/site-packages (0.1.1)
```

> If so, it might be fixed by #2503.

I found the binaries in https://github.com/astral-sh/uv/actions/runs/8320517237, but how can I try with pip installation using those? :)

---

_Comment by @charliermarsh on 2024-05-22 13:33_

Do you mind re-resting?

---

_Comment by @bersbersbers on 2024-05-24 09:22_

Thanks, will retest in early June.

---

_Comment by @bersbersbers on 2024-06-01 06:50_

I can still reproduce the problem (Python 3.12.3, uv 0.2.5).

---

_Comment by @charliermarsh on 2024-07-28 00:50_

I think you need to activate the virtual environment here?

---

_Comment by @bersbersbers on 2024-07-28 06:54_

Unlikely - I use `PIP_REQUIRE_VIRTUALENV` everywhere. But I can re-test again in two weeks.

---

_Comment by @bersbersbers on 2024-08-10 18:42_

I can still reproduce this with `pip==24.2` and `uv==0.2.35`. Virtual env is activated (in fact, `PIP_REQUIRE_VIRTUALENV` is set).

Both

```
python -m pip uninstall -y pip && python -m ensurepip && python -m pip install -U pip==24.2 && pip uninstall -y uv && python -m pip install uv && uv
```

and

```
python -m pip uninstall -y pip && python -m ensurepip && python -m pip install -U pip==24.2 && python -m pip uninstall -y uv && python -m pip install uv && uv
```

sometimes fail like this:
```
...
-bash: /home/bers/.pyenv/shims/uv: No such file or directory
```

Interestingly, it always works this way:
```
python -m pip uninstall -y pip && python -m ensurepip && python -m pip install -U pip==24.2 && python -m pip uninstall -y uv && pip install uv && uv 
```

and once it works this way, the above two commands also work. So there must be some additional state that causes breakage.

---

_Comment by @charliermarsh on 2024-08-10 18:50_

Hmm ok, I ran the first command without issue just now. I'm guessing it's something to do with the pyenv interaction...?

---

_Comment by @bersbersbers on 2024-08-11 07:28_

May well be, yes. Unfortunately, I do not use system python anywhere and would like to avoid polluting my system with all the dependencies that come with it, so I cannot compare. But I was able to come up with very simple reproduction steps on another, clean WSL instance (in terms of Python):

```
curl https://pyenv.run | bash
# manipulate ~/.profile as instructed by the above command and restart shell

pyenv install 3
pyenv global 3
python -m pip install uv
uv --version # does not work

python -m pip uninstall -y uv
pip install uv
uv --version # works
```

So whatever state makes the commands above sometimes work and sometimes not, installing `pyenv` from scratch is close to the "broken" state.

---

_Comment by @zanieb on 2024-08-11 15:11_

Is this issue specific to uv? The reproduction you just shared seems entirely unrelated to our behaviors — i.e. it's pip and pyenv's job to properly install uv.

---

_Comment by @bersbersbers on 2024-08-11 15:25_

> The reproduction you just shared seems entirely unrelated to our behaviors — i.e. it's pip and pyenv's job to properly install uv.

Agreed.

> Is this issue specific to uv? 

I thought so, and I remember that I tested `black` and `pylint` before, and saw the same thing. But maybe I was falling for the randomness of the issue, and it was simply working for `black` and `pyint` because they had been installed and brought into the "working" state before.

Long story short: with the above reproduction steps, in a clean environment, I can reproduce the same behavior with `black` and `pylint` now. Thanks for following up!

---

_Comment by @bersbersbers on 2024-08-11 15:38_

Follow https://github.com/pypa/pip/issues/12909 if interested.

---

_Closed by @bersbersbers on 2024-08-11 15:38_

---
