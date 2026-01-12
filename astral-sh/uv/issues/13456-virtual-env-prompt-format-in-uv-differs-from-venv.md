```yaml
number: 13456
title: VIRTUAL_ENV_PROMPT format in uv differs from venv or virtualenv
type: issue
state: closed
author: ericbn
labels:
  - bug
assignees: []
created_at: 2025-05-14T16:31:32Z
updated_at: 2025-06-06T10:17:23Z
url: https://github.com/astral-sh/uv/issues/13456
synced_at: 2026-01-12T16:01:29Z
```

# VIRTUAL_ENV_PROMPT format in uv differs from venv or virtualenv

---

_@ericbn_

### Summary

With venv:
```
$ python3 -m venv .venv --prompt base
$ source .venv/bin/activate
$ echo "'$VIRTUAL_ENV_PROMPT'"
'base'
$ deactivate
```

With virtualenv:
```
$ uvx virtualenv .venv --prompt base
$ source .venv/bin/activate
$ echo "'$VIRTUAL_ENV_PROMPT'"
'base'
$ deactivate
```

With uv:
```
$ uv venv --prompt base
$ source .venv/bin/activate
$ echo "'$VIRTUAL_ENV_PROMPT'"
'(base) '
$ deactivate
```

uv adds parenthesis and a trailing space to `VIRTUAL_ENV_PROMPT`, while venv and virtualenv do not. Also, without `--prompt base`, all 3 cases above differ in the following way: venv and virtualenv yield `'.venv'` and uv yields `'(pwd) '` where `pwd` is the current directory name.

Just calling out the differences to make sure which ones are intentional. I'd argue adding the parenthesis and the trailing space to `VIRTUAL_ENV_PROMPT` might be differing too much from the other tools and confusing for scripts that want to use the value of this environment value. A typical case is setting `VIRTUAL_ENV_DISABLE_PROMPT=1` and then using `$VIRTUAL_ENV_PROMPT` when rendering a custom shell prompt.

Also, I see the 3 tools above set the `prompt` in `.venv/pyvenv.cfg` the same way (no parenthesis and no trailing space), so maybe another argument in favor of keeping `VIRTUAL_ENV_PROMPT` also the same.


### Platform

macOS 15 arm64

### Version

uv 0.7.3 (Homebrew 2025-05-07)

### Python version

Python 3.12.9

---

_Label `bug` added by @ericbn on 2025-05-14 16:31_

---

_Comment by @ericbn on 2025-05-17 02:26_

This issue was introduced in 0ec2d4e434878c6f99f7fea77b5236292a35093a to try to fix #8006. Since all tools are consistently producing `VIRTUAL_ENV_PROMPT` values without parenthesis and a trailing space, that issue is arguably an issue with VSCode and not with uv. Also, that commit only updated the bash/POSIX script and not the others, making everything even more inconsistent.

---

_Closed by @charliermarsh on 2025-05-17 11:48_

---

_Closed by @charliermarsh on 2025-05-17 11:48_

---

_Comment by @muzimuzhi on 2025-06-06 10:11_

> Also, without `--prompt base`, all 3 cases above differ in the following way: venv and virtualenv yield `'.venv'` and uv yields `'(pwd) '` where `pwd` is the current directory name.

This part of the problem persists in uv 0.7.11.

`uv venv` without `--prompt` still sets `VIRTUAL_ENV_PROMPT` to the _current directory name_, though now without parenthesis and the trailing space.

Preparation
```
$ mkdir my-proj
$ cd my-proj
```

With venv:

```
$ python3 -m venv .venv
$ source .venv/bin/activate
$ echo "'$VIRTUAL_ENV_PROMPT'"
'.venv'
$ deactivate
```

With virtualenv:
```
$ uvx virtualenv .venv
$ source .venv/bin/activate
$ echo "'$VIRTUAL_ENV_PROMPT'"
'.venv'
$ deactivate
```

With uv:
```
$ uv venv
$ source .venv/bin/activate
$ echo "'$VIRTUAL_ENV_PROMPT'"
'my-proj'
$ deactivate
```

---
