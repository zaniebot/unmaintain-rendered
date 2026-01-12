```yaml
number: 12765
title: "'classifiers' should be a list, got type 'tuple'"
type: issue
state: closed
author: yjc980121
labels:
  - needs-mre
assignees: []
created_at: 2025-04-09T01:26:13Z
updated_at: 2025-07-11T02:23:03Z
url: https://github.com/astral-sh/uv/issues/12765
synced_at: 2026-01-12T16:01:12Z
```

# 'classifiers' should be a list, got type 'tuple'

---

_@yjc980121_

### Summary

 uv add nacos-sdk-python


  × Failed to build `alibabacloud-darabonba-array==0.1.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.get_requires_for_build_wheel` failed: failed to open file
      `C:\Users\xxx\AppData\Local\uv\cache\builds-v0\.tmp2jf7Ig\get_requires_for_build_wheel.txt`: 系统找不到指定的文件

      [stdout]
      running egg_info
      writing alibabacloud_darabonba_array.egg-info\PKG-INFO
      writing dependency_links to alibabacloud_darabonba_array.egg-info\dependency_links.txt
      writing top-level names to alibabacloud_darabonba_array.egg-info\top_level.txt
      reading manifest file 'alibabacloud_darabonba_array.egg-info\SOURCES.txt'
      writing manifest file 'alibabacloud_darabonba_array.egg-info\SOURCES.txt'

      [stderr]
      Warning: 'classifiers' should be a list, got type 'tuple'
      C:\Users\xxx\AppData\Local\uv\cache\builds-v0\.tmp2jf7Ig\Lib\site-packages\setuptools\dist.py:759: SetuptoolsDepted.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: Apache Software License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()

      hint: This usually indicates a problem with the package or the build environment.
  help: `alibabacloud-darabonba-array` (v0.1.0) was included because `nacos-sdk-python` (v2.0.3) depends on `alibabacloud-kms20
      [stdout]
      running egg_info
      writing alibabacloud_darabonba_array.egg-info\PKG-INFO
      writing dependency_links to alibabacloud_darabonba_array.egg-info\dependency_links.txt
      writing top-level names to alibabacloud_darabonba_array.egg-info\top_level.txt
      reading manifest file 'alibabacloud_darabonba_array.egg-info\SOURCES.txt'
      writing manifest file 'alibabacloud_darabonba_array.egg-info\SOURCES.txt'

      [stderr]
      Warning: 'classifiers' should be a list, got type 'tuple'
      C:\Users\xxx\AppData\Local\uv\cache\builds-v0\.tmpssdgZ7\Lib\site-packages\setuptools\dist.py:759: SetuptoolsDeprecationWarning: License classifiers are
      deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: Apache Software License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()

      hint: This usually indicates a problem with the package or the build environment.
  help: `alibabacloud-darabonba-array` (v0.1.0) was included because `dify2openai` (v0.1.0) depends on `alibabacloud-darabonba-array==0.1.0`

### Platform

windows 11 x86_64

### Version

uv 0.5.16 (333f03f11 2025-01-08)

### Python version

Python 3.12.8 | Python 3.10.16

---

_Label `bug` added by @yjc980121 on 2025-04-09 01:26_

---

_Comment by @charliermarsh on 2025-04-09 01:37_

This built without error for me:

```
❯ uv pip install alibabacloud-darabonba-array==0.1.0
Resolved 1 package in 772ms
      Built alibabacloud-darabonba-array==0.1.0
Prepared 1 package in 306ms
Installed 1 package in 0.69ms
 + alibabacloud-darabonba-array==0.1.0
```

---

_Comment by @yjc980121 on 2025-04-09 08:43_

> This built without error for me:
> 
> ```
> ❯ uv pip install alibabacloud-darabonba-array==0.1.0
> Resolved 1 package in 772ms
>       Built alibabacloud-darabonba-array==0.1.0
> Prepared 1 package in 306ms
> Installed 1 package in 0.69ms
>  + alibabacloud-darabonba-array==0.1.0
> ```

When dealing with older pip packages, encountering a warning like 'classifiers' should be a list, got type 'tuple' is acceptable. However, the program should still properly iterate over the tuple. After all, in early Python development, the distinction between tuple and list usage was not strictly enforced, and these packages are likely remnants from before the pyproject.toml specification.

---

_Comment by @charliermarsh on 2025-04-10 14:13_

I would need a minimal reproduction in order to help -- sorry! I can't reproduce or help based on the above alone.

---

_Label `bug` removed by @charliermarsh on 2025-04-10 14:13_

---

_Label `needs-mre` added by @charliermarsh on 2025-04-10 14:13_

---

_Comment by @yjc980121 on 2025-04-12 04:38_

> I would need a minimal reproduction in order to help -- sorry! I can't reproduce or help based on the above alone.

i use   `uv add` , but you use   `uv pip install `

---

_Closed by @charliermarsh on 2025-07-11 02:23_

---
