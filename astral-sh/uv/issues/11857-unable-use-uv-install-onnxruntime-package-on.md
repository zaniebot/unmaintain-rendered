```yaml
number: 11857
title: unable use uv install  onnxruntime package on intel64 macos
type: issue
state: closed
author: jeevic
labels:
  - cache
assignees: []
created_at: 2025-02-28T14:44:11Z
updated_at: 2025-03-02T01:36:40Z
url: https://github.com/astral-sh/uv/issues/11857
synced_at: 2026-01-12T16:00:48Z
```

# unable use uv install  onnxruntime package on intel64 macos

---

_@jeevic_

### Summary

when use uv install onnxruntime==1.20.1  it not work but pip install can 
uv version: 0.6.3 

the error info：
`
uv pip install onnxruntime==1.20.1 

  × No solution found when resolving dependencies:
  ╰─▶ Because onnxruntime==1.20.1 has no wheels with a matching platform tag (e.g., `macosx_12_0_x86_64`) and you require
      onnxruntime==1.20.1, we can conclude that your requirements are unsatisfiable.
      hint: Wheels are available for `onnxruntime` (v1.20.1) on the following platforms: `manylinux_2_27_aarch64`, `manylinux_2_27_x86_64`,
      `manylinux_2_28_aarch64`, `manylinux_2_28_x86_64`, `macosx_13_0_universal2`, `win32`, `win_amd64`
`

pip install work well
`pip install onnxruntime==1.20.1   
Looking in indexes: https://mirrors.aliyun.com/pypi/simple/
Collecting onnxruntime==1.20.1
  Using cached https://mirrors.aliyun.com/pypi/packages/4e/28/99f903b0eb1cd6f3faa0e343217d9fb9f47b84bca98bd9859884631336ee/onnxruntime-1.20.1-cp310-cp310-macosx_13_0_universal2.whl (31.0 MB)
Requirement already satisfied: coloredlogs in /Users/jeevi/.pyenv/versions/3.10.14/envs/py3.10.14/lib/python3.10/site-packages (from onnxruntime==1.20.1) (15.0.1)
Requirement already satisfied: flatbuffers in /Users/jeevi/.pyenv/versions/3.10.14/envs/py3.10.14/lib/python3.10/site-packages (from onnxruntime==1.20.1) (25.2.10)
Requirement already satisfied: numpy>=1.21.6 in /Users/jeevi/.pyenv/versions/3.10.14/envs/py3.10.14/lib/python3.10/site-packages (from onnxruntime==1.20.1) (1.26.4)
Requirement already satisfied: packaging in /Users/jeevi/.pyenv/versions/3.10.14/envs/py3.10.14/lib/python3.10/site-packages (from onnxruntime==1.20.1) (24.2)
Requirement already satisfied: protobuf in /Users/jeevi/.pyenv/versions/3.10.14/envs/py3.10.14/lib/python3.10/site-packages (from onnxruntime==1.20.1) (5.29.3)
Requirement already satisfied: sympy in /Users/jeevi/.pyenv/versions/3.10.14/envs/py3.10.14/lib/python3.10/site-packages (from onnxruntime==1.20.1) (1.13.2)
Requirement already satisfied: humanfriendly>=9.1 in /Users/jeevi/.pyenv/versions/3.10.14/envs/py3.10.14/lib/python3.10/site-packages (from coloredlogs->onnxruntime==1.20.1) (10.0)
Requirement already satisfied: mpmath<1.4,>=1.1.0 in /Users/jeevi/.pyenv/versions/3.10.14/envs/py3.10.14/lib/python3.10/site-packages (from sympy->onnxruntime==1.20.1) (1.3.0)
Installing collected packages: onnxruntime
Successfully installed onnxruntime-1.20.1`


### Platform

macOS 13.7.4 intel x64

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

3.10.14

> 

---

_Label `bug` added by @jeevic on 2025-02-28 14:44_

---

_Renamed from "unable use uv install  onnxruntime package" to "unable use uv install  onnxruntime package on intel64 macos" by @jeevic on 2025-02-28 14:49_

---

_Comment by @charliermarsh on 2025-02-28 20:56_

Umm, it's really hard to say, it seems like uv "thinks" you're on macOS 12. Did you try clearing the cache? Did you upgrade from macOS 12 at some point?

---

_Label `bug` removed by @charliermarsh on 2025-03-01 00:30_

---

_Label `needs-mre` added by @charliermarsh on 2025-03-01 00:30_

---

_Comment by @jeevic on 2025-03-01 02:47_

Thank you for your answer。The problem bothered me for many days。 I've upgraded the macos system before(catalina ->ventura)， but not clean uv。 use command  `uv cache clean` 。 it can works well。

---

_Comment by @charliermarsh on 2025-03-01 02:58_

Very interesting. My guess is that we cached the information about your environment, and it's not invalidated when you upgrade.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-01 03:31_

---

_Comment by @charliermarsh on 2025-03-01 03:41_

I put up a change at https://github.com/astral-sh/uv/pull/11875 that should help us avoid this confusion in the future.

---

_Label `needs-mre` removed by @charliermarsh on 2025-03-01 03:41_

---

_Label `cache` added by @charliermarsh on 2025-03-01 03:41_

---

_Closed by @charliermarsh on 2025-03-02 01:36_

---

_Closed by @charliermarsh on 2025-03-02 01:36_

---
