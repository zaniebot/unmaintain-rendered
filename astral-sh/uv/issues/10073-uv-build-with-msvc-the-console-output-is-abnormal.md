---
number: 10073
title: uv build with msvc, the console output is abnormal
type: issue
state: open
author: fanck0605
labels:
  - bug
  - windows
assignees: []
created_at: 2024-12-21T07:57:33Z
updated_at: 2024-12-21T23:52:54Z
url: https://github.com/astral-sh/uv/issues/10073
synced_at: 2026-01-10T01:24:49Z
---

# uv build with msvc, the console output is abnormal

---

_Issue opened by @fanck0605 on 2024-12-21 07:57_

## Environment
Platform: Windows 10 (**Chinese Language**)
Console charsets: **GBK**
UV: 0.5.11

## Details

I tried to build a `.pyd` file using scikit-build-core on Windows (msvc backend), when i use `uv build`, the output of msvc cannot print abnormal in prowershell console (like this `����ɴ��������`), while i directly using `python -m build` will not.

This may be caused by `uv build` not handling non-UTF-8 charsets well. Can anyone fix it?

## Console output

```
-- Build files have been written to: C:/Users/User/AppData/Local/Temp/tmpz_8ybcie/build
*** Building project with Visual Studio 16 2019...
���� .NET Framework �� Microsoft (R) ��������汾 16.11.2+f32259642
��Ȩ����(C) Microsoft Corporation����������Ȩ����

C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\MSBuild\Microsoft\VC\v160\Microsoft.CppBuild.targets(517,5): warning MSB8029: �м�Ŀ¼�����Ŀ¼�޷�פ������ʱĿ¼�£���Ϊ����ܻᵼ���������ɳ������⡣ [C:\Users\User\AppData\Local\Temp\tmpz_8ybcie\build\ZERO_CHECK.vcxproj]
  1>Checking Build System
C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\MSBuild\Microsoft\VC\v160\Microsoft.CppBuild.targets(517,5): warning MSB8029: �м�Ŀ¼�����Ŀ¼�޷�פ������ʱĿ¼�£���Ϊ����ܻᵼ���������ɳ������⡣ [C:\Users\User\AppData\Local\Temp\tmpz_8ybcie\build\_core.vcxproj]
  Building Custom Rule D:/.uv/cache/sdists-v6/.tmpJfNZWj/scikit_build_example-0.0.1/CMakeLists.txt
  main.cpp
    ���ڴ����� C:/Users/User/AppData/Local/Temp/tmpz_8ybcie/build/Release/_core.lib �Ͷ��� C:/Users/User/AppData/Local/Temp/tmpz_8ybcie/build/Release/_core.exp
  _core.vcxproj -> C:\Users\User\AppData\Local\Temp\tmpz_8ybcie\build\Release\_core.cp312-win_amd64.pyd
```

## Reproduce

1. Install  [Visual Stutio Build Tools](https://visualstudio.microsoft.com/zh-hans/downloads/?q=build+tools)

2. Run `uv build`

```
git clone https://github.com/pybind/scikit_build_example.git
cd scikit_build_example
uv build
```

---

_Comment by @anqorithm on 2024-12-21 09:09_

It works fine when using the uv command on WSL (Ubuntu on Windows) with the same environment. It seems that the issue is related to character encoding handling on Windows 10 with the GBK charset, which is causing problems with non-UTF-8 characters in the output.

Running the uv build command under WSL avoids this issue, as it uses a different console environment that handles the character encoding more effectively.

Let me know if you'd like further assistance!

![Image](https://github.com/user-attachments/assets/de73b68b-f676-4c0e-bf51-2aafc332f701)



---

_Label `bug` added by @charliermarsh on 2024-12-21 23:52_

---

_Label `windows` added by @charliermarsh on 2024-12-21 23:52_

---

_Comment by @charliermarsh on 2024-12-21 23:52_

Seems like a bug, thanks.

---
