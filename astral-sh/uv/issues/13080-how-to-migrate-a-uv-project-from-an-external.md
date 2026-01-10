---
number: 13080
title: How to migrate a uv project from an external network Ubuntu server to an offline internal network Ubuntu environment?
type: issue
state: open
author: phoenixor
labels:
  - question
assignees: []
created_at: 2025-04-24T03:22:25Z
updated_at: 2025-12-18T00:14:14Z
url: https://github.com/astral-sh/uv/issues/13080
synced_at: 2026-01-10T01:25:28Z
---

# How to migrate a uv project from an external network Ubuntu server to an offline internal network Ubuntu environment?

---

_Issue opened by @phoenixor on 2025-04-24 03:22_

### Question

How to migrate a uv project from an external network Ubuntu server to an offline internal network Ubuntu environment? The project paths are exactly the same, both in /app/api_server_test/. In the external network, a virtual environment was created using uv with Python 3.11.12, and all dependent packages were installed after activation. The entire project was compressed into a 7zip file and copied to the internal network server for decompression. In the external network, executing "uv run main.py" runs normally, but in the internal network, the following error occurs:

error: Querying Python at `/app/api_server_test/.venv/bin/python3` failed with exit status exit status: 127

[stderr]
/app/api_server_test/.venv/bin/python3: error while loading shared libraries: /app/api_server_test/.venv/bin/../lib/libpython3.11.so.1.0: cannot open shared object file: No such file or directory
------------------------------ 
external network Platform: Ubuntu 20.04.6 LTS
offline internal network platform: Ubuntu 22.04.3 LTS


### Platform

_No response_

### Version

uv 0.6.16

---

_Label `question` added by @phoenixor on 2025-04-24 03:22_

---

_Comment by @konstin on 2025-04-25 10:53_

Copying venvs is usually too brittle, copying the uv cache instead and rerunning the venv creation and package installation on the target machine generally works better.

---

_Comment by @HuangChiEn on 2025-12-16 01:10_

> Copying venvs is usually too brittle, copying the uv cache instead and rerunning the venv creation and package installation on the target machine generally works better.

Hello, i also meet the same situation.
Does copying venvs is fine, or copy other files (pyproject.toml, .python-version) are also required ? 

( In my case, the server is completely **offline**, it's impossible to reinstall the python package ; so i want to make sure **copy** what kind of files can help me reproduce the enviroment.

Any suggestion are appreciated !! 


---

_Comment by @zanieb on 2025-12-16 01:14_

You can copy the virtual environment by itself, it depends how you want to run commands in the environment — i.e., if you want to use `uv run` you might want the project, etc.

In general, you can also follow a strategy like we do in our multistage Docker file example https://github.com/astral-sh/uv-docker-example/blob/fa761744819c05f4ce207bde1f0a396f6be3915f/standalone.Dockerfile#L35-L39 and just copy across a managed Python version along with the environment. It just needs to be at the same absolute path.

---

_Comment by @HuangChiEn on 2025-12-18 00:14_

> You can copy the virtual environment by itself, it depends how you want to run commands in the environment — i.e., if you want to use `uv run` you might want the project, etc.
> 
> In general, you can also follow a strategy like we do in our multistage Docker file example https://github.com/astral-sh/uv-docker-example/blob/fa761744819c05f4ce207bde1f0a396f6be3915f/standalone.Dockerfile#L35-L39 and just copy across a managed Python version along with the environment. It just needs to be at the same absolute path.

i see, TKS for your quick reply!!
As your suggestion, Dockerfile COPY venv seems a promise solution ~

---
