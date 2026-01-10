```yaml
number: 10223
title: Add loongarch64 to supported Python platform tags
type: pull_request
state: merged
author: wojiushixiaobai
labels:
  - compatibility
assignees: []
merged: true
base: main
head: patch_loong64
created_at: 2024-12-30T02:07:04Z
updated_at: 2024-12-31T03:00:37Z
url: https://github.com/astral-sh/uv/pull/10223
synced_at: 2026-01-10T11:44:39Z
```

# Add loongarch64 to supported Python platform tags

---

_Pull request opened by @wojiushixiaobai on 2024-12-30 02:07_

_No description provided._

---

_Comment by @charliermarsh on 2024-12-30 02:31_

Have you tested this on an applicable machine?

---

_Comment by @wojiushixiaobai on 2024-12-30 04:50_

@charliermarsh 
Sorry, I forgot to add the description,
I'm testing the functionality of the loongarch64 architecture, I'll add the results to the reply once the test passes.

---

_Comment by @charliermarsh on 2024-12-30 15:28_

Okay thanks. Just comment here when you've had a chance to test + updated the summary!

---

_Label `compatibility` added by @charliermarsh on 2024-12-30 15:28_

---

_Comment by @wojiushixiaobai on 2024-12-31 02:45_

@charliermarsh 
Test passed successfully.

```sh
root@85348a55bc91:/opt/test# uv init
Initialized project `test`
root@85348a55bc91:/opt/test# uv add django
Using CPython 3.12.8 interpreter at: /usr/local/bin/python3.12
Creating virtual environment at: .venv
Resolved 5 packages in 986ms
Prepared 3 packages in 1.23s
Installed 3 packages in 281ms
 + asgiref==3.8.1
 + django==5.1.4
 + sqlparse==0.5.3
root@85348a55bc91:/opt/test# uv python find --verbose
DEBUG uv 0.5.13
DEBUG Found project root: `/opt/test`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/opt/test/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Searching for Python 3.12 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.8-linux-loongarch64-gnu` at `/opt/test/.venv/bin/python3` (virtual environment)
/opt/test/.venv/bin/python3
```

![image](https://github.com/user-attachments/assets/4220a871-6c69-4b12-9d4b-f4761e70bdb0)



---

_Comment by @charliermarsh on 2024-12-31 02:57_

Thanks!

---

_Merged by @charliermarsh on 2024-12-31 03:00_

---

_Closed by @charliermarsh on 2024-12-31 03:00_

---
