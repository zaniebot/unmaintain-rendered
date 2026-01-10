---
number: 3951
title: "`--system` flag no longer working as documented"
type: issue
state: closed
author: dpoznik
labels:
  - needs-decision
assignees: []
created_at: 2024-05-31T21:59:26Z
updated_at: 2024-06-10T16:19:42Z
url: https://github.com/astral-sh/uv/issues/3951
synced_at: 2026-01-10T01:23:32Z
---

# `--system` flag no longer working as documented

---

_Issue opened by @dpoznik on 2024-05-31 21:59_

I have been using `uv` in virtual environments managed by `pyenv virtualenv`. The `--system` flag worked great with `uv` 0.1.45 but does not work with 0.2.5, and the docs still suggest that it should still work:
> For convenience, `uv pip install --system` will install into the system Python environment, as an approximate shorthand for, e.g., `uv pip install --python=$(which python3)`. 

I understand that, currently, there are two work-arounds:
- `uv pip ... --python=$(which python)`
- `python -m uv ...`

But `--system` was more convenient, and it wasn't clear to me that it was intentional for it to have stopped working as previously.

Successful usage of `--system` flag with 0.1.45:
```sh
$ venv_name=uv-0.1.x
$ pyenv uninstall --force ${venv_name}
$ pyenv virtualenv ${venv_name}
$ pyenv shell ${venv_name}
$ pip install uv==0.1.45

[details omitted]

$ uv --version
uv 0.1.45 (a33a05e2d 2024-05-20)
$ pyenv version
uv-0.1.x (set by PYENV_VERSION environment variable)
$ python -c 'import sys; print(f"{(sys.prefix==sys.base_prefix)=}")'
(sys.prefix==sys.base_prefix)=False
$ uv pip install --system --upgrade pip setuptools wheel
Resolved 3 packages in 299ms
Installed 3 packages in 34ms
 - pip==22.3.1
 + pip==24.0
 - setuptools==65.5.0
 + setuptools==70.0.0
 + wheel==0.43.0
$ pyenv shell --unset
```

Unsuccessful usage of `--system` flag with 0.2.5:
```sh
$ venv_name=uv-0.2.x
$ pyenv uninstall --force ${venv_name}
$ pyenv virtualenv ${venv_name}
$ pyenv shell ${venv_name}
$ pip install uv

[details omitted]

$ uv --version
uv 0.2.5 (47db418ba 2024-05-28)
$ pyenv version
uv-0.2.x (set by PYENV_VERSION environment variable)
$ python -c 'import sys; print(f"{(sys.prefix==sys.base_prefix)=}")'
(sys.prefix==sys.base_prefix)=False
$ uv pip install --system --upgrade pip setuptools wheel --verbose
DEBUG Searching for Python interpreter in search path
DEBUG Found CPython 3.11.3 at `/Users/dpoznik/.pyenv/versions/uv-0.2.x/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/Users/dpoznik/.pyenv/versions/uv-0.2.x/bin/python3`: system interpreter required
DEBUG Found CPython 3.11.3 at `/Users/dpoznik/.pyenv/versions/uv-0.2.x/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/Users/dpoznik/.pyenv/versions/uv-0.2.x/bin/python`: system interpreter required
DEBUG Found CPython 3.11.3 at `/Users/dpoznik/.pyenv/shims/python3` (search path)
DEBUG Ignoring Python interpreter at `/Users/dpoznik/.pyenv/versions/uv-0.2.x/bin/python3`: system interpreter required
DEBUG Found CPython 3.11.3 at `/Users/dpoznik/.pyenv/shims/python` (search path)
DEBUG Ignoring Python interpreter at `/Users/dpoznik/.pyenv/versions/uv-0.2.x/bin/python`: system interpreter required
DEBUG Found CPython 3.12.3 at `/usr/local/bin/python3` (search path)
DEBUG Using Python 3.12.3 environment at /usr/local/opt/python@3.12/bin/python3.12
error: The interpreter at /usr/local/opt/python@3.12/Frameworks/Python.framework/Versions/3.12 is externally managed, and indicates the following:

[additional details omitted]

$ uv pip install --python=$(which python) --upgrade pip setuptools wheel
Resolved 3 packages in 562ms
Uninstalled 2 packages in 654ms
Installed 3 packages in 29ms
 - pip==22.3.1
 + pip==24.0
 - setuptools==65.5.0
 + setuptools==70.0.0
 + wheel==0.43.0
$ python -m uv pip install --upgrade pip setuptools wheel
Resolved 3 packages in 1.08s
Audited 3 packages in 0.22ms
$ pyenv shell --unset
```
Thanks for your help with this issue and for developing this wonderful tool!

