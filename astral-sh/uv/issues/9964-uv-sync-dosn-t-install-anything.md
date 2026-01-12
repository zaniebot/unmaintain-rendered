```yaml
number: 9964
title: "uv sync dosn't install anything"
type: issue
state: closed
author: cjdxhjj
labels:
  - needs-mre
assignees: []
created_at: 2024-12-17T11:03:04Z
updated_at: 2024-12-18T07:36:25Z
url: https://github.com/astral-sh/uv/issues/9964
synced_at: 2026-01-12T16:00:03Z
```

# uv sync dosn't install anything

---

_@cjdxhjj_

![Image](https://github.com/user-attachments/assets/780fb5f3-6aff-4395-b175-703b086acd75)


---

_Comment by @cjdxhjj on 2024-12-17 11:12_

i use uv sync first and run the app, it report no module found, then i check the sitepackages, and found nothing
then i use uv pip install, 
![Image](https://github.com/user-attachments/assets/af2cc3af-ed5d-4ed6-abdc-4d59d057a87c)

![Image](https://github.com/user-attachments/assets/e4050ec1-8aaa-4cbb-a570-39084f282432)


---

_Comment by @konstin on 2024-12-17 14:16_

Can you share `uv sync -v`? Please use a text copy (e.g. https://gist.github.com/) instead of screenshots.

---

_Label `needs-mre` added by @konstin on 2024-12-17 14:16_

---

_Comment by @zanieb on 2024-12-17 14:19_

(See #9452 for general guidelines on how to report issues)

---

_Comment by @cjdxhjj on 2024-12-18 01:03_

uv 0.5.8
PRETTY_NAME="Ubuntu 22.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.5 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy

---

_Comment by @cjdxhjj on 2024-12-18 01:04_

![Image](https://github.com/user-attachments/assets/8fdd7c73-19e4-4a5d-9df8-1b45f13e64f1)


---

_Comment by @cjdxhjj on 2024-12-18 01:06_

[log.txt](https://github.com/user-attachments/files/18173091/log.txt)


---

_Comment by @cjdxhjj on 2024-12-18 01:07_

@konstin @zanieb 

---

_Comment by @zanieb on 2024-12-18 01:24_

Have you configured a build system? https://docs.astral.sh/uv/concepts/projects/config/#build-systems


---

_Comment by @zanieb on 2024-12-18 01:25_

Please take more time with your responses, you need to explain _way_ more or we cannot help you.

---

_Comment by @cjdxhjj on 2024-12-18 01:29_

the pyproject.toml is here
[pyproject.txt](https://github.com/user-attachments/files/18173327/pyproject.txt)



---

_Comment by @cjdxhjj on 2024-12-18 01:30_

yes, it was config as 
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

---

_Comment by @zanieb on 2024-12-18 02:21_

It looks like you're in a workspace member and we're syncing the root?

```
DEBUG Found project root: `/data0/langflow/src/backend/base`
DEBUG Found workspace root: `/data0/langflow`
```

Are the packages in `/data0/langflow/.venv`?

---

_Comment by @cjdxhjj on 2024-12-18 02:24_

![Image](https://github.com/user-attachments/assets/fa101974-c086-422d-8c33-af9c13556374)
/data0/langflow/.venv exisit, but the package is empty, and that was the wrong path

---

_Comment by @cjdxhjj on 2024-12-18 02:25_

![Image](https://github.com/user-attachments/assets/c398634b-d578-4800-8ed5-59048236d2bc)
the project root path is not the python project root

---

_Comment by @cjdxhjj on 2024-12-18 02:31_

![Image](https://github.com/user-attachments/assets/87d6d630-be70-4c65-b08f-f0ef660d11a8)
![Image](https://github.com/user-attachments/assets/7eca790f-a583-48b0-8308-4a80b87fa8b7)
![Image](https://github.com/user-attachments/assets/fb6a462c-d5ab-4300-8347-79d5816f0935)


---

_Comment by @cjdxhjj on 2024-12-18 02:51_

Sorry, I made a mistake。 the /data0/langflow package installed. the machine is another one
![Image](https://github.com/user-attachments/assets/03d1fc5f-0669-47d5-9fd5-c726a947682f)


---

_Comment by @cjdxhjj on 2024-12-18 02:52_

but the package install on the wrong path.

---

_Comment by @zanieb on 2024-12-18 03:15_

What's the `pyproject.toml` content at `/data0/langflow`?

---

_Comment by @cjdxhjj on 2024-12-18 03:18_

[pyproject.toml.txt](https://github.com/user-attachments/files/18174289/pyproject.toml.txt)


---

_Comment by @cjdxhjj on 2024-12-18 03:24_

the repo is here https://github.com/langflow-ai/langflow

---

_Comment by @zanieb on 2024-12-18 03:29_

This is working as intended. The project is configured as a workspace member https://github.com/langflow-ai/langflow/blob/5a8d73c5b29d8f496bc02d8ef2d3a6dfe3c2e0c0/pyproject.toml#L5-L6 — it's intentional that the workspace root us used.

---

_Comment by @zanieb on 2024-12-18 03:30_

See also https://docs.astral.sh/uv/concepts/projects/workspaces/

---

_Comment by @cjdxhjj on 2024-12-18 07:36_

thanks @zanieb , @konstin 

---

_Closed by @cjdxhjj on 2024-12-18 07:36_

---
