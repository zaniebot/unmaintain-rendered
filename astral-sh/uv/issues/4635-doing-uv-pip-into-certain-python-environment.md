```yaml
number: 4635
title: "Doing `uv pip` into certain python environment (xonsh shell case)"
type: issue
state: closed
author: anki-code
labels:
  - question
assignees: []
created_at: 2024-06-28T20:39:34Z
updated_at: 2025-07-20T05:26:48Z
url: https://github.com/astral-sh/uv/issues/4635
synced_at: 2026-01-12T15:58:51Z
```

# Doing `uv pip` into certain python environment (xonsh shell case)

---

_@anki-code_

Hello! Thank you for this fast and awesome thing!

The [xonsh shell](https://xon.sh/) is a python-powered shell and you need to run it from python. After run you have `xpip` command that refers to the python (pip) that used to run xonsh e.g.

```xsh
xonsh
sys.executable
# '/Users/pc/.local/xonsh-env/bin/python'  # https://github.com/anki-code/xonsh-install
which xpip
# /Users/pc/.local/xonsh-env/bin/python -m pip
```

I want to replace `xpip` to `xuv` and I see that this is working perfect:

```xsh
aliases['xuv-install'] = "uv pip install --python=@(sys.executable) @($args)"
xuv-install lolcat
# Installed 1 package # to /Users/pc/.local/xonsh-env/bin
```

Two questions:

1. Is it the right way to do this?

2. Is it possible to set python path before action? i.e.

    ```xsh
    aliases['xuv'] = "uv pip --python=@(sys.executable) @($args)"
    xuv install lolcat
    xuv uninstall lolcat
    ```
    

Thanks!

---

_Renamed from "Doing `uv pip install` into certain python environment (xonsh shell case)" to "Doing `uv pip` into certain python environment (xonsh shell case)" by @anki-code on 2024-06-28 20:42_

---

_Comment by @zanieb on 2024-06-28 22:22_

You can use the `UV_PYTHON` environment variable if you want it to be before the command. Setting the `--python` option to the path seems really reasonable!

---

_Label `question` added by @zanieb on 2024-06-28 22:22_

---

_Comment by @anki-code on 2024-06-28 23:03_

Thank you for the answer! This works awesome:
```xsh
aliases['xuv'] = '$UV_PYTHON=@(__xonsh__.imp.sys.executable) uv pip @($args)'
xuv install lolcat
# Installed 1 package
xuv uninstall lolcat
# Uninstalled 1 package
```

---

_Closed by @anki-code on 2024-06-28 23:03_

---

_Comment by @EricDepagne on 2024-08-14 21:57_

I know the issue is closed, but I' d like to thank you both, @zanieb and @anki-code, as i had the same issue and you helped me fix it  ~~(although I had to use `which python3` instead of `sys.executable` for it to work in a virtualenv managed by `vox`)~~  Obviously, if you import sys before, it works.

---

_Comment by @dysfungi on 2025-07-20 01:22_

For those coming here to reference the `$UV_PYTHON` alias solution in https://github.com/astral-sh/uv/issues/4635#issuecomment-2197754249 and running into issues, please see https://github.com/xonsh/xonsh/issues/5883 and https://github.com/xonsh/xonsh/issues/5631.

---
