---
number: 15421
title: "`uv sync` fails on `PyQt5` but pip install is OK"
type: issue
state: closed
author: tyctor
labels:
  - question
assignees: []
created_at: 2025-08-21T16:43:16Z
updated_at: 2025-08-21T18:54:46Z
url: https://github.com/astral-sh/uv/issues/15421
synced_at: 2026-01-10T01:25:56Z
---

# `uv sync` fails on `PyQt5` but pip install is OK

---

_Issue opened by @tyctor on 2025-08-21 16:43_

### Question

Hi 

I read that `uv` should be drop-in replacement for `pip` https://docs.astral.sh/uv/pip/compatibility/
So I have `PyQt5` dependency in my project, which `pip` install without problems, but `uv` is failing.

```
Resolved 198 packages in 4ms
  × Failed to build `pyqt5==5.15.2`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `sipbuild.api.build_wheel` failed (exit code: 1)

      [stderr]
      pyproject.toml: line 7: using '[tool.sip.metadata]' to specify the project metadata is deprecated and will be removed in SIP v7.0.0, use '[project]' instead
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "C:\Users\user5\AppData\Local\uv\cache\builds-v0\.tmpZhHtY4\Lib\site-packages\sipbuild\api.py", line 28, in build_wheel
          project = AbstractProject.bootstrap('wheel',
                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\user5\AppData\Local\uv\cache\builds-v0\.tmpZhHtY4\Lib\site-packages\sipbuild\abstract_project.py", line 74, in bootstrap
          project.setup(pyproject, tool, tool_description)
        File "C:\Users\user5\AppData\Local\uv\cache\builds-v0\.tmpZhHtY4\Lib\site-packages\sipbuild\project.py", line 633, in setup
          self.apply_user_defaults(tool)
        File "C:\Users\user5\AppData\Local\uv\cache\sdists-v9\index\5cada04d7ec01f7b\pyqt5\5.15.2\-MGGM6gVprDhwTvex3siK\src\project.py", line 63, in
      apply_user_defaults
          super().apply_user_defaults(tool)
        File "C:\Users\user5\AppData\Local\uv\cache\builds-v0\.tmpZhHtY4\Lib\site-packages\pyqtbuild\project.py", line 51, in apply_user_defaults
          super().apply_user_defaults(tool)
        File "C:\Users\user5\AppData\Local\uv\cache\builds-v0\.tmpZhHtY4\Lib\site-packages\sipbuild\project.py", line 243, in apply_user_defaults
          self.builder.apply_user_defaults(tool)
        File "C:\Users\user5\AppData\Local\uv\cache\builds-v0\.tmpZhHtY4\Lib\site-packages\pyqtbuild\builder.py", line 49, in apply_user_defaults
          raise PyProjectOptionException('qmake',
      sipbuild.pyproject.PyProjectOptionException
 ```

Is `uv` more "strict" as `pip` or why it cannot install project while `pip` can?


### Platform

Windows 11

### Version

uv 0.8.12

---

_Label `question` added by @tyctor on 2025-08-21 16:43_

---

_Comment by @konstin on 2025-08-21 17:29_

This is unfortunately a known problem with the PyQt5 package, which only publishes specific platforms in some versions (there's some previous discussion: https://github.com/astral-sh/uv/issues?q=is%3Aissue%20PyQt5, and see specifically https://github.com/astral-sh/uv/issues/11865 for the Windows case), and the workaround is to set [`tool.uv.required-environments`](https://docs.astral.sh/uv/concepts/resolution/#required-environments) to include your platform.

---

_Renamed from "uv sync fails but pip install is OK" to "`uv sync` fails on `PyQt5` but pip install is OK" by @zanieb on 2025-08-21 17:55_

---

_Comment by @tyctor on 2025-08-21 18:53_

thanks for answer

adding this to `pyproject.toml` resolved the error
```
[dependency-groups]
dev = [
    "pyqt5==5.15.11",
    "pyqt5-qt5==5.15.2",
]
```

---

_Closed by @tyctor on 2025-08-21 18:54_

---
