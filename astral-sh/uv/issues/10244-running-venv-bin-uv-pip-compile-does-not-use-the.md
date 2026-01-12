```yaml
number: 10244
title: "Running `venv/bin/uv pip compile` does not use the Python in`venv/bin` (if that venv was not activated)"
type: issue
state: closed
author: thecityofguanyu
labels:
  - question
assignees: []
created_at: 2024-12-30T21:14:35Z
updated_at: 2024-12-31T14:48:37Z
url: https://github.com/astral-sh/uv/issues/10244
synced_at: 2026-01-12T16:00:09Z
```

# Running `venv/bin/uv pip compile` does not use the Python in`venv/bin` (if that venv was not activated)

---

_@thecityofguanyu_

Given the following:

- An "unactivated" Python3.12 virtualenv, `venv/`, into which `uv` 0.5.11 was installed
- A system install of Python <3.12, which will be resolved from PATH
- A pip-compile `requirements.in` file that includes a package with `requires-python = ">=3.12"`

When the following command is run:

- `venv/bin/uv pip compile requirements.in  --verbose`

The command will fail because `uv` selects the <3.12 system Python instead of the one associated with the virtualenv into which `uv` itself was installed.

Note that Python docs seem to suggest this should be okay to do:
https://docs.python.org/3/library/venv.html
> You don’t specifically need to activate a virtual environment, as you can just specify the full path to that environment’s Python interpreter when invoking Python

Example output demonstrating:

```shell
~/ echo my-package > requirements.in
~/ python3.12 -m venv venv
~/venv/bin/pip --version
pip 24.3.1 from ./venv/lib/python3.12/site-packages/pip (python 3.12)
~/ venv/bin/pip install uv
Looking in indexes: redacted
Collecting uv
  Using cached https://redacted/uv-0.5.11-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (15.0 MB)
Installing collected packages: uv
Successfully installed uv-0.5.11
~/ venv/bin/uv pip compile requirements.in  --verbose
DEBUG uv 0.5.11
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/home/user/.local/share/uv/python`
DEBUG Found `cpython-3.11.11-linux-x86_64-gnu` at `/opt/bin/python3.11` (search path)
DEBUG Using Python 3.11.11 interpreter at /opt/bin/python3.11 for builds
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.11
DEBUG Solving with target Python version: >=3.11.11
DEBUG Adding direct dependency: my-package*
[redacted]
DEBUG Searching for a compatible version of my-package (*)
DEBUG No compatible version found for: Python
DEBUG Recording unit propagation conflict of Python from incompatibility of (my-package)
DEBUG Searching for a compatible version of my-package (<0.1.0 | >0.1.0)
DEBUG Searching for a compatible version of my-package (<0.0.1 | >0.0.1, <0.1.0 | >0.1.0)
DEBUG No compatible version found for: my-package
  × No solution found when resolving dependencies:
  ╰─▶ Because the current Python version (3.11.11) does not satisfy Python>=3.12 and all versions of my-package depend on Python>=3.12, we can conclude that all versions of my-package cannot be used.
      And because you require my-package, we can conclude that your requirements are unsatisfiable.
```

---

_Comment by @thecityofguanyu on 2024-12-30 21:27_

Caveat: Yes, simply activating the venv gets around this issue and is almost surely the best practice. Just wanted to open this issue anyway to see if this was known behavior.

---

_Comment by @thecityofguanyu on 2024-12-30 21:42_

Reading the docs, I see that perhaps this would have fixed the issue as well

>If uv is installed in a Python environment, e.g., with pip, it can still be used to modify other environments. However, when invoked with python -m uv, uv will default to using the parent interpreter's environment. Invoking uv via Python adds startup overhead and is not recommended for general usage.

https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments

---

_Comment by @thecityofguanyu on 2024-12-30 21:42_

Feel free to close this issue if there is no further comment on the topic given https://github.com/astral-sh/uv/issues/10244#issuecomment-2565943734

---

_Comment by @zanieb on 2024-12-30 21:48_

Yeah `venv/bin/python -m uv` is the recommended pattern if this is the behavior you want. We can't just sniff that we're installed in a virtual environment because that would presumably cause problems when uv is installed with tools like `pipx`.

---

_Label `question` added by @zanieb on 2024-12-30 21:48_

---

_Comment by @thecityofguanyu on 2024-12-30 22:02_

I am certainly not married to this behavior. 😄 

Ultimately, I think the main issues at play here not with the `uv` code itself, but rather:

- The wishy-washy language from Python's docs about around the virtualenv
- The inherited baggage from being a "drop-in replacement"

The language I'm referring to are the bits in the Python docs that say things like:

>A virtual environment **may** be “activated”  ...

And:

>You **don’t specifically need to** activate a virtual environment ...

While perhaps technically accurate for Python, I think that paints the wrong picture. Those statements are (I would imagine) almost never the right answer for actual usage.

---

_Comment by @thecityofguanyu on 2024-12-30 22:02_

Either way, thanks @zanieb for the confirmation! Will close this.

---

_Closed by @thecityofguanyu on 2024-12-30 22:02_

---

_Comment by @zanieb on 2024-12-30 22:05_

Are you referring to the upstream Python documentation or ours?

---

_Comment by @thecityofguanyu on 2024-12-31 14:48_

The upstream Python documentation, particularly on https://docs.python.org/3/library/venv.html

---
