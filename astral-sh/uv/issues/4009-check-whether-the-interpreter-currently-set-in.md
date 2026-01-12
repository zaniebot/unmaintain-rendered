```yaml
number: 4009
title: "Check whether the interpreter currently set in `PATH` is associated with a virtual environment"
type: issue
state: closed
author: dpoznik
labels:
  - compatibility
assignees: []
created_at: 2024-06-04T02:36:50Z
updated_at: 2024-11-09T00:09:01Z
url: https://github.com/astral-sh/uv/issues/4009
synced_at: 2026-01-12T15:58:47Z
```

# Check whether the interpreter currently set in `PATH` is associated with a virtual environment

---

_@dpoznik_

This issue is forked from #3951.

When I attempt to use uv in a pyenv-virtualenv, I receive an error message indicating that there are no Python interpreters found in virtual environments. Since `sys.prefix!=sys.base_prefix` within a pyenv-virtualenv, it would be nice to be able to use uv therein, without having to invoke  with `--python=$(which python)`.

```
$ venv_name=uv
$ pyenv uninstall --force ${venv_name}
$ pyenv virtualenv ${venv_name}
$ pyenv shell ${venv_name}
$ pip install uv

[details omitted]

$ uv --version
uv 0.2.6 (a589ad506 2024-06-03)
$ pyenv version
uv (set by PYENV_VERSION environment variable)
$ python -c 'import sys; print(f"{(sys.prefix==sys.base_prefix)=}")'
(sys.prefix==sys.base_prefix)=False
$ uv pip install --upgrade pip --verbose
DEBUG Searching for Python interpreter in virtual environments
error: No Python interpreters found in virtual environments
```


---

_Label `compatibility` added by @charliermarsh on 2024-06-04 03:17_

---

_Comment by @charliermarsh on 2024-06-04 03:17_

Thanks @dpoznik!

---

_Comment by @zanieb on 2024-06-04 13:20_

Thanks for the issue! We should definitely support this without requiring a `--system` flag workaround.

Do you know _why_ pyenv environments do not have `sys.prefix == sys.base_prefix`? Is there a `pyvenv.cfg` in the environment it creates?

---

_Comment by @dpoznik on 2024-06-04 17:18_

> We should definitely support this without requiring a `--system` flag workaround.

Sounds great; thanks!

> Do you know _why_ pyenv environments do not have `sys.prefix == sys.base_prefix`? Is there a `pyvenv.cfg` in the environment it creates?

Yes, [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) uses `python -m venv` on the back end:
```
$ cat $(pyenv root)/versions/uv/pyvenv.cfg
home = /Users/dpoznik/.pyenv/versions/3.11.3/bin
include-system-site-packages = false
version = 3.11.3
executable = /Users/dpoznik/.pyenv/versions/3.11.3/bin/python3.11
command = /Users/dpoznik/.pyenv/versions/3.11.3/bin/python3.11 -m venv /Users/dpoznik/.pyenv/versions/3.11.3/envs/uv
```

