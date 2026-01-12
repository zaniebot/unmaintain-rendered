```yaml
number: 16686
title: Add iOS support for Python discovery
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/ios-support
created_at: 2025-11-11T11:59:15Z
updated_at: 2025-11-12T11:03:29Z
url: https://github.com/astral-sh/uv/pull/16686
synced_at: 2026-01-12T16:12:23Z
```

# Add iOS support for Python discovery

---

_@konstin_

iOS support exist nominally (https://github.com/astral-sh/uv/pull/15640), but Python discovery currently fails.

---

_Label `enhancement` added by @konstin on 2025-11-11 11:59_

---

_Comment by @henryiii on 2025-11-11 15:51_

```
Using CPython 3.13.9 interpreter at: /Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
Creating virtual environment at: /private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-y837m8qq/cp313-ios_arm64_iphoneos/build/venv
Activate with: source /private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-y837m8qq/cp313-ios_arm64_iphoneos/build/venv/bin/activate
error: Failed to inspect Python interpreter from active virtual environment at `/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-y837m8qq/cp313-ios_arm64_iphoneos/build/venv/bin/python3`
  Caused by: Querying Python at `/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-y837m8qq/cp313-ios_arm64_iphoneos/build/venv/bin/python3` failed with exit status exit status: 1
[stderr]
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import sys; sys.path = ["/Users/runner/.cache/uv/.tmpjHV1nh"] + sys.path; from python.get_interpreter_info import main; main()
                                                                                                                            ~~~~^^
  File "/Users/runner/.cache/uv/.tmpjHV1nh/python/get_interpreter_info.py", line 581, in main
    os_and_arch = get_operating_system_and_architecture()
  File "/Users/runner/.cache/uv/.tmpjHV1nh/python/get_interpreter_info.py", line 516, in get_operating_system_and_architecture
    version = platform.ios_ver().version.split(".")
              ^^^^^^^^^^^^^^^^^^^^^^^^^^
AttributeError: 'IOSVersionInfo' object has no attribute 'version'
```

(From https://github.com/pypa/cibuildwheel/pull/2586)

See https://github.com/python/cpython/blob/d890aba748e5213585f9f906888999227dc3fa9c/Lib/platform.py#L517-L521

---

_Comment by @konstin on 2025-11-11 17:02_

Could you try again with the fix?

---

_@zanieb approved on 2025-11-11 17:14_

---

_@zanieb reviewed on 2025-11-11 17:14_

---

_Review comment by @zanieb on `crates/uv-python/python/get_interpreter_info.py`:668 on 2025-11-11 17:14_

This looks like a formatter version mismatch

---

_Comment by @henryiii on 2025-11-11 23:46_

Makes it past this part, I think!

```
----------------------------- Captured stderr call -----------------------------
Using CPython 3.13.9 interpreter at: /Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
Creating virtual environment at: /private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv
Activate with: source /private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/bin/activate
error: Failed to inspect Python interpreter from active virtual environment at `/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/bin/python3`
  Caused by: Querying Python at `/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/bin/python3` returned an invalid response: missing field `simulator`

[stdout]
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.13.9", "os_name": "posix", "platform_machine": "arm64", "platform_python_implementation": "CPython", "platform_release": "13.0", "platform_system": "iOS", "platform_version": "", "python_full_version": "3.13.9", "python_version": "3.13", "sys_platform": "ios"}, "sys_base_prefix": "/Users/runner/Library/Caches/cibuildwheel/Python-3.13-iOS-support.b12/Python.xcframework/ios-arm64", "sys_base_exec_prefix": "/Users/runner/Library/Caches/cibuildwheel/Python-3.13-iOS-support.b12/Python.xcframework/ios-arm64", "sys_prefix": "/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv", "sys_base_executable": "/Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13", "sys_executable": "/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/bin/python3", "sys_path": ["/Library/Frameworks/Python.framework/Versions/3.13/lib/python313.zip", "/Library/Frameworks/Python.framework/Versions/3.13/lib/python3.13", "/Library/Frameworks/Python.framework/Versions/3.13/lib/python3.13/lib-dynload", "/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/lib/python3.13/site-packages"], "site_packages": ["/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/lib/python3.13/site-packages"], "stdlib": "/Users/runner/Library/Caches/cibuildwheel/Python-3.13-iOS-support.b12/Python.xcframework/ios-arm64/lib/python3.13", "standalone": false, "scheme": {"platlib": "/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/lib/python3.13/site-packages", "purelib": "/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/lib/python3.13/site-packages", "include": "/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/include/site/python3.13", "scripts": "/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv/bin", "data": "/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-t2e7gdvz/cp313-ios_arm64_iphoneos/build/venv"}, "virtualenv": {"purelib": "lib/python3.13/site-packages", "platlib": "lib/python3.13/site-packages", "include": "include/site/python3.13", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "ios", "major": 13, "minor": 0}, "arch": "iphoneos"}, "manylinux_compatible": false, "gil_disabled": false, "debug_enabled": false, "pointer_size": "64"}
```

---

_Merged by @konstin on 2025-11-12 11:03_

---

_Closed by @konstin on 2025-11-12 11:03_

---

_Branch deleted on 2025-11-12 11:03_

---
