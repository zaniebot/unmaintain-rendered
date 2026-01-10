```yaml
number: 7915
title: Failed to check python version compatibility of the package when installing or locking
type: issue
state: closed
author: vtannguyen
labels:
  - question
assignees: []
created_at: 2024-10-04T08:07:26Z
updated_at: 2024-10-06T14:19:49Z
url: https://github.com/astral-sh/uv/issues/7915
synced_at: 2026-01-10T04:45:10Z
```

# Failed to check python version compatibility of the package when installing or locking

---

_Issue opened by @vtannguyen on 2024-10-04 08:07_

UV seems to not check the python version compatibility when installing new package using `uv pip install` or `uv add` or locking package using `uv pip compile` or `uv lock`. The bug found in uv latest version `0.4.18`. The platform I'm using is fedora  version 40

To reproduce the bug, please take the following steps:

- Set up python env to version 3.8.13 using `pyenv`
```sh
pyenv local 3.8.13
```
- Set up venv and install uv
```sh
python -m venv .venv
source .venv/bin/activate
pip install uv
```
- Create a simple `pyproject.toml` file with the following content:
```
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.8,<3.9"
dependencies = [
]
```
- Try to add new package `typing`
```sh
uv add typing
```
- You should see the version `3.10.0.0` is installed
- try to uninstall the typing package using pip
```
pip uninstall -y typing
```
- try to install typing version 3.10.0.0 using pip
```
pip install typing==3.10.0.0
```
You should get the following error
```
ERROR: Could not find a version that satisfies the requirement typing==3.10.0.0 (from versions: 3.5.0b1, 3.5.0, 3.5.0.1, 3.5.1.0, 3.5.2.2, 3.5.3.0, 3.6.1, 3.6.2, 3.6.4, 3.6.6, 3.7.4, 3.7.4.1, 3.7.4.3)
ERROR: No matching distribution found for typing==3.10.0.0
```
When checking the html response at url https://pypi.org/simple/typing/, you can clearly see that for typing version 3.10.0.0, the required python version is `data-requires-python="&gt;=2.7, !=3.0.*, !=3.1.*, !=3.2.*, !=3.3.*, &lt;3.5"`, which requires-python = ">=3.8,<3.9" in `pyproject.toml` file does not satisfy.

---

_Comment by @charliermarsh on 2024-10-04 13:07_

We don't respect upper-bounds on `requires-python` for published packages. We treat the `requires-python` as the lower-bound on Python compatibility. There's extensive discussion around this in https://github.com/astral-sh/uv/issues/4022. You may also be interested in https://discuss.python.org/t/requires-python-upper-limits/12663.

---

_Closed by @charliermarsh on 2024-10-04 13:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-04 13:07_

---

_Label `question` added by @charliermarsh on 2024-10-04 13:07_

---

_Comment by @vtannguyen on 2024-10-04 14:12_

But `uv` managed to install an inappropriate version of `typing` while pip refused to do so (installing `typing` version 3.10.0.0 in python 3.8.13 environment). Could I call it a bug?

---

_Comment by @charliermarsh on 2024-10-04 14:29_

No, because from our perspective the package has `requires-python = ">=2.7"`, which _is_ compatible  We disregard the upper bound.

---

_Comment by @vtannguyen on 2024-10-04 14:41_

But then uv is incompatible with pip and pip-tools right? Because when i used “pip install .” It installed proper version of typing, which is 3.7.4.3 in python 3.8.13 environment. While uv insisted on 3.10.0.0. Also, when i used uv to compile requirements.txt file to install inside the docker container with pip, it just failed. If I compiled that file using pip-tools, everything works just fine. 

---

_Comment by @charliermarsh on 2024-10-04 21:19_

Yes, our handling of `requires-python` differs from pip's handling of `requires-python`. I'd recommend setting a constraint to avoid installing `typing==3.10.0.0` if you want to avoid that version. If you read the linked discussion from above (it's long, I know), there's more explanation on the motivation behind this behavior. (It's even possible that pip adopts those semantics in the future.)

---

_Comment by @vtannguyen on 2024-10-06 07:38_

I read the discussion above and can understand the reason behind the decision. But the main problem here is the pip compatibility. In the Readme file, it says that uv provide "A pip-compatible interface". That's not correct right? Because `uv pip install ...` might behave differently than `pip install ...` and `uv pip compile ...` might generate a requirements.txt file that is uninstallable by pip. If that is the case, could you please update the description in the Readme.md file, state clearly that `uv pip ...` is not fully compatible with pip so other developers understand it clearly?

---

_Comment by @charliermarsh on 2024-10-06 09:46_

uv is a drop-in replacement for common pip commands and operations (as stated in the README), but it’s not guaranteed that every command will behave in exactly the same way — we intentionally deviate when we think it’s correct or necessary to do so (and IMO it’s not really even possible to build a tool that upholds the contract that strictly).

You can read more here: https://docs.astral.sh/uv/pip/compatibility/.

(It’s definitely _not_ common for the requires-python upper-bound to play a role in resolution like this, which is why it almost never comes up in issues.)

If anything, I think it should be gated with a PEP 508 marker such that it’s not installed on Python 3.5 or later as per the package documentation.

---

_Comment by @vtannguyen on 2024-10-06 14:19_

Thanks for your explanation. Now I'm more clear on how uv works. 

One last question though, the whole purpose of the command `uv pip compile ...` is to generate a simple text file `requirements.txt`, which can then be installed anywhere (like inside docker container) with pip right? But now, the requirements.txt file is sometime not installable by pip. So every time I compile that file, I would need to check if I can use pip to install it first. If it's not the case, I need to check what is the problem, then try to fix it by restrict some packages versions. With other tool like pip-tools, it guarantees that the compiled requirements.txt file is always installable by pip. So in the end, the time trying to fix the problem might be much more than the time `uv` saves me because of its performance compare to the other tools.

I hope that the `uv` team can some how fix that problem and make it better for everyone.

Thanks a lot for creating a great tool with amazing performance.

---
