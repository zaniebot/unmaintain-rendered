---
number: 13807
title: realpath not available on macOS 12 (breaks venv activate and uv tool run)
type: issue
state: closed
author: kandji-trent
labels:
  - question
assignees: []
created_at: 2025-06-03T13:29:13Z
updated_at: 2025-06-12T15:16:25Z
url: https://github.com/astral-sh/uv/issues/13807
synced_at: 2026-01-07T13:12:18-06:00
---

# realpath not available on macOS 12 (breaks venv activate and uv tool run)

---

_Issue opened by @kandji-trent on 2025-06-03 13:29_

### Summary

The `realpath` binary is not available on macOS 12 and earlier. This results in error messages when attempting to activate a (relocatable) virtual environment or when running a tool without installing (`uvx` or `uv tool run`) as both of these use `realpath` in the script. 

The offending lines of code appear to be:
* `uv wheel`: https://github.com/astral-sh/uv/blob/98c708149afa854796672e1d49e7120d028af5ef/crates/uv-install-wheel/src/wheel.rs#L127
*  `uv-virtualenv`: https://github.com/astral-sh/uv/blob/98c708149afa854796672e1d49e7120d028af5ef/crates/uv-virtualenv/src/virtualenv.rs#L299

Here is an example running on macOS 12.7.6, ARM with `uv venv --relocatable`
```shell
anka@arm-12_7_5 ~ % uv venv --relocatable
Using CPython 3.13.3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
anka@arm-12_7_5 ~ % source .venv/bin/activate
.venv/bin/activate:81: command not found: realpath
(anka) anka@arm-12_7_5 ~ % which realpath 
realpath not found
```

Here is a example running on an macOS 12.7.6, ARM with `uvx`
```shell
anka@arm-12_7_5 ~ % uvx kst --version
/Users/anka/.cache/uv/archive-v0/-v1Dn8vBMUCf8TwtMb7Lg/bin/kst: line 2: realpath: command not found
/Users/anka/.cache/uv/archive-v0/-v1Dn8vBMUCf8TwtMb7Lg/bin/kst: line 2: /Users/anka/python: No such file or directory
/Users/anka/.cache/uv/archive-v0/-v1Dn8vBMUCf8TwtMb7Lg/bin/kst: line 2: exec: /Users/anka/python: cannot execute: No such file or directory
```

If the tool is first installed with `uv tool install`, then this does not happen. This make sense since the console script uses a different (simplified) shabang. 
```shell
anka@arm-12_7_5 ~ % uv tool install kst
Resolved 21 packages in 11ms
Installed 21 packages in 22ms
 + annotated-types==0.7.0
 + certifi==2025.4.26
 + charset-normalizer==3.4.2
 + click==8.1.8
 + idna==3.10
 + kst==1.0.2
 + markdown-it-py==3.0.0
 + mdurl==0.1.2
 + platformdirs==4.3.8
 + pydantic==2.11.5
 + pydantic-core==2.33.2
 + pygments==2.19.1
 + requests==2.32.3
 + rich==14.0.0
 + ruamel-yaml==0.18.12
 + ruamel-yaml-clib==0.2.12
 + shellingham==1.5.4
 + typer==0.16.0
 + typing-extensions==4.14.0
 + typing-inspection==0.4.1
 + urllib3==2.4.0
Installed 1 executable: kst
anka@arm-12_7_5 ~ % kst --version
kst, version 1.0.2
anka@arm-12_7_5 ~ % uvx kst --version
kst, version 1.0.2
```

I have tested this across both Intel and ARM versions of macOS and Python 3.11-13

### Platform

macOS 12 arm64, macOS 12 amd64

### Version

uv 0.7.9 (13a86a23b 2025-05-30)

### Python version

Python 3.13

---

_Label `bug` added by @kandji-trent on 2025-06-03 13:29_

---

_Comment by @zanieb on 2025-06-03 13:58_

I think this is intended at this time. Can you install `realpath`?

---

_Label `bug` removed by @zanieb on 2025-06-03 14:02_

---

_Label `question` added by @zanieb on 2025-06-03 14:02_

---

_Comment by @kandji-trent on 2025-06-03 16:50_

No issue installing it as a workaround for our purposes Just wanted to report the bug in case it was unknown. I wasn't able to find reference to supported macOS versions. Only a blanket macOS is supported statement (https://docs.astral.sh/uv/reference/policies/platforms/):

> uv has Tier 1 support for the following platforms:
>
> macOS (Apple Silicon)
> macOS (x86_64)
> Linux (x86_64)
> Windows (x86_64)

Feel free to close this out. And thank you for all the hard work on `uv`!

---

_Comment by @zanieb on 2025-06-03 17:02_

Thanks! We could add a note to the documentation.

---

_Referenced in [astral-sh/uv#13993](../../astral-sh/uv/pulls/13993.md) on 2025-06-12 13:36_

---

_Closed by @zanieb on 2025-06-12 15:16_

---

_Referenced in [astral-sh/uv#14471](../../astral-sh/uv/issues/14471.md) on 2025-07-06 22:34_

---
