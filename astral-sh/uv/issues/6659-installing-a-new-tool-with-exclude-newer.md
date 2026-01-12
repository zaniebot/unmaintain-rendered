```yaml
number: 6659
title: Installing a new tool with --exclude-newer downgrades another previously installed tool
type: issue
state: closed
author: olutra
labels:
  - bug
assignees: []
created_at: 2024-08-26T21:25:40Z
updated_at: 2024-08-26T22:08:52Z
url: https://github.com/astral-sh/uv/issues/6659
synced_at: 2026-01-12T15:59:06Z
```

# Installing a new tool with --exclude-newer downgrades another previously installed tool

---

_@olutra_

Installing a new tool (docker-compose) with `--exclude-newer` option downgrades another previously installed tool (yt-dlp) if `upgrade --all` command is called.
python 3.12 is used implicitly for the yt-dlp tool.
python 3.11 is used explicitly for the docker-compose tool, because the tool doesn't support python 3.12.

It appears that the certifi package version restriction applies to both tools, although it shouldn't.

#### uv platform
    Linux Mint 21.2
    Ubuntu 24.04 LTS

#### uv --version
    uv 0.3.3

<details>
  <summary>Commands</summary>

    tester@testbox:~$ uv tool list
    No tools installed

    tester@testbox:~$ uv tool install yt-dlp
    Resolved 10 packages in 16ms
    Installed 10 packages in 52ms
    + brotli==1.1.0
    + certifi==2024.7.4
    + charset-normalizer==3.3.2
    + idna==3.8
    + mutagen==1.47.0
    + pycryptodomex==3.20.0
    + requests==2.32.3
    + urllib3==2.2.2
    + websockets==13.0
    + yt-dlp==2024.8.6
    Installed 1 executable: yt-dlp

    tester@testbox:~$ uv tool upgrade --all
    Nothing to upgrade

    ester@testbox:~$ uv tool list
    yt-dlp v2024.8.6
    - yt-dlp

    tester@testbox:~$ uv tool install docker-compose --python 3.11 --exclude-newer 2023-01-01
    Resolved 26 packages in 376ms
    Installed 26 packages in 34ms
    + attrs==22.2.0
    + bcrypt==4.0.1
    + certifi==2022.12.7
    + cffi==1.15.1
    + charset-normalizer==2.1.1
    + cryptography==38.0.4
    + distro==1.8.0
    + docker==6.0.1
    + docker-compose==1.29.2
    + dockerpty==0.4.1
    + docopt==0.6.2
    + idna==3.4
    + jsonschema==3.2.0
    + packaging==22.0
    + paramiko==2.12.0
    + pycparser==2.21
    + pynacl==1.5.0
    + pyrsistent==0.19.3
    + python-dotenv==0.21.0
    + pyyaml==5.4.1
    + requests==2.28.1
    + setuptools==65.6.3
    + six==1.16.0
    + texttable==1.6.7
    + urllib3==1.26.13
    + websocket-client==0.59.0
    Installed 1 executable: docker-compose  

    tester@testbox:~$ uv tool list
    docker-compose v1.29.2
    - docker-compose
    yt-dlp v2024.8.6
    - yt-dlp

    tester@testbox:~$ uv tool upgrade --all
    Updated yt-dlp v2024.8.6 -> v2023.10.13
    - certifi==2024.7.4
    + certifi==2022.12.7
    - charset-normalizer==3.3.2
    - idna==3.8
    - requests==2.32.3
    - urllib3==2.2.2
    - yt-dlp==2024.8.6
    + yt-dlp==2023.10.13
    Installed 1 executable: yt-dlp

    tester@testbox:~$ uv tool list
    docker-compose v1.29.2
    - docker-compose
    yt-dlp v2023.10.13
    - yt-dlp

</details>

---

_Comment by @charliermarsh on 2024-08-26 21:27_

Interesting! I will take a look, thank you.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-26 21:32_

---

_Label `bug` added by @charliermarsh on 2024-08-26 21:44_

---

_Closed by @charliermarsh on 2024-08-26 22:08_

---

_Closed by @charliermarsh on 2024-08-26 22:08_

---
