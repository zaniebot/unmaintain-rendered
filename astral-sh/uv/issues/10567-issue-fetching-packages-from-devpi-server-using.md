```yaml
number: 10567
title: Issue Fetching Packages from Devpi Server Using uv and --index-url
type: issue
state: closed
author: ALEXRG27
labels:
  - needs-mre
assignees: []
created_at: 2025-01-13T15:21:41Z
updated_at: 2025-01-31T18:18:54Z
url: https://github.com/astral-sh/uv/issues/10567
synced_at: 2026-01-12T16:00:16Z
```

# Issue Fetching Packages from Devpi Server Using uv and --index-url

---

_@ALEXRG27_

Description:
I am hosting all my packages on a Devpi server, and I have configured pip.ini as shown below to manage the sources. However, when I try to install the experimentbase package, it does not fetch correctly. While I can see the information on the index, the installation fails. Below is the relevant configuration and logs:

pip.ini Configuration:
[global]
index-url = http://<ip>:80/nightly/dist
trusted-host = <ip>

[install]
extra-index-url = 
    http://<ip>:80/releases/dist
    https://pypi.org/simple
Steps to Reproduce:

Set up a virtual environment using uv.
Activate the environment: .\env_uv\Scripts\activate.
Try installing the package:
uv pip install experimentbase --index-url=http://<ip>/nightly/dist -v.
Logs:
(env_uv) PS C:\jenkins\workspace\New folder> uv venv env_uv
Using CPython 3.11.1 interpreter at: C:\Program Files\Python311\python.exe
Creating virtual environment at: env_uv
Activate with: env_uv\Scripts\activate
(env_uv) PS C:\jenkins\workspace\New folder> .\env_uv\Scripts\activate
(env_uv) PS C:\jenkins\workspace\New folder> uv pip install experimentbase --index-url=http://<ip>/nightly/dist -v    
DEBUG uv 0.5.18 (27d1bad55 2025-01-11)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.11.1-windows-x86_64-none` at `C:\jenkins\workspace\New folder\env_uv\Scripts\python.exe` (active virtual environment)
Using Python 3.11.1 environment at: env_uv
DEBUG Acquired lock for `env_uv`
DEBUG At least one requirement is not satisfied: experimentbase
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.1
DEBUG Solving with target Python version: >=3.11.1
DEBUG Adding direct dependency: experimentbase*
DEBUG Found stale response for: http://<ip>/nightly/dist/experimentbase/
DEBUG Sending revalidation request for: http://<ip>/nightly/dist/experimentbase/
DEBUG Found modified response for: http://<ip>/nightly/dist/experimentbase/
WARN Skipping file for experimentbase: 
WARN Skipping file for experimentbase:
WARN Skipping file for experimentbase:
WARN Skipping file for experimentbase: +searchhelp
WARN Skipping file for experimentbase: +status
WARN Skipping file for experimentbase: 0.0.1.303.dev0+g9b572e0ac7
WARN Skipping file for experimentbase: dist
WARN Skipping file for experimentbase: dist
WARN Skipping file for experimentbase: experimentbase
WARN Skipping file for experimentbase: experimentbase
WARN Skipping file for experimentbase: latest
WARN Skipping file for experimentbase: nightly
WARN Skipping file for experimentbase: pypi
DEBUG Searching for a compatible version of experimentbase (*)
DEBUG No compatible version found for: experimentbase
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of experimentbase and you require experimentbase, we can conclude that your requirements   
      are unsatisfiable.
DEBUG Released lock at `C:\workspace\env_uv\.lock`
Expected Behavior:
The experimentbase package should be installed from the specified Devpi server.

Actual Behavior:
The package installation fails with multiple warnings related to missing files and no compatible version found.

I would appreciate it if you could investigate why the packages are not being fetched from the Devpi server. Additionally, I would be grateful for any recommendations regarding configuration changes or troubleshooting steps that might help resolve the issue. The installation also fails when specifying the version:
uv pip install experimentbase==0.0.1.303.dev0+g9b572e0ac7 --index-url=http://<ip>/nightly/dist -v

---

_Comment by @charliermarsh on 2025-01-13 15:32_

We don't read from `pip.ini`. Are you expecting it to? Separately, it looks like there's some mismatch between your local registry and the URL you're passing in. These should be filenames:

```
WARN Skipping file for experimentbase:
WARN Skipping file for experimentbase:
WARN Skipping file for experimentbase:
WARN Skipping file for experimentbase: +searchhelp
WARN Skipping file for experimentbase: +status
WARN Skipping file for experimentbase: 0.0.1.303.dev0+g9b572e0ac7
WARN Skipping file for experimentbase: dist
WARN Skipping file for experimentbase: dist
WARN Skipping file for experimentbase: experimentbase
WARN Skipping file for experimentbase: experimentbase
WARN Skipping file for experimentbase: latest
WARN Skipping file for experimentbase: nightly
WARN Skipping file for experimentbase: pypi
```

---

_Label `needs-mre` added by @charliermarsh on 2025-01-13 15:32_

---

_Comment by @ALEXRG27 on 2025-01-13 15:48_

I was providing uv with the --url defined in pip.ini I was not expecting uv to use pip.ini
But I was expecting both flows to work
PS C:\jenkins\workspace\New folder> python -m venv env
PS C:\jenkins\workspace\New folder> .\env\Scripts\activate
(env) PS C:\jenkins\workspace\New folder> pip install experimentbase
Looking in indexes: http://<ip>:80/nightly/dist, http://<ip>:80/releasebeta/dist, https://pypi.org/simple
Collecting experimentbase
  Using cached http://<ip>/nightly/dist/%2Bf/b42/9ca544189b0c9/experimentbase-0.0.1.303.dev0%2Bg9b572e0ac7-py3-none-any.whl (4.6 kB)
Installing collected packages: experimentbase
Successfully installed experimentbase-0.0.1.303.dev0+g9b572e0ac7

Doing exaclty this with uv venv env_uv , activate, and then uv pip install specifying the url of my package server, It it not fetching the package when it is there. I was expecting to work the same as pip.

<img width="910" alt="Image" src="https://github.com/user-attachments/assets/2a32823b-544f-4035-bbe1-7738d7d8f3f8" />

---

_Comment by @charliermarsh on 2025-01-13 15:49_

I'll need something I can run myself to reproduce, like a Git repo or a Dockerfile or a series of specific commands.

---

_Comment by @zanieb on 2025-01-13 16:05_

Are  you using the `/simple` suffix on your devpi index URL?

---

_Comment by @ALEXRG27 on 2025-01-14 09:50_

Thank you so much, Zanieb, for pointing that out! It turns out that using /simple doesn’t work, but /+simple does. I’m still unsure why this is necessary at all.

Here’s an example of the issue and how it was resolved:

![Image](https://github.com/user-attachments/assets/0380daca-f346-40c0-a001-d0e7666627f9)



---

_Comment by @zanieb on 2025-01-31 17:33_

Thanks for following up! We are tracking support for the non-simple index in https://github.com/astral-sh/uv/issues/4907

---

_Closed by @zanieb on 2025-01-31 18:18_

---
