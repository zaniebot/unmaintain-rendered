---
number: 2013
title: "uv doesn't install flask in conda"
type: issue
state: closed
author: matt-sd-watson
labels:
  - needs-mre
assignees: []
created_at: 2024-02-27T13:15:20Z
updated_at: 2024-04-08T02:14:11Z
url: https://github.com/astral-sh/uv/issues/2013
synced_at: 2026-01-10T01:23:10Z
---

# uv doesn't install flask in conda

---

_Issue opened by @matt-sd-watson on 2024-02-27 13:15_

The following commands were used to create a conda environment for a flask application:

```
conda create --name app_test python=3.9
conda activate app_test
uv pip install -r requirements.txt
```
with the attached requirements file. 

[requirements.txt](https://github.com/astral-sh/uv/files/14420011/requirements.txt)

After installation, flask was not found in the environment.





---

_Label `needs-mre` added by @charliermarsh on 2024-02-27 16:19_

---

_Comment by @charliermarsh on 2024-02-27 16:25_

Unfortunately I'm unable to repro this:

```
(app_test) uv on  main [$] is 📦 v0.1.11 via 🐍 v3.9.18 via 🦀 v1.76.0
❯ uv pip install requests
Resolved 5 packages in 118ms
Downloaded 1 package in 27ms
Installed 5 packages in 5ms
 + certifi==2024.2.2
 + charset-normalizer==3.3.2
 + idna==3.6
 + requests==2.31.0
 + urllib3==2.2.1
(app_test) uv on  main [$] is 📦 v0.1.11 via 🐍 v3.9.18 via 🦀 v1.76.0
❯ python
Python 3.9.18 | packaged by conda-forge | (main, Dec 23 2023, 16:35:41)
[Clang 16.0.6 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> umport
KeyboardInterrupt
>>> import requests
>>>
```

Would you share the output of `uv pip install -r requirements.txt --verbose --no-cache --reinstall`?

---

_Comment by @charliermarsh on 2024-04-08 02:14_

Closing for now but happy to revisit in light of more information.

---

_Closed by @charliermarsh on 2024-04-08 02:14_

---