---

_Comment by @charliermarsh on 2024-05-31 23:12_

Thanks for the clear issue. I suspect the issue here is that the Python you're using appears to be that of a virtual environment, but `--system` now ignores any virtualenv Pythons. I will defer to @zanieb though who is the expert here and is on vacation this week.

---

_Label `needs-decision` added by @charliermarsh on 2024-05-31 23:12_

---

_Comment by @dpoznik on 2024-05-31 23:36_

Thanks for the quick reply! Since it sounds like the new behavior is intended, one solution may be to:
- Clarify the `README.md` discussion of `--system` to reflect the recent behavior changes
- Add a new flag (e.g., `--current-python`) as shorthand for `--python=$(which python3)`

However, the underlying issue is that `uv` doesn't recognize a pyenv-virtualenv as a virtual environment:
```
$ uv pip install --upgrade pip setuptools wheel --verbose
DEBUG Searching for Python interpreter in virtual environments
error: No Python interpreters found in virtual environments
```
I had been using `--system` to work around this, but if `uv` were to recognize a pyenv-virtualenv as a virtual environment, then no flag would be necessary. So that may be the best solution, if possible. I'd be happy to create a new issue if so.

Thanks!


---

_Referenced in [astral-sh/uv#4009](../../astral-sh/uv/issues/4009.md) on 2024-06-04 02:36_

---

_Comment by @dpoznik on 2024-06-04 02:40_

> I had been using `--system` to work around this, but if `uv` were to recognize a pyenv-virtualenv as a virtual environment, then no flag would be necessary. So that may be the best solution, if possible. I'd be happy to create a new issue if so.

Since this issue may just reduce to a request to update the documentation around `--system`, I went ahead and created a new issue specific to the pyenv-virtualenv issue: #4009.

---

_Assigned to @zanieb by @zanieb on 2024-06-04 14:47_

---

_Referenced in [astral-sh/uv#4031](../../astral-sh/uv/pulls/4031.md) on 2024-06-04 23:54_

---

_Comment by @zanieb on 2024-06-04 23:56_

Does #4031 help?

---

_Referenced in [astral-sh/uv#4034](../../astral-sh/uv/pulls/4034.md) on 2024-06-05 01:16_

---

_Comment by @dpoznik on 2024-06-05 01:23_

> Does #4031 help?

Hmm, I'm not sure. I think the primary issue is in the previous paragraph, specifically the phrase:
>  as an approximate shorthand for, e.g., `uv pip install --python=$(which python3)`

In uv 0.1.x, I believe `--system` was more or less equivalent to `--python=$(which python3)`, whereas this is not the case in uv 0.2.x, rendering the phrase misleading. I get that it includes the fuzzing term "approximate", but it's not currently clear in that paragraph under which conditions the equivalence holds.

Since, AFAICT, `--python=$(which python3)` will install for the _current_ interpreter, and `--system` will do something different, I recommend removing the phrase and commenting separately on how to install for the current interpreter.

#4034 includes some suggestions. Feel free to use pieces of it or to ignore it completely.


---

_Closed by @zanieb on 2024-06-10 16:19_

---