In case relevant, I have the following in my `.bashrc`:
```
eval "$(pyenv virtualenv-init -)"
```
That configures `pyenv-virtualenv` to "automatically activate/deactivate virtualenvs on entering/leaving directories which contain a `.python-version` file that contains the name of a valid virtual environment", as described [here](https://github.com/pyenv/pyenv-virtualenv?tab=readme-ov-file#activate-virtualenv). Aside: This is the main feature for which I'm still using `pyenv-virtualenv` rather than `uv venv`; perhaps automatic activation would make for a worthwhile feature request in a third Issue.

Thanks for your help!

---

_Comment by @zanieb on 2024-06-04 17:59_

Hm I'm confused that the virtual environment isn't PEP 405 compliant if it has `pyenv.cfg` created by `venv`.


---

_Comment by @dpoznik on 2024-06-04 18:37_

> I'm confused that the virtual environment isn't PEP 405 compliant if it has `pyenv.cfg` created by `venv`.

Interesting. I'm not all that familiar with PEP 405 or with how pyenv-virtualenv sets up environments.
What does compliance entail?

It's definitely possible I'm missing something, but I don't think #4018 will address this issue, as `VIRTUAL_ENV` is unset when the pyenv-virtualenv is activated:
```sh
$ pyenv version
uv (set by PYENV_VERSION environment variable)
$ env | grep VIRTUAL_ENV
$ 
```


---

_Comment by @zanieb on 2024-06-04 18:49_

Oh sorry I think I might have misunderstood the original issue! I'll start from the top so we can make sure we're on the same page.

[PEP 405](https://peps.python.org/pep-0405/) stipulates

> If a pyvenv.cfg file is found either adjacent to the Python executable or one directory above it (if the executable is a symlink, it is not dereferenced), this file is scanned for lines of the form key = value. If a home key is found, this signifies that the Python binary belongs to a virtual environment, and the value of the home key is the directory containing the Python executable used to create this virtual environment.
>
> In this case, prefix-finding continues as normal using the value of the home key as the effective Python binary location, which finds the prefix of the base installation. sys.base_prefix is set to this value, while sys.prefix is set to the directory containing pyvenv.cfg.

Thus, we say a PEP 405 compliant virtual environment must have _inequal_ `sys.prefix` and `sys.base_prefix` values e.g.

https://github.com/astral-sh/uv/blob/365ca637c77b19b3f4f1caafc58801aff45abfae/crates/uv-interpreter/src/interpreter.rs#L156-L162

(and in [`pip`](https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/utils/virtualenv.py#L14-L19))

As I understand re-reading your comments now, you _do_ have a compliant virtual environment in the current interpreter, e.g. `python -m ...` will be invoked inside a virtual environment. However, we do not detect this environment.

We only detect virtual environments in three ways:

- `.venv` directory in the file system hierarchy
- `VIRTUAL_ENV`
- `CONDA_PREFIX`

We don't currently support checking if the _current_ interpreter on the `PATH` is a virtual environment. I presume this is the case for you because the pyenv `python` shim invokes commands within the proper environment? We could explore adding support for this.


---

_Comment by @dpoznik on 2024-06-04 19:39_

> Oh sorry I think I might have misunderstood the original issue! I'll start from the top so we can make sure we're on the same page.

Cool, thanks for all the information. We are definitely on the same page now.

> As I understand re-reading your comments now, you _do_ have a compliant virtual environment in the current interpreter

Exactly:
```
$ python -c 'import sys; print(f"{(sys.prefix==sys.base_prefix)=}")'
(sys.prefix==sys.base_prefix)=False
```

> We only detect virtual environments in three ways:
> 
> * `.venv` directory in the file system hierarchy
> * `VIRTUAL_ENV`
> * `CONDA_PREFIX`

Got it. That explains it, as the pyenv-virtualenv has none of these properties. Thanks for this information!

> We don't currently support checking if the _current_ interpreter on the `PATH` is a virtual environment. I presume this is the case for you because the pyenv `python` shim invokes commands within the proper environment?

Exactly.

> We could explore adding support for this.

That would be great; thanks!

Now that I have a better sense of what's going on, I'll go ahead and rename the Issue.
Thanks again!


---

_Renamed from "Recognize a pyenv-virtualenv as a virtual environment" to "Check whether the *current* interpreter on the `PATH` is a virtual environment" by @dpoznik on 2024-06-04 19:40_

---

_Renamed from "Check whether the *current* interpreter on the `PATH` is a virtual environment" to "Check whether the current interpreter on the `PATH` is a virtual environment" by @dpoznik on 2024-06-04 19:40_

---

_Renamed from "Check whether the current interpreter on the `PATH` is a virtual environment" to "Check whether the interpreter currently set in `PATH` is associated with a virtual environment" by @dpoznik on 2024-06-11 00:38_

---

_Comment by @dpoznik on 2024-11-09 00:09_

I noticed that the problem reported in this issue has been resolved. Thanks!

Executing the same sequence of commands as is found in the description results in an expected/successful outcome. So I will go ahead and close the issue.
```sh
$ venv_name=uv
$ pyenv uninstall --force ${venv_name}
$ pyenv virtualenv ${venv_name}
$ pyenv shell ${venv_name}
$ pyv
uv (set by PYENV_VERSION environment variable)
$ pip install uv

[details omitted]

$ uv --version
uv 0.5.0 (8d665267c 2024-11-07)
$ pyenv version
uv (set by PYENV_VERSION environment variable)
$ python -c 'import sys; print(f"{(sys.prefix==sys.base_prefix)=}")'
(sys.prefix==sys.base_prefix)=False
$ uv pip install --upgrade pip --verbose
DEBUG uv 0.5.0 (8d665267c 2024-11-07)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.7-macos-aarch64-none` at `/Users/dpoznik/.pyenv/versions/uv/bin/python` (search path)
Using Python 3.12.7 environment at .pyenv/versions/uv

[details omitted]

```

A bisection indicates that version `0.2.14` fixed the issue:
```sh
[m|~]: uv pip install -U --python=$(which python) uv==0.2.13
Resolved 1 package in 577ms
Prepared 1 package in 10.23s
Uninstalled 1 package in 13ms
Installed 1 package in 7ms
 - uv==0.2.15
 + uv==0.2.13
[m|~]: uv pip install --upgrade pip
error: No Python interpreters found in virtual environments
[m|~]: uv pip install -U --python=$(which python) uv==0.2.14
Resolved 1 package in 3.69s
Downloaded 1 package in 9.28s
Uninstalled 1 package in 14ms
Installed 1 package in 7ms
 - uv==0.2.13
 + uv==0.2.14
[m|~]: uv pip install --upgrade pip
Resolved 1 package in 189ms
Audited 1 package in 0.59ms
```
But I don't see anything in the [release notes](https://github.com/astral-sh/uv/releases/tag/0.2.14) to this effect, so maybe the improvement was incidental? Just figured I'd mention in case doing so were to lessen the chance of a regression.


---

_Closed by @dpoznik on 2024-11-09 00:09_

---
