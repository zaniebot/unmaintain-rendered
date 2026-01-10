```yaml
number: 2707
title: "fixed uv can't create .venv for cpython-x86 on Windows "
type: pull_request
state: merged
author: Zander-1024
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2024-03-28T07:28:01Z
updated_at: 2024-04-03T01:45:54Z
url: https://github.com/astral-sh/uv/pull/2707
synced_at: 2026-01-10T14:49:08Z
```

# fixed uv can't create .venv for cpython-x86 on Windows 

---

_Pull request opened by @Zander-1024 on 2024-03-28 07:28_

Adaptation to the win32 platform is added.

https://docs.python.org/3/library/sysconfig.html#sysconfig.get_platform


## Summary

fixed uv can't create .venv for cpython-x86 on Windows 

[uv can't create .venv for cpython-x86 on Windows ](https://github.com/astral-sh/rye/issues/952)



---

_Comment by @bluss on 2024-03-28 09:03_

Related to astral-sh/rye/issues/952

---

_Review comment by @konstin on `crates/uv-interpreter/python/get_interpreter_info.py`:423 on 2024-03-28 09:16_

We use `"result": "error"` and handle the error in rust code instead of raising exceptions

---

_Review comment by @konstin on `crates/uv-interpreter/python/get_interpreter_info.py`:422 on 2024-03-28 09:16_

Do you have some context on why 32-bit windows is the only platform where that happens, and can you add a comment above the line explaining the special case?

---

_@konstin reviewed on 2024-03-28 09:16_

---

_@Zander-1024 reviewed on 2024-03-28 09:35_

---

_Review comment by @Zander-1024 on `crates/uv-interpreter/python/get_interpreter_info.py`:423 on 2024-03-28 09:35_

I know, but this is python code, and this function is just to get platform information. For unsupported platforms, you should also exit here. I've thought of two solutions. One is to just exit and add exception printing, and the other is to raise an exception. I think using an exception here might be appropriate.

---

_@Zander-1024 reviewed on 2024-03-28 09:40_

---

_Review comment by @Zander-1024 on `crates/uv-interpreter/python/get_interpreter_info.py`:422 on 2024-03-28 09:40_

https://docs.python.org/3/library/sysconfig.html#sysconfig.get_platform
According to the documentation, sysconfig.get_platform () returns "win32" when using the x86 version of Windows. This is a known issue. I'll add the documentation to the notes later.

---

_@konstin reviewed on 2024-03-28 09:50_

---

_Review comment by @konstin on `crates/uv-interpreter/python/get_interpreter_info.py`:423 on 2024-03-28 09:50_

If the python code raises an exception, the rust caller doesn't know what went wrong: Is the platform not supported? Is the version not supported? Is this `python` not a python? Is it a permission error? In rust we get an exit status, stdout and stderr, we can't even tell if we started an actual python interpreter.

Showing a python stacktrace is verbose and not helpful to the user. A user only needs one line (platform not supported) and does not need to see implementation details of our interpreter checking code. Instead, we return that the script ran successfully but the platform was not supported [here](https://github.com/astral-sh/uv/blob/c180fedbce60d8c58611d18cf351ef904fa2280d/crates/uv-interpreter/python/get_interpreter_info.py#L488-L497) and transform it into a rust error [here](https://github.com/astral-sh/uv/blob/7d285148b26550842ba7bb574fa7187faab183f9/crates/uv-interpreter/src/interpreter.rs#L346-L347). This way we get a clear, concise error message:

```
$ uv venv
  × Can't use Python at `/home/konsti/projects/uv/.venv/bin/python3`
  ╰─▶ Unknown operation system: `wedontknowthisos`
```

---

_@Zander-1024 reviewed on 2024-03-28 10:29_

---

_Review comment by @Zander-1024 on `crates/uv-interpreter/python/get_interpreter_info.py`:423 on 2024-03-28 10:29_

Ok, thanks for your answer

---

_Label `bug` added by @zanieb on 2024-03-28 14:23_

---

_Review requested from @konstin by @Zander-1024 on 2024-03-31 11:36_

---

_@T-256 approved on 2024-04-01 17:03_

Thanks, this just happened to me today. We could also have CI for Windows 32bit.

---

_Comment by @zanieb on 2024-04-01 21:37_

A "check system" CI job would make sense for that. 

---

_@konstin approved on 2024-04-02 09:17_

---

_Comment by @konstin on 2024-04-02 09:18_

@Zander-1024 Do you want to add a check system check or should we merge without?

---

_Comment by @Zander-1024 on 2024-04-02 15:54_

Sorry, I don't know where the problem with this ci build lies. Looking at the print, I should have installed pylint.

---

_Comment by @zanieb on 2024-04-02 16:35_

The system check tests require the Python you are testing with to be the default on the system e.g. `python3`. e.g. you're spawning with a `py -3.10` invocation but it needs to be `python3` and presumably there will be some futzing to make x86 Python the default?

---

_@zanieb approved on 2024-04-03 01:45_

---

_Comment by @zanieb on 2024-04-03 01:45_

Thanks!

---

_Merged by @zanieb on 2024-04-03 01:45_

---

_Closed by @zanieb on 2024-04-03 01:45_

---
