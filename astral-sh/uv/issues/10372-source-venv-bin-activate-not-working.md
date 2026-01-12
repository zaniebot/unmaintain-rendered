```yaml
number: 10372
title: source .venv/bin/activate not working
type: issue
state: open
author: Trid-collab
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-01-07T17:19:27Z
updated_at: 2025-10-11T12:53:19Z
url: https://github.com/astral-sh/uv/issues/10372
synced_at: 2026-01-12T16:00:12Z
```

# source .venv/bin/activate not working

---

_@Trid-collab_

Need help. I am on macOS 15.2 and moving from pyenv trying to use uv. I have installed uv through home-brew and initialize my python directory  on my zsh shell using uv init and then created a virtual environment using uv venv which creates the necessary folder .venv. But the python environment in the directory doesn't seem to get activated when I try `source  .venv/bin/activate` I am not sure what am I missing here. Is there any additional config which is needed to the zsh shell or other place. I have installed python using `uv python install --preview`

---

_Comment by @charliermarsh on 2025-01-07 17:21_

How are you concluding that the environment is not activated? What commands are you running that are failing, and what do you expect them to do instead?

---

_Label `question` added by @charliermarsh on 2025-01-07 17:21_

---

_Comment by @zanieb on 2025-01-07 17:23_

Please also see #9452 on how to report issues.

---

_Comment by @Trid-collab on 2025-01-07 17:27_

I have added say a new package pandas using. `uv add pandas` in the directory. after running `source .venv/bin/activate`. Then when I open a python file using neovim and try to import pandas it doesn't seem to resolve. This wasn't the case when using pyenv

---

_Comment by @zanieb on 2025-01-07 17:33_

How do you know neovim is using your active environment?

---

_Comment by @zanieb on 2025-01-07 17:34_

Here's an example reproduction

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/example`
❯ cd example
❯ uv add pandas
warning: Ignoring existing virtual environment linked to non-existent Python interpreter: .venv/bin/python3 -> python
Using CPython 3.11.11 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 7 packages in 198ms
Prepared 6 packages in 1.17s
Installed 6 packages in 35ms
 + numpy==2.2.1
 + pandas==2.2.3
 + python-dateutil==2.9.0.post0
 + pytz==2024.2
 + six==1.17.0
 + tzdata==2024.2
❯ source .venv/bin/activate
❯ python -c "import pandas; print(pandas)"
<module 'pandas' from '/Users/zb/workspace/example/.venv/lib/python3.11/site-packages/pandas/__init__.py'>
```

---

_Label `needs-mre` added by @zanieb on 2025-01-07 17:34_

---

_Comment by @Trid-collab on 2025-01-07 17:52_

This is the output of the commands 
```tridi/master  uv add pandas pyarrow matplotlib python-calamine
Resolved 47 packages in 418ms
Prepared 13 packages in 3.34s
Installed 13 packages in 73ms
 + contourpy==1.3.1
 + cycler==0.12.1
 + fonttools==4.55.3
 + kiwisolver==1.4.8
 + matplotlib==3.10.0
 + numpy==2.2.1
 + pandas==2.2.3
 + pillow==11.1.0
 + pyarrow==18.1.0
 + pyparsing==3.2.1
 + python-calamine==0.3.1
 + pytz==2024.2
 + tzdata==2024.2
 trid/master  source .venv/bin/activate
trid/master python -c "import pandas; print(pandas)"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import pandas; print(pandas)
    ^^^^^^^^^^^^^
ModuleNotFoundError: No module named 'pandas'
zsh: exit 1     python -c "import pandas; print(pandas)"
```

---

_Comment by @zanieb on 2025-01-07 18:04_

Thanks! Can you share the output of `which python` and `echo $VIRTUAL_ENV`?

For multiline code blocks, use triple backticks ```

---

_Comment by @Trid-collab on 2025-01-07 18:29_

```which python
/Users/trid/.pyenv/shims/python
```
it seems that it isn't detecting the python 3.13.1 which was installed using `uv python install --preview`
I have also run `uv tool update-shell` which is also working 

```
echo $VIRTUAL_ENV

tridi > echo $VIRTUAL_ENV ?
zsh: no matches found: ?
```
```
 uv python find
/Users/trid/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13
```

---

_Comment by @charliermarsh on 2025-01-07 18:34_

Your pyenv shim seems to be getting in the way. Is this like https://github.com/astral-sh/uv/issues/6204?

---

_Comment by @zanieb on 2025-01-07 18:43_

Yeah you might need to remove your pyenv shim, I presume it ensures it's at the front of the `PATH`.

---

_Comment by @Trid-collab on 2025-01-09 15:32_

Thanks, when I comment out my pyenv lines in my zshrc it works. But it's not an ideal solution because as of now I need pyenv for certain use cases. Also the `.python-version` file in case of the pyenv enables automatic switching to the virtual environment which is put up in there, this option doesn't seem to work with uv even though it creates a `.pyenv-version` file. Is there something I need to do enable this feature.

---

_Comment by @Trid-collab on 2025-01-12 13:50_

Another observation. In fish shell if I use `source .venv/bin/activate.fish` the environment gets activated inspite of pyenv details still being active on the fish shell config file

---

_Comment by @ArmanBehkish on 2025-10-11 12:53_

I encountered a similar problem and here is how I fixed it:
OS: mac
shell: zsh
goal: update python version in the current uv project
```
echo "3.14.0" > .python-version
uv sync
```
after this, the dependencies and python version is correct, but the virtual environment cannot be activated with source.
The reason should be the conflict .python-version caused with pyenv.

solution: disable py-env hooks in the shell environment by commenting out following line from ~/.zshrc
```
eval "$(pyenv virtualenv-init -)"
```

---
