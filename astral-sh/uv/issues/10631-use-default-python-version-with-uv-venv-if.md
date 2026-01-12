```yaml
number: 10631
title: Use Default Python Version with uv venv (if pyproject.toml already exists)
type: issue
state: closed
author: sglbl
labels:
  - question
assignees: []
created_at: 2025-01-15T13:01:59Z
updated_at: 2025-09-11T14:40:50Z
url: https://github.com/astral-sh/uv/issues/10631
synced_at: 2026-01-12T16:00:17Z
```

# Use Default Python Version with uv venv (if pyproject.toml already exists)

---

_@sglbl_

I want to set a **global default python version** (like `asdf` does)?
Because now when I initialize `uv`, it initializes with my latest python version installed (if I haven't created .python-version file already). It's the same for virtual environment.

I have **Ubuntu 24.04 Gnome/Wayland** with `python3.12` preinstalled. And in every project I use `python3.10`. That's why I'm trying to set a **default** global python version without needing uninstall my Ubuntu Python. But `uv` always chooses the highest version.


@zanieb already told me about `cd ~ && uv python pin 3.10` but it only works for 1 directory down.

![Image](https://github.com/user-attachments/assets/9618c9c2-31bc-43b8-b19a-ffb2434c9bee)


---

_Label `question` added by @charliermarsh on 2025-01-15 16:37_

---

_Assigned to @zanieb by @charliermarsh on 2025-01-15 16:37_

---

_Comment by @zanieb on 2025-01-15 16:49_

We don't read `.python-version` files _above_ the project directory when you're in a project.

---

_Comment by @zanieb on 2025-01-15 16:49_

We'll need to add a _real_ default Python version option to support this.

---

_Comment by @sglbl on 2025-02-27 11:26_

> We don't read `.python-version` files _above_ the project directory when you're in a project.
Yes but in another issue's answer you suggested this to me, that's why I thought it's gonna work.

I would really like to have a real default python version option. But for now I'm able to use this as a workaround.

![Image](https://github.com/user-attachments/assets/1ef17a04-a492-4a50-bea4-876d36ca88d9)

I added `python3.10` of **uv** to the `$PATH`. And I can finally use this as my default version everywhere.


---

_Comment by @sglbl on 2025-03-25 17:29_

I saw that version `v0.6.9` has added an option to set a environment variable: `export UV_MANAGED_PYTHON=true`
This is really helpful for me and at least I can use a default python version.
But now another problem occurs;
```bash
sglbl@deducepc:~/deduce/dene$ locate python3 | grep ^/usr/bin
/usr/bin/pybabel-python3
/usr/bin/python3
/usr/bin/python3-config
/usr/bin/python3-coverage
/usr/bin/python3.12
/usr/bin/python3.12-config
/usr/bin/python3.12-coverage
/usr/bin/x86_64-linux-gnu-python3-config
/usr/bin/x86_64-linux-gnu-python3.12-config
sglbl@deducepc:~/deduce/dene$ uv python list --only-installed
cpython-3.10.12-linux-x86_64-gnu    /home/sglbl/.local/share/uv/python/cpython-3.10.12-linux-x86_64-gnu/bin/python3.10
sglbl@deducepc:~/deduce/dene$ uv venv --python 3.12
cpython-3.12.9-linux-x86_64-gnu ------------------------------ 1.23 MiB/20.31 MiB
^C
sglbl@deducepc:~/deduce/dene$ uv venv --no-managed-python
error: the argument '--no-managed-python' cannot be used with '--managed-python'

Usage: uv venv --no-managed-python [PATH]

For more information, try '--help'.
```

Now I cannot use my system python even with --python and even with --no-managed-python. `--python 3.12` tries to download that version but it's already my system version. Do I really have to unset one variable each time I want to use?

---

_Comment by @zanieb on 2025-03-25 18:51_

Huh, this works as I'd expect

```
❯ uv pip install anyio --no-managed-python --managed-python
Audited 1 package in 2ms
```

but this does not

```
❯ UV_NO_MANAGED_PYTHON=1 uv pip install anyio --managed-python
error: the argument '--managed-python' cannot be used with '--no-managed-python'

Usage: uv pip install --managed-python <PACKAGE|--requirements <REQUIREMENTS>|--editable <EDITABLE>|--group <GROUP>>

For more information, try '--help'.      
```

This may be a broader problem with our CLI parsing? They're intended to override each other cc @jtfmumm 

---

_Comment by @sglbl on 2025-09-11 14:40_

> We'll need to add a _real_ default Python version option to support this.

@zanieb I think https://github.com/astral-sh/uv/pull/12115 solves the issue. (Add support for --global default version in uv python pin). Thanks.

---

_Closed by @sglbl on 2025-09-11 14:40_

---
